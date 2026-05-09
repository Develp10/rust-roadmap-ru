# Урок 10. Обработка ошибок: `Result`, `?`, `thiserror`, `anyhow`

## Теория

### Две категории ошибок

В Rust ошибки делятся на **восстановимые** (recoverable) и **невосстановимые** (unrecoverable).

- **Невосстановимые** — `panic!`. Программа аварийно завершается (или раскручивает стек). Используется только для багов и нарушения инвариантов: «такого не должно быть никогда».
- **Восстановимые** — `Result<T, E>`. Это ожидаемые ошибки бизнес-логики: файл не найден, парсинг не удался, сеть недоступна. Их обязаны обработать.

```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

`Result` — это просто enum. Никакой магии. Компилятор заставляет вас обработать оба варианта.

### Обработка `Result`

**`match`** — самый явный способ:

```rust
use std::fs::File;

let f = match File::open("data.txt") {
    Ok(file) => file,
    Err(e) => {
        eprintln!("ошибка открытия: {e}");
        return;
    }
};
```

**`unwrap`** и **`expect`** — паникуют при `Err`. Допустимо в прототипах, тестах и там, где ошибка действительно невозможна:

```rust
let f = File::open("config.toml").expect("config.toml обязан существовать");
```

**`?`** — оператор «протолкнуть ошибку наверх». Если `Result == Err`, функция немедленно возвращает `Err`. Если `Ok` — извлекает значение.

```rust
use std::io::Read;
use std::fs::File;

fn read_config() -> Result<String, std::io::Error> {
    let mut f = File::open("config.toml")?; // Err -> return Err
    let mut s = String::new();
    f.read_to_string(&mut s)?;
    Ok(s)
}
```

### Свои типы ошибок

Когда функция может фейлиться по-разному, делаем `enum`:

```rust
#[derive(Debug)]
enum AppError {
    Io(std::io::Error),
    Parse(std::num::ParseIntError),
    NotFound,
}

impl From<std::io::Error> for AppError {
    fn from(e: std::io::Error) -> Self { AppError::Io(e) }
}

impl From<std::num::ParseIntError> for AppError {
    fn from(e: std::num::ParseIntError) -> Self { AppError::Parse(e) }
}
```

`From` нужен, чтобы `?` автоматически конвертировал ошибки внутренних функций в ваш `AppError`.

### `thiserror` — для библиотек

В библиотеках вы хотите типизированные, документированные ошибки. `thiserror` пишет boilerplate за вас:

```rust
use thiserror::Error;

#[derive(Error, Debug)]
pub enum DbError {
    #[error("соединение разорвано")]
    Disconnected,

    #[error("запрос вернул {0} строк, ожидали {1}")]
    UnexpectedRows(usize, usize),

    #[error("io: {0}")]
    Io(#[from] std::io::Error), // авто-From
}
```

### `anyhow` — для приложений

В приложениях (CLI, сервисы) часто всё равно, какой именно тип ошибки — нужно её показать пользователю и завершиться. `anyhow::Result<T>` — это `Result<T, anyhow::Error>`:

```rust
use anyhow::{Context, Result};

fn load() -> Result<String> {
    let s = std::fs::read_to_string("config.toml")
        .context("не удалось прочитать config.toml")?;
    Ok(s)
}
```

`.context(...)` добавляет понятное сообщение к технической ошибке.

### Правило большого пальца

- **Библиотека** → `thiserror` + конкретный enum.
- **Приложение** → `anyhow` + `.context()`.
- **`unwrap`/`expect`** → только в тестах, прототипах и при доказанной невозможности `Err`.
- **`panic!`** → только при нарушении инвариантов (это баг, не ошибка).

### `Option` тоже работает с `?`

```rust
fn first_word_len(s: &str) -> Option<usize> {
    let w = s.split_whitespace().next()?;
    Some(w.len())
}
```

## Практика

Пишем CLI, читающий число из файла и удваивающий его.

`Cargo.toml`:

```toml
[dependencies]
thiserror = "1"
anyhow = "1"
```

`src/main.rs`:

```rust
use anyhow::{Context, Result};
use thiserror::Error;
use std::fs;

#[derive(Error, Debug)]
enum DoubleError {
    #[error("файл пуст")]
    Empty,
    #[error("слишком большое число: {0}")]
    TooBig(i64),
}

fn read_number(path: &str) -> Result<i64> {
    let raw = fs::read_to_string(path)
        .with_context(|| format!("чтение {path}"))?;
    let trimmed = raw.trim();
    if trimmed.is_empty() {
        return Err(DoubleError::Empty.into());
    }
    let n: i64 = trimmed.parse()
        .with_context(|| format!("парсинг '{trimmed}'"))?;
    Ok(n)
}

fn double_safe(n: i64) -> Result<i64, DoubleError> {
    n.checked_mul(2).ok_or(DoubleError::TooBig(n))
}

fn main() -> Result<()> {
    let n = read_number("number.txt")?;
    let d = double_safe(n)?;
    println!("{n} * 2 = {d}");
    Ok(())
}
```

Создайте `number.txt` с содержимым `21` и запустите. Затем попробуйте: пустой файл, не-число, число `9223372036854775807` (i64::MAX).

## Тест

**1.** Что напечатает `main`?
```rust
fn parse(s: &str) -> Result<i32, std::num::ParseIntError> {
    let n: i32 = s.parse()?;
    Ok(n * 2)
}
fn main() {
    println!("{:?}", parse("21"));
    println!("{:?}", parse("xx"));
}
```

**2.** Почему этот код не компилируется?
```rust
fn read() -> Result<String, String> {
    let s = std::fs::read_to_string("a.txt")?;
    Ok(s)
}
```

**3.** Чем `unwrap()` отличается от `expect("...")` на практике?

**4.** Можно ли использовать `?` в `main()`? Если да, как должна выглядеть сигнатура?

**5.** Когда выбрать `thiserror`, а когда `anyhow`?

---

### Ответы

**1.** `Ok(42)` и `Err(ParseIntError { kind: InvalidDigit })`. Оператор `?` пробрасывает ошибку парсинга наружу.

**2.** `std::fs::read_to_string` возвращает `Result<String, std::io::Error>`, а функция объявляет `Result<String, String>`. Нет `From<io::Error> for String`, поэтому `?` не может конвертировать. Решение: поменять тип ошибки на `io::Error` или сделать свой enum с `#[from]`.

**3.** Поведение одинаковое (паника при `Err`), но `expect` принимает сообщение, которое попадает в стек-трейс. На production-коде `expect("инвариант X нарушен")` — это документация, объясняющая будущему читателю, **почему** автор уверен, что `Err` невозможен.

**4.** Да. `fn main() -> Result<(), Box<dyn std::error::Error>>` или `fn main() -> anyhow::Result<()>`. При возврате `Err` процесс завершается с кодом 1 и печатает `Debug`-представление ошибки.

**5.** `thiserror` — для **библиотек**: пользователю важно различать варианты ошибок и обрабатывать их типизированно. `anyhow` — для **приложений**: ошибку нужно просто показать и завершиться, типизация не нужна, зато удобен `.context(...)` для накопления контекста по цепочке.
