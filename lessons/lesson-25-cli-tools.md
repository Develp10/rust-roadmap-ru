# Урок 25. CLI-приложения на Rust

## Теория

### Почему Rust для CLI

Rust — отличный выбор для CLI-инструментов:

- быстрый старт и низкое потребление памяти
- статически собранный бинарник без рантайма
- кроссплатформенность из коробки
- богатая экосистема (`clap`, `indicatif`, `anyhow`, `tracing`)

Известные CLI на Rust: `ripgrep`, `bat`, `fd`, `exa`/`eza`, `zoxide`, `hyperfine`, `tokei`.

### clap — парсинг аргументов

```toml
[dependencies]
clap = { version = "4", features = ["derive"] }
```

```rust
use clap::Parser;

#[derive(Parser)]
#[command(version, about = "Greet someone")]
struct Cli {
    /// имя получателя
    name: String,
    /// сколько раз поприветствовать
    #[arg(short, long, default_value_t = 1)]
    count: u8,
}

fn main() {
    let cli = Cli::parse();
    for _ in 0..cli.count {
        println!("Hello, {}!", cli.name);
    }
}
```

### Подкоманды

```rust
#[derive(Parser)]
struct Cli {
    #[command(subcommand)]
    cmd: Cmd,
}

#[derive(clap::Subcommand)]
enum Cmd {
    Add { name: String },
    Remove { id: u64 },
    List,
}
```

### anyhow и thiserror

```rust
use anyhow::{Context, Result};

fn run(path: &str) -> Result<()> {
    let data = std::fs::read_to_string(path)
        .with_context(|| format!("reading {}", path))?;
    println!("{}", data);
    Ok(())
}
```

### Логирование: tracing

```rust
use tracing::{info, warn};
use tracing_subscriber::EnvFilter;

fn main() {
    tracing_subscriber::fmt()
        .with_env_filter(EnvFilter::from_default_env())
        .init();
    info!(name = "world", "starting");
}
```

```bash
RUST_LOG=info cargo run
```

### Цвета и интерактив

| Крейт | Назначение |
|-------|-----------|
| `colored` / `owo-colors` | ANSI-цвета |
| `indicatif` | прогресс-бары |
| `dialoguer` | интерактивные подсказки |
| `crossterm` | TUI/raw mode |
| `ratatui` | полноценные терминальные UI |

```rust
use indicatif::{ProgressBar, ProgressStyle};

let bar = ProgressBar::new(100);
bar.set_style(ProgressStyle::with_template("{bar:40} {pos}/{len}").unwrap());
for _ in 0..100 {
    bar.inc(1);
    std::thread::sleep(std::time::Duration::from_millis(10));
}
bar.finish();
```

### Конфиг и переменные окружения

```rust
#[derive(serde::Deserialize)]
struct Cfg { host: String, port: u16 }

let cfg: Cfg = config::Config::builder()
    .add_source(config::File::with_name("config"))
    .add_source(config::Environment::with_prefix("APP"))
    .build()?
    .try_deserialize()?;
```

### Кроссплатформенные пути

```rust
use std::path::PathBuf;
let home = dirs::home_dir().unwrap();
let cfg: PathBuf = home.join(".config").join("myapp").join("config.toml");
```

### Сборка и распространение

```bash
cargo build --release
cargo install --path .
```

Для статической сборки на Linux: `x86_64-unknown-linux-musl`.
Для CI и релизов: `cargo-dist`, GitHub Actions, `cross` для разных архитектур.

## Практика

### 1. wc-клон

```rust
use clap::Parser;
use std::io::{BufRead, BufReader};

#[derive(Parser)]
struct Cli {
    files: Vec<std::path::PathBuf>,
    #[arg(short, long)] lines: bool,
    #[arg(short, long)] words: bool,
    #[arg(short, long)] bytes: bool,
}

fn main() -> anyhow::Result<()> {
    let cli = Cli::parse();
    for p in &cli.files {
        let f = std::fs::File::open(p)?;
        let mut l = 0; let mut w = 0; let mut b = 0;
        for line in BufReader::new(f).lines() {
            let line = line?;
            l += 1;
            w += line.split_whitespace().count();
            b += line.len() + 1;
        }
        println!("{} {} {} {}", l, w, b, p.display());
    }
    Ok(())
}
```

### 2. todo-CLI с подкомандами

```rust
#[derive(clap::Parser)]
struct Cli { #[command(subcommand)] cmd: Cmd }

#[derive(clap::Subcommand)]
enum Cmd { Add { text: String }, Done { id: usize }, List }

fn main() -> anyhow::Result<()> {
    let cli = Cli::parse();
    let path = dirs::home_dir().unwrap().join(".todo.json");
    let mut items: Vec<(bool, String)> = if path.exists() {
        serde_json::from_str(&std::fs::read_to_string(&path)?)?
    } else { Vec::new() };
    match cli.cmd {
        Cmd::Add { text } => items.push((false, text)),
        Cmd::Done { id } => { if let Some(it) = items.get_mut(id) { it.0 = true; } }
        Cmd::List => for (i, (d, t)) in items.iter().enumerate() {
            println!("{} [{}] {}", i, if *d { "x" } else { " " }, t);
        }
    }
    std::fs::write(&path, serde_json::to_string_pretty(&items)?)?;
    Ok(())
}
```

### 3. Прогресс-бар скачивания

```rust
use indicatif::{ProgressBar, ProgressStyle};
use std::io::{Read, Write};

fn download(url: &str, out: &str) -> anyhow::Result<()> {
    let resp = reqwest::blocking::get(url)?;
    let total = resp.content_length().unwrap_or(0);
    let bar = ProgressBar::new(total);
    bar.set_style(ProgressStyle::with_template("{bar:40} {bytes}/{total_bytes}").unwrap());
    let mut file = std::fs::File::create(out)?;
    let mut src = resp;
    let mut buf = [0u8; 8192];
    loop {
        let n = src.read(&mut buf)?;
        if n == 0 { break; }
        file.write_all(&buf[..n])?;
        bar.inc(n as u64);
    }
    bar.finish();
    Ok(())
}
```

### 4. TUI на ratatui

```rust
use ratatui::{prelude::*, widgets::*};
fn render(f: &mut Frame) {
    let block = Block::default().title("Hello").borders(Borders::ALL);
    f.render_widget(block, f.size());
}
```

### 5. Чтение stdin

```rust
use std::io::{self, BufRead};
fn main() {
    let stdin = io::stdin();
    for line in stdin.lock().lines() {
        let l = line.unwrap();
        println!("got: {}", l);
    }
}
```

### 6. Цветной вывод

```rust
use owo_colors::OwoColorize;
println!("{} {}", "OK".green().bold(), "everything works".dimmed());
```

## Тест

1. Какой крейт используется для парсинга аргументов с derive-макросами?
2. Что делает атрибут `#[command(subcommand)]`?
3. Чем удобен `anyhow::Context::with_context`?
4. Какой переменной окружения управляется уровень логов `tracing`?
5. Какой крейт для прогресс-баров?
6. Как сделать статически слинкованный бинарник под Linux?
7. Какой крейт для интерактивных подсказок (выбор/ввод)?
8. Как получить путь к домашней директории кроссплатформенно?
9. Какой крейт для TUI?
10. Какая команда устанавливает текущий проект как локальный CLI?

---

### Ответы

1. `clap` (с feature `derive`).
2. Помечает поле, в которое будет распарсена выбранная подкоманда (enum c `#[derive(Subcommand)]`).
3. Добавляет контекст (сообщение) к ошибке, чтобы получить понятную трассировку причин.
4. `RUST_LOG` (через `EnvFilter::from_default_env()`).
5. `indicatif`.
6. Использовать target `x86_64-unknown-linux-musl` (`rustup target add` + `cargo build --target ...`).
7. `dialoguer`.
8. `dirs::home_dir()` (или `directories::ProjectDirs`).
9. `ratatui`.
10. `cargo install --path .`.
