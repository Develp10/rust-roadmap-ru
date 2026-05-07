# rust-roadmap-ru
🦀 Полный roadmap по изучению Rust на русском + большой список ресурсов. Telegram: t.me/rust_code
<img width="720" height="1280" alt="image" src="https://github.com/user-attachments/assets/9bae9d76-2c51-4ce8-8164-528dd7804fbe" />


# 🦀 Roadmap по изучению Rust: от нуля до профи

Этот путь рассчитан примерно на 6–12 месяцев активного обучения.

## Этап 0. Подготовка окружения (1–3 дня)
- Установка `rustup`, знакомство с `cargo`, `rustc`, `clippy`, `rustfmt`.
- Настройка IDE: VS Code + `rust-analyzer`, RustRover или Neovim + LSP.
- Создание первого проекта `cargo new hello_world`.
- Команды `cargo run`, `cargo build`, `cargo test`, `cargo check`, `cargo doc --open`.

**Мини-проект:** «Hello, world!» с парсингом аргументов командной строки.

## Этап 1. Базовый синтаксис (2–3 недели)
- Переменные, `let`, `mut`, константы, шадоуинг.
- Примитивные типы: целые, плавающие, `bool`, `char`, кортежи, массивы.
- Управляющие конструкции: `if`, `match`, `loop`, `while`, `for`.
- Функции, выражения vs утверждения.
- Модули и `use`, видимость (`pub`).
- Документация (`///`, `//!`).

**Мини-проекты:** калькулятор, угадай число, конвертер температур, FizzBuzz.

## Этап 2. Владение, заимствование, времена жизни (3–4 недели) — самое важное!
- Стек vs куча, модель памяти.
- Ownership, Move-семантика, Copy/Clone.
- Заимствования `&`, `&mut`, правила borrow checker.
- Срезы (`&str`, `&[T]`).
- Времена жизни (`'a`), элизия лайфтаймов.
- Строки: `String` vs `&str`, UTF-8.

**Мини-проекты:** аналог `wc`, парсер CSV без сторонних крейтов.

## Этап 3. Структуры данных и абстракции (3 недели)
- `struct`, `enum`, `Option`, `Result`.
- Сопоставление с образцом (`match`, `if let`, `while let`).
- Методы и `impl`.
- Трейты, дефолтные методы.
- Дженерики, ограничения трейтов, `where`.
- Объекты-трейты `dyn Trait`, `Box<dyn ...>`.
- Коллекции: `Vec`, `HashMap`, `HashSet`, `BTreeMap`, `VecDeque`.

**Мини-проекты:** телефонная книга, todo-list в CLI, простой граф.

## Этап 4. Обработка ошибок и тестирование (1–2 недели)
- `panic!`, `unwrap`, `expect`.
- Идиоматичная обработка через `Result<T, E>`, оператор `?`.
- Свои типы ошибок, `From`/`Into`, крейты `thiserror`, `anyhow`.
- Юнит-тесты, интеграционные тесты, doc-тесты.
- Бенчмарки (`criterion`).

**Мини-проект:** утилита, читающая файлы и валидирующая их.

## Этап 5. Умные указатели и интериор-мутабельность (2 недели)
- `Box<T>`, `Rc<T>`, `Arc<T>`, `Cell<T>`, `RefCell<T>`, `Mutex<T>`, `RwLock<T>`.
- Циклические ссылки и `Weak<T>`.
- `Deref`, `Drop`, RAII.
- `Cow` (Clone-on-Write).

**Мини-проект:** двусвязный список или дерево.

## Этап 6. Многопоточность (2–3 недели)
- Потоки `std::thread`, передача данных через `move`.
- Каналы `mpsc`, `crossbeam`.
- `Send`, `Sync` — безопасность данных на уровне типов.
- Атомики `AtomicUsize`, `Ordering`.
- Пулы потоков (`rayon`).

**Мини-проект:** параллельный word-counter, map-reduce на `rayon`.

## Этап 7. Асинхронное программирование (3–4 недели)
- `Future`, `async`/`await`, runtime (`tokio`, `async-std`, `smol`).
- Стримы (`futures::Stream`).
- `select!`, `join!`, отмена задач.
- Сетевые приложения: TCP/UDP, `hyper`, `reqwest`.

**Мини-проект:** асинхронный HTTP-клиент или чат-сервер.

## Этап 8. Макросы и метапрограммирование (2 недели)
- Декларативные макросы `macro_rules!`.
- Процедурные макросы: derive, attribute, function-like.
- Крейты `syn`, `quote`, `proc-macro2`.

**Мини-проект:** свой `derive(Builder)` или мини-DSL.

## Этап 9. Unsafe Rust и FFI (2 недели)
- Когда и зачем нужен `unsafe`.
- Сырые указатели, `transmute`.
- Работа с C: `extern "C"`, `bindgen`, `cbindgen`.
- Memory layout: `repr(C)`, `repr(transparent)`.
- Знакомство с Miri.

**Мини-проект:** обёртка над C-библиотекой.

## Этап 10. Экосистема и реальные проекты
- **Web:** `axum`, `actix-web`, `rocket`, `warp`.
- **БД:** `sqlx`, `diesel`, `sea-orm`, `redis-rs`.
- **Сериализация:** `serde`, `serde_json`, `bincode`, `prost`.
- **CLI:** `clap`, `indicatif`, `console`.
- **Логирование:** `tracing`, `log`.
- **gRPC:** `tonic`.
- **Embedded:** `embedded-hal`, ESP32/STM32.
- **WebAssembly:** `wasm-bindgen`, `yew`, `leptos`, `dioxus`.
- **Геймдев:** `bevy`, `macroquad`, `ggez`.
- **ML:** `candle`, `burn`, `tch-rs`.

## Этап 11. Уровень профи
- Профилирование: `perf`, `flamegraph`.
- Оптимизация: SIMD, `#[inline]`, layout структур.
- Fuzz-тестирование (`cargo-fuzz`), property-based (`proptest`).
- Чтение исходников `std`, `tokio`, `serde`, `hyper`.
- Контрибьюшн в open-source.
- Публикация крейта на crates.io: документация, CI, semver.
- `nightly`-фичи, RFC, развитие языка.
