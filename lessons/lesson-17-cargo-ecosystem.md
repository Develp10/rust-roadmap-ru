# Урок 17. Cargo и экосистема

`cargo` — это менеджер сборки, пакетный менеджер и тест-раннер в одном флаконе. Понимание Cargo превращает Rust из «языка, на котором я пишу функции» в полноценный инструмент разработки приложений и библиотек.

## Теория

### Что такое крейт и пакет

- **Крейт (crate)** — единица компиляции. Бывает двух видов: библиотечный (`lib.rs`) и бинарный (`main.rs` или файл в `src/bin/`).
- **Пакет (package)** — то, что описано одним `Cargo.toml`. Может содержать максимум один библиотечный крейт и сколько угодно бинарников.

### Структура `Cargo.toml`

```toml
[package]
name = "myapp"
version = "0.1.0"
edition = "2021"
authors = ["Имя <mail@example.com>"]
description = "Краткое описание"
license = "MIT OR Apache-2.0"
repository = "https://github.com/me/myapp"

[dependencies]
serde = { version = "1", features = ["derive"] }
tokio = { version = "1", features = ["full"] }

[dev-dependencies]
criterion = "0.5"

[build-dependencies]
cc = "1"

[features]
default = ["json"]
json = ["dep:serde_json"]
```

Запомним:

- `[dependencies]` — нужны при обычной сборке.
- `[dev-dependencies]` — только для тестов и примеров.
- `[build-dependencies]` — для скрипта `build.rs`.
- `[features]` — флаги, включающие куски кода и зависимости.

### Версии и SemVer

Cargo следует SemVer: `1.2.3` совместим со всем диапазоном `>=1.2.3, <2.0.0`. Запись `"1"` означает «совместимо с 1.x.y». Для жёстких ограничений используют `= "1.2.3"`. Файл `Cargo.lock` фиксирует точные версии — для бинарников его коммитят, для библиотек обычно нет.

### Профили сборки

```toml
[profile.dev]
opt-level = 0
debug = true

[profile.release]
opt-level = 3
lto = "thin"
codegen-units = 1
strip = true
```

- `cargo build` — профиль `dev`, быстрая сборка, без оптимизаций.
- `cargo build --release` — профиль `release`, оптимизированный бинарник.
- `cargo test` — профиль `test` (наследуется от `dev`).
- `cargo bench` — профиль `bench` (наследуется от `release`).

LTO (link-time optimization) и `codegen-units = 1` дают лучшую производительность ценой времени сборки.

### Workspace

Когда проект разрастается, его делят на крейты и собирают в workspace:

```
my_project/
├── Cargo.toml          # workspace root
├── api/
│   └── Cargo.toml
├── core/
│   └── Cargo.toml
└── cli/
    └── Cargo.toml
```

Корневой `Cargo.toml`:

```toml
[workspace]
members = ["api", "core", "cli"]
resolver = "2"

[workspace.dependencies]
serde = { version = "1", features = ["derive"] }
tokio = { version = "1", features = ["full"] }
```

Дочерний `api/Cargo.toml`:

```toml
[package]
name = "api"
version = "0.1.0"
edition = "2021"

[dependencies]
core = { path = "../core" }
serde = { workspace = true }
```

`workspace = true` наследует версию и фичи из корня. Это лучший способ синхронизировать зависимости в больших проектах.

### Features (флаги компиляции)

Features включают опциональные зависимости и куски кода:

```toml
[dependencies]
serde_json = { version = "1", optional = true }

[features]
default = []
json = ["dep:serde_json"]
```

В коде:

```rust
#[cfg(feature = "json")]
pub fn to_json(value: &str) -> String {
    serde_json::to_string(value).unwrap()
}
```

Сборка с фичей: `cargo build --features json`. Без фичи функция просто не существует.

**Важно:** features в Cargo *аддитивные* — если два крейта включают одну фичу зависимости, они получают объединение. Никогда не делайте features, которые «отключают» поведение, — это ломает кооперацию крейтов.

### Полезные подкоманды Cargo

- `cargo check` — быстрая проверка типов без генерации кода.
- `cargo clippy` — линтер, найдёт сотни типичных проблем.
- `cargo fmt` — форматирование по стандарту.
- `cargo doc --open` — собирает и открывает документацию.
- `cargo tree` — дерево зависимостей.
- `cargo update` — пересчитать `Cargo.lock`.
- `cargo outdated` (плагин) — какие пакеты устарели.
- `cargo audit` (плагин) — проверка известных уязвимостей по RustSec.
- `cargo expand` (плагин) — раскрытие макросов.
- `cargo machete` (плагин) — поиск неиспользуемых зависимостей.

### Публикация на crates.io

1. Зарегистрироваться на crates.io и получить токен: `cargo login <token>`.
2. Заполнить в `Cargo.toml`: `description`, `license`, `repository`, `readme`, `keywords`, `categories`.
3. `cargo publish --dry-run` — посмотреть, что попадёт в архив.
4. `cargo publish`.

Важные детали:

- Удалять опубликованные версии нельзя, можно только yank.
- Версии монотонны: `0.1.0` -> `0.1.1` или `0.2.0`.
- Имя крейта уникально на crates.io.

## Практика

Создадим небольшой workspace из двух крейтов: библиотеки `mathx` с математикой и бинарника `mathx-cli` с CLI-обёрткой.

Структура:

```
mathx-workspace/
├── Cargo.toml
├── mathx/
│   ├── Cargo.toml
│   └── src/lib.rs
└── mathx-cli/
    ├── Cargo.toml
    └── src/main.rs
```

Корневой `Cargo.toml`:

```toml
[workspace]
members = ["mathx", "mathx-cli"]
resolver = "2"

[workspace.dependencies]
clap = { version = "4", features = ["derive"] }
```

`mathx/Cargo.toml`:

```toml
[package]
name = "mathx"
version = "0.1.0"
edition = "2021"
```

`mathx/src/lib.rs`:

```rust
pub fn factorial(n: u64) -> u64 {
    (1..=n).product()
}

pub fn gcd(a: u64, b: u64) -> u64 {
    if b == 0 { a } else { gcd(b, a % b) }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn fact_zero() { assert_eq!(factorial(0), 1); }
    #[test]
    fn fact_five() { assert_eq!(factorial(5), 120); }
    #[test]
    fn gcd_basic() { assert_eq!(gcd(12, 18), 6); }
}
```

`mathx-cli/Cargo.toml`:

```toml
[package]
name = "mathx-cli"
version = "0.1.0"
edition = "2021"

[dependencies]
mathx = { path = "../mathx" }
clap = { workspace = true }
```

`mathx-cli/src/main.rs`:

```rust
use clap::{Parser, Subcommand};

#[derive(Parser)]
#[command(name = "mathx", about = "Маленький математический CLI")]
struct Cli {
    #[command(subcommand)]
    cmd: Cmd,
}

#[derive(Subcommand)]
enum Cmd {
    Fact { n: u64 },
    Gcd { a: u64, b: u64 },
}

fn main() {
    let cli = Cli::parse();
    match cli.cmd {
        Cmd::Fact { n } => println!("{}", mathx::factorial(n)),
        Cmd::Gcd { a, b } => println!("{}", mathx::gcd(a, b)),
    }
}
```

Запуск:

```bash
cargo run -p mathx-cli -- fact 6        # 720
cargo run -p mathx-cli -- gcd 48 18     # 6
cargo test --workspace
```

## Тест

1. В чём разница между *крейтом* и *пакетом* в терминах Cargo?
2. Чем отличаются `[dependencies]` и `[dev-dependencies]`?
3. Что такое workspace и какую проблему он решает?
4. Почему features в Cargo должны быть «аддитивными»?
5. Что произойдёт, если попытаться повторно опубликовать ту же версию крейта на crates.io?

---

### Ответы

1. *Крейт* — единица компиляции (библиотека или бинарник). *Пакет* — то, что описано в одном `Cargo.toml`; пакет может содержать одну библиотеку и любое число бинарников, которые становятся отдельными крейтами.
2. `[dependencies]` собираются при обычной сборке и попадают в финальный артефакт. `[dev-dependencies]` нужны только для тестов, бенчмарков и примеров; в продакшен-бинарник они не попадают.
3. Workspace — это набор крейтов, собранных в одном репозитории и шарящих `target/` и `Cargo.lock`. Он решает задачи модульности больших проектов: ускоряет сборку, синхронизирует версии зависимостей через `[workspace.dependencies]` и упрощает совместное тестирование.
4. Один и тот же крейт может оказаться в графе зависимостей через несколько путей. Если features складывать (объединять), то итоговая компиляция корректна для всех потребителей. Если бы features могли «выключать» поведение, два потребителя одного крейта могли бы конфликтовать, и Cargo не смог бы выбрать единый набор флагов.
5. Cargo откажет: версии на crates.io неизменяемы. Удалить версию нельзя, можно только пометить её как yanked, чтобы её перестали брать в новые сборки. Для исправления нужно опубликовать новую версию.
