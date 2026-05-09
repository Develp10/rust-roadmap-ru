# Урок 10. Обработка ошибок: Result, ?, thiserror, anyhow

Обработка ошибок — одна из тех вещей, в которых Rust принципиально отличается от большинства языков. Здесь нет исключений; ошибки — это **значения**, которые передаются явно через тип `Result<T, E>`. Это даёт три важных свойства: ошибки невозможно «забыть», поток выполнения предсказуем, а компилятор помогает поддерживать корректную обработку.

## Теория

### Две категории ошибок

В Rust ошибки делят на **восстановимые** (recoverable) и **невосстановимые** (unrecoverable):

- **Невосстановимые** — `panic!`. Это для багов и нарушений инвариантов: «такого никогда не должно случиться». Программа аварийно завершается (по умолчанию раскручивает стек, вызывая `Drop` для всех ресурсов).
- **Восстановимые** — `Result<T, E>`. Это ожидаемые ситуации: файл не найден, невалидный ввод, сеть недоступна, валидация не прошла. Их **нужно** обрабатывать.

```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

`Result` — это просто enum в стандартной библиотеке. Никакой магии. Компилятор требует обработать оба варианта (точнее — сделать что-то с возвращённым `Result`, иначе будет предупреждение `#[must_use]`).

### panic! и его поведение

```rust
fn divide(a: i32, b: i32) -> i32 {
    if b == 0 {
        panic!("деление на ноль: a = {}, b = {}", a, b);
    }
    a / b
}
```

При панике по умолчанию:

1. Печатается сообщение и stack trace (если задана `RUST_BACKTRACE=1`).
2. Стек раскручивается, дропаются все локальные значения (RAII работает).
3. Поток завершается. Главный поток — программа выходит с кодом 101.

В `Cargo.toml` можно настроить поведение: `panic = "abort"` отключает раскрутку (меньше бинарник, нет вызова деструкторов).

Когда уместен `panic!`:

- `unreachable!()` — недостижимая ветка.
- `todo!()` — заглушка во время разработки.
- `unimplemented!()` — то же самое, но более «постоянное».
- `assert!`/`debug_assert!` — проверка инвариантов.
- Доказанное «такого не бывает», например `expect("config exists")` после `File::create` сразу за этим.

### Обработка Result через match

```rust
use std::fs::File;
use std::io;

fn open_log() -> File {
    let f = match File::open("app.log") {
        Ok(file) => file,
        Err(e) if e.kind() == io::ErrorKind::NotFound => {
            File::create("app.log").expect("не удалось создать лог")
        }
        Err(e) => panic!("неожиданная ошибка: {e}"),
    };
    f
}
```

Полезно использовать guard'ы (`if e.kind() == ...`) для разных подвидов ошибки.

### unwrap, expect и компания

```rust
let n: i32 = "42".parse().unwrap();                  // паникует при Err
let n: i32 = "42".parse().expect("парсинг 42 ок");    // паникует с сообщением
let n: i32 = "42".parse().unwrap_or(0);               // дефолт при Err
let n: i32 = "42".parse().unwrap_or_else(|e| {
    eprintln!("ошибка: {e}");
    -1
});
```

`unwrap_or_default()` — берёт `Default::default()` для `T`. `map_err` — преобразует только ошибочный вариант. `ok()` — превращает `Result<T, E>` в `Option<T>`, теряя ошибку.

Хороший стиль: в **продакшене** избегать `unwrap`, использовать `expect("инвариант X")` с пояснением, либо обрабатывать ошибку явно. `unwrap` оправдан в тестах, прототипах и при доказанной невозможности `Err`.

### Оператор ?

`?` — сахар для «протолкнуть ошибку наверх». Если `Result == Err`, функция немедленно возвращает `Err` (с автоматической конверсией через `From`). Если `Ok` — извлекает значение.

```rust
use std::fs::File;
use std::io::{self, Read};

fn read_config() -> Result<String, io::Error> {
    let mut f = File::open("config.toml")?;   // Err -> return Err
    let mut s = String::new();
    f.read_to_string(&mut s)?;
    Ok(s)
}
```

Эквивалентный код через `match` был бы намного длиннее. `?` фактически — главная идиома работы с `Result`.

### Свои типы ошибок

Когда функция может фейлиться по-разному, делают `enum`:

```rust
use std::fmt;

#[derive(Debug)]
enum AppError {
    Io(std::io::Error),
    Parse(std::num::ParseIntError),
    NotFound(String),
    Validation(String),
}

impl fmt::Display for AppError {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        match self {
            AppError::Io(e) => write!(f, "ошибка ввода-вывода: {e}"),
            AppError::Parse(e) => write!(f, "ошибка парсинга: {e}"),
            AppError::NotFound(name) => write!(f, "не найдено: {name}"),
            AppError::Validation(msg) => write!(f, "невалидно: {msg}"),
        }
    }
}

impl std::error::Error for AppError {
    fn source(&self) -> Option<&(dyn std::error::Error + 'static)> {
        match self {
            AppError::Io(e) => Some(e),
            AppError::Parse(e) => Some(e),
            _ => None,
        }
    }
}

impl From<std::io::Error> for AppError {
    fn from(e: std::io::Error) -> Self { AppError::Io(e) }
}

impl From<std::num::ParseIntError> for AppError {
    fn from(e: std::num::ParseIntError) -> Self { AppError::Parse(e) }
}
```

Реализации `From<...>` нужны, чтобы оператор `?` сам конвертировал внутренние ошибки в ваш `AppError`. `Display` — для пользовательского сообщения, `std::error::Error` — для интеграции с экосистемой и `source()` (цепочка ошибок).

### thiserror — для библиотек

Писать boilerplate `Display`/`From` вручную утомительно. Крейт `thiserror` делает это за вас:

```rust
use thiserror::Error;

#[derive(Error, Debug)]
pub enum DbError {
    #[error("соединение разорвано")]
    Disconnected,

    #[error("запрос вернул {0} строк, ожидали {1}")]
    UnexpectedRows(usize, usize),

    #[error("io: {0}")]
    Io(#[from] std::io::Error),

    #[error("query failed: {message}")]
    Query { message: String, code: i32 },
}
```

`#[from]` создаёт реализацию `From<std::io::Error>` автоматически. Получаемый `DbError` — типизированная, документированная ошибка с понятным `Display`.

`thiserror` идеален для **библиотек**: пользователь библиотеки хочет различать варианты и обрабатывать их по-разному.

### anyhow — для приложений

В CLI и сервисах обычно достаточно «есть ошибка, надо её показать». `anyhow::Error` — это «универсальная» ошибка, которая хранит любой `std::error::Error + Send + Sync + 'static`:

```rust
use anyhow::{Context, Result};

fn load_config() -> Result<Config> {
    let raw = std::fs::read_to_string("config.toml")
        .context("не удалось прочитать config.toml")?;
    let cfg: Config = toml::from_str(&raw)
        .context("не удалось распарсить config.toml")?;
    Ok(cfg)
}
```

Запуск с `RUST_BACKTRACE=1` и `anyhow::Error` даст:

```
Error: не удалось распарсить config.toml

Caused by:
    expected =, found < at line 3 column 5
```

`.context()` накапливает поясняющие сообщения сверху вниз. `with_context(|| format!(...))` ленивее и подходит, когда сообщение строится дорого.

### Правило большого пальца

| Ситуация | Что использовать |
|----------|------------------|
| Библиотека | `thiserror` + конкретный `enum` |
| Приложение | `anyhow` + `.context()` |
| Прототип/тест | `unwrap`/`expect` |
| Невозможный `Err` | `expect("инвариант X")` |
| Баг/инвариант | `panic!`/`assert!`/`unreachable!` |

### Option тоже работает с ?

Оператор `?` поддерживает не только `Result`, но и `Option` (если функция возвращает `Option`):

```rust
fn first_word_len(s: &str) -> Option<usize> {
    let w = s.split_whitespace().next()?;
    Some(w.len())
}
```

Конверсия между `Option` и `Result` возможна через `.ok_or(err)` / `.ok_or_else(|| err)` и `.ok()`.

### main, возвращающий Result

```rust
use anyhow::Result;

fn main() -> Result<()> {
    let cfg = load_config()?;
    run(cfg)?;
    Ok(())
}
```

Если `main` вернёт `Err`, программа выйдет с кодом 1 и напечатает Debug-представление ошибки. Это удобный способ собрать ошибку из глубин кода в одну точку выхода.

### Backtraces и контекст

С Rust 1.65+ доступен тип `std::backtrace::Backtrace`. `anyhow` и `thiserror` умеют сохранять его автоматически при включённой переменной `RUST_BACKTRACE=1`. Это бесценно при отладке прода.

### Не паниковать в FFI

Если ваш Rust-код вызывается из C/Go/Python через FFI, panic через границу — UB. Используйте `std::panic::catch_unwind` для перехвата:

```rust
let result = std::panic::catch_unwind(|| {
    risky_operation()
});
match result {
    Ok(v) => /* нормально */,
    Err(_) => /* был panic, превращаем в код ошибки */,
}
```

## Практика

### Задача 1. Простой парсер

```rust
fn parse_double(s: &str) -> Result<i32, std::num::ParseIntError> {
    let n: i32 = s.parse()?;
    Ok(n * 2)
}

fn main() {
    println!("{:?}", parse_double("21"));     // Ok(42)
    println!("{:?}", parse_double("xyz"));    // Err(...)
    println!("{:?}", parse_double(" 7"));     // Err(...) — пробелы не парсятся
}
```

### Задача 2. Свой enum ошибок

```rust
use std::fmt;

#[derive(Debug)]
enum CalcError {
    DivByZero,
    Negative,
    Overflow,
}

impl fmt::Display for CalcError {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        match self {
            CalcError::DivByZero => write!(f, "деление на ноль"),
            CalcError::Negative => write!(f, "отрицательное число недопустимо"),
            CalcError::Overflow => write!(f, "переполнение"),
        }
    }
}
impl std::error::Error for CalcError {}

fn safe_div(a: i64, b: i64) -> Result<i64, CalcError> {
    if b == 0 { return Err(CalcError::DivByZero); }
    a.checked_div(b).ok_or(CalcError::Overflow)
}

fn safe_sqrt(n: i64) -> Result<f64, CalcError> {
    if n < 0 { return Err(CalcError::Negative); }
    Ok((n as f64).sqrt())
}

fn main() {
    println!("{:?}", safe_div(10, 0));
    println!("{:?}", safe_sqrt(-4));
    println!("{:?}", safe_sqrt(16));
}
```

### Задача 3. Цепочка ? с автоконверсией

```rust
use std::fs;
use std::num::ParseIntError;
use std::io;

#[derive(Debug)]
enum AppError {
    Io(io::Error),
    Parse(ParseIntError),
}
impl From<io::Error> for AppError { fn from(e: io::Error) -> Self { Self::Io(e) } }
impl From<ParseIntError> for AppError { fn from(e: ParseIntError) -> Self { Self::Parse(e) } }

fn load_number(path: &str) -> Result<i32, AppError> {
    let content = fs::read_to_string(path)?;       // io::Error -> AppError
    let n: i32 = content.trim().parse()?;          // ParseIntError -> AppError
    Ok(n)
}

fn main() {
    println!("{:?}", load_number("number.txt"));
    println!("{:?}", load_number("missing.txt"));
}
```

### Задача 4. thiserror + anyhow вместе

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

Создайте `number.txt` с содержимым `21` и запустите. Затем попробуйте: пустой файл, не-число, число `9223372036854775807` (`i64::MAX`).

### Задача 5. Восстановление с unwrap_or_else

```rust
fn get_port_from_env() -> u16 {
    std::env::var("PORT")
        .unwrap_or_else(|_| "8080".to_string())
        .parse()
        .unwrap_or(8080)
}

fn main() {
    println!("port = {}", get_port_from_env());
}
```

### Задача 6. Option и Result вместе

```rust
fn last_digit(s: &str) -> Result<u32, String> {
    s.chars()
        .rev()
        .find(|c| c.is_ascii_digit())
        .ok_or_else(|| format!("в '{s}' нет цифр"))
        .and_then(|c| c.to_digit(10).ok_or_else(|| "не цифра?".to_string()))
}

fn main() {
    for s in ["abc123", "no digits", "42!"] {
        println!("{s:?} -> {:?}", last_digit(s));
    }
}
```

### Задача 7. main с Result и context

```rust
use anyhow::{Context, Result};

fn parse_int(s: &str) -> Result<i32> {
    let n: i32 = s.parse().with_context(|| format!("парсинг '{s}'"))?;
    Ok(n)
}

fn double(s: &str) -> Result<i32> {
    let n = parse_int(s).context("в функции double")?;
    Ok(n * 2)
}

fn main() -> Result<()> {
    let r = double("xx")?;
    println!("{r}");
    Ok(())
}
```

Запустите — увидите цепочку контекста сверху вниз.

## Тест

**1.** Что напечатает программа?

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

Как починить?

**3.** Чем `unwrap()` отличается от `expect("...")` на практике?

**4.** Можно ли использовать `?` в `main()`? Если да, как должна выглядеть сигнатура?

**5.** Когда выбрать `thiserror`, а когда `anyhow`?

**6.** Что напечатает программа?

```rust
fn first_word_len(s: &str) -> Option<usize> {
    let w = s.split_whitespace().next()?;
    Some(w.len())
}
fn main() {
    println!("{:?}", first_word_len("hello world"));
    println!("{:?}", first_word_len("   "));
    println!("{:?}", first_word_len(""));
}
```

**7.** Скомпилируется ли?

```rust
fn maybe() -> Result<i32, String> {
    let s = "42";
    let n: i32 = s.parse()?;
    Ok(n)
}
```

**8.** Чем `a.context("x")?` отличается от `a.with_context(|| "x".to_string())?` в `anyhow`?

**9.** В чём смысл реализации `std::error::Error::source()` для своего типа ошибки?

**10.** Что произойдёт, если в FFI-функции, экспортированной как `extern "C"`, случится `panic!`?

---

### Ответы

1. `Ok(42)` и `Err(ParseIntError { kind: InvalidDigit })` (точное представление зависит от версии stdlib). Оператор `?` пробрасывает ошибку парсинга наружу как `Err`.

2. `std::fs::read_to_string` возвращает `Result<String, std::io::Error>`, а функция объявлена с типом ошибки `String`. Нет реализации `From<io::Error> for String`, поэтому `?` не может конвертировать. Чинится: либо поменять тип ошибки на `io::Error`, либо сделать свой `enum AppError` с `#[from]`, либо вручную `.map_err(|e| e.to_string())?`.

3. Поведение одинаковое (паника при `Err`). Различие — в сообщении: `expect("...")` принимает строку, которая попадает в panic-сообщение и stack trace. На production-коде `expect("инвариант X нарушен")` — это документация: будущий читатель сразу видит, **почему** автор был уверен, что `Err` невозможен.

4. Да. Сигнатура — `fn main() -> Result<(), Box<dyn std::error::Error>>` или `fn main() -> anyhow::Result<()>`. Любой возврат `Err` приведёт к завершению процесса с кодом 1 и печатью Debug-представления ошибки.

5. `thiserror` — для **библиотек**: пользователю важно различать варианты ошибок и обрабатывать их типизированно, поэтому нужен конкретный enum с задокументированными вариантами. `anyhow` — для **приложений**: ошибку обычно надо просто показать пользователю и завершиться; типизация не нужна, зато бесценен `.context(...)` для накопления цепочки причин.

6. `Some(5)`, `None`, `None`. Для `"hello world"` первое слово — «hello», его длина 5. Для строки из пробелов и пустой строки `split_whitespace().next()` даёт `None`, и `?` пробрасывает его наружу.

7. Не скомпилируется. `s.parse::<i32>()` возвращает `Result<i32, ParseIntError>`, а функция объявляет ошибку `String`. `From<ParseIntError> for String` нет, поэтому `?` не сработает. Чинится через `.map_err(|e| e.to_string())?` или сменой типа ошибки.

8. `context("x")` принимает уже готовую строку или `&'static str` — её всегда нужно построить, даже если ошибки не было. `with_context(|| ...)` принимает замыкание; оно вызывается **только при ошибке**, поэтому полезно, когда сообщение строится дорого (например, через `format!` со сложными подстановками).

9. `source()` возвращает «причину» текущей ошибки — нижележащую ошибку. Это создаёт цепочку, которую инструменты (`anyhow`, логгеры, тесты) умеют пройти и красиво напечатать. Без `source` цепочка обрывается на верхнем уровне, и пользователь не увидит, какая io- или parse-ошибка привела к проблеме.

10. По умолчанию это **undefined behavior**: разворачивание стека через FFI-границу — UB по правилам Rust и C. С Rust 1.71+ panic в `extern "C"` функциях по умолчанию приводит к `abort` (а не UB), но всё равно крайне нежелателен. Правильный способ — обернуть тело функции в `std::panic::catch_unwind(|| ...)` и сконвертировать панику в код ошибки для вызывающего.
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
