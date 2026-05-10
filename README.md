# 🦀 Rust Roadmap (на русском)

Полный курс по языку программирования Rust на русском языке: от первой строчки кода до асинхронности, embedded и WebAssembly. Каждый урок состоит из теории, практики и теста с ответами. [t.me/rust_code]([https://t.me/rust_code](https://t.me/+HelhY88sArowYjgy)** — основной канал телеграм по Rust на русском.

## 📚 О курсе

- **25 уроков** — от базового синтаксиса до прикладной разработки
- **Каждый урок**: теория с примерами → практика с задачами → тест → разбор ответов
- **Только Rust 2021/2024** — современный идиоматичный код, актуальные крейты экосистемы
- **Без воды** — только практически полезный материал

[Roadmap на английском](https://github.com/Develp10/rust-roadmap-en/tree/main)
## 🗺️ Дорожная карта

### Часть 1. Основы (уроки 1–8)

| № | Урок | Темы |
|---|------|------|
| 1 | [Введение в Rust](lessons/lesson-01-intro.md) | установка, cargo, hello world, структура проекта |
| 2 | [Переменные и типы](lessons/lesson-02-variables-types.md) | let/mut, целые/плавающие, bool, char, кортежи, массивы, преобразования, переполнение |
| 3 | [Управление потоком](lessons/lesson-03-control-flow.md) | if/else, loop, while, for, match, let-else, loop labels |
| 4 | [Владение (ownership)](lessons/lesson-04-ownership.md) | move, Copy, Clone, scope, Drop, замыкания и захват |
| 5 | [Заимствование и ссылки](lessons/lesson-05-borrowing.md) | &T, &mut T, NLL, reborrow, elision, времена жизни |
| 6 | [Slices, String, Vec](lessons/lesson-06-slices-strings-vec.md) | срезы, UTF-8, capacity, итерация по строкам |
| 7 | [Structs и enums](lessons/lesson-07-structs-enums.md) | tuple/unit-структуры, derive, match на enum, niche, builder |
| 8 | [Generics и traits](lessons/lesson-08-generics-traits.md) | мономорфизация, dyn vs impl, trait bounds, ассоциированные типы, перегрузка операторов |

### Часть 2. Углублённые темы (уроки 9–14)

| № | Урок | Темы |
|---|------|------|
| 9 | [Lifetimes](lessons/lesson-09-lifetimes.md) | элизия, 'static, HRTB, структуры со ссылками |
| 10 | [Обработка ошибок](lessons/lesson-10-error-handling.md) | Result/Option, ? оператор, panic, thiserror, anyhow, FFI |
| 11 | [Коллекции и итераторы](lessons/lesson-11-collections-iterators.md) | HashMap, BTreeMap, VecDeque, BinaryHeap, кастомные итераторы |
| 12 | [Умные указатели](lessons/lesson-12-smart-pointers.md) | Box, Rc, Arc, RefCell, Mutex/RwLock, OnceLock, Cow, Weak |
| 13 | [Многопоточность](lessons/lesson-13-concurrency.md) | thread, Send/Sync, каналы, scoped threads |
| 14 | [Async и Tokio](lessons/lesson-14-async-tokio.md) | Future, async/await, tokio runtime, select!, каналы |

### Часть 3. Продвинутые возможности (уроки 15–19)

| № | Урок | Темы |
|---|------|------|
| 15 | [Макросы](lessons/lesson-15-macros.md) | macro_rules!, процедурные макросы, derive |
| 16 | [Тестирование](lessons/lesson-16-testing.md) | unit, integration, doc-tests, property-based |
| 17 | [Экосистема Cargo](lessons/lesson-17-cargo-ecosystem.md) | workspaces, features, profiles, публикация |
| 18 | [Unsafe и FFI](lessons/lesson-18-unsafe-ffi.md) | unsafe, raw-указатели, extern "C", bindgen |
| 19 | [Веб с axum](lessons/lesson-19-web-axum.md) | роутинг, extractors, middleware, JSON, state |

### Часть 4. Прикладные направления (уроки 20–25)

| № | Урок | Темы |
|---|------|------|
| 20 | [Базы данных: sqlx и diesel](lessons/lesson-20-databases-sqlx.md) | пулы, миграции, транзакции, repository |
| 21 | [gRPC с tonic](lessons/lesson-21-grpc-tonic.md) | protobuf, streaming, interceptors, TLS |
| 22 | [Embedded и no_std](lessons/lesson-22-embedded-no-std.md) | core/alloc, embedded-hal, RTIC, Embassy |
| 23 | [WebAssembly](lessons/lesson-23-wasm.md) | wasm-bindgen, web-sys, Yew/Leptos, WASI |
| 24 | [Паттерны проектирования](lessons/lesson-24-design-patterns.md) | newtype, builder, typestate, strategy, RAII |
| 25 | [CLI-приложения](lessons/lesson-25-cli-tools.md) | clap, anyhow, tracing, indicatif, ratatui |

## 🚀 Как учиться

1. Установите Rust с [rustup.rs](https://rustup.rs/) и любой редактор с поддержкой rust-analyzer (VS Code, Helix, Zed, RustRover).
2. Идите по урокам последовательно: каждый следующий опирается на предыдущий.
3. Для каждого урока: прочитайте теорию → разберите код практики → решите задачи самостоятельно → пройдите тест и сверьтесь с ответами.
4. Создавайте `cargo new` песочницу под каждый урок и экспериментируйте.

## 🛠️ Что понадобится

- **Rust 1.75+** (`rustup update stable`)
- **cargo** — встроенный пакетный менеджер
- **rust-analyzer** — LSP для подсветки и автодополнения
- Терминал, базовое знакомство с Git и любым другим языком (для контекста)

## 📦 Полезные команды

```bash
# создать проект
cargo new my_project
cd my_project

# собрать и запустить
cargo run
cargo build --release

# тесты, проверки, форматирование
cargo test
cargo clippy
cargo fmt

# обновить зависимости
cargo update
```

## 📖 Дополнительные ресурсы

- [The Rust Book (русский перевод)](https://doc.rust-lang.ru/book/)
- [Rust by Example](https://doc.rust-lang.org/rust-by-example/)
- [Rustlings — упражнения](https://github.com/rust-lang/rustlings)
- [crates.io](https://crates.io/) — реестр пакетов
- [docs.rs](https://docs.rs/) — документация всех опубликованных крейтов
- [This Week in Rust](https://this-week-in-rust.org/) — еженедельный дайджест

## 🤝 Вклад

Нашли опечатку, неточность или хотите добавить пример? Открывайте issue или присылайте pull request.

## 📜 Лицензия

Материалы курса распространяются под лицензией MIT. Используйте, копируйте, модифицируйте и распространяйте свободно.

---

> Удачи в изучении Rust! 🦀
# rust-roadmap-ru
🦀 Полный roadmap по изучению Rust на русском + большой список ресурсов. Telegram: [t.me/rust_code](https://t.me/+HelhY88sArowYjgy)
<img width="720" height="1280" alt="image" src="https://github.com/user-attachments/assets/9bae9d76-2c51-4ce8-8164-528dd7804fbe" />


---

## 📚 Полезные ресурсы

### 📺 Telegram-каналы и подборки
- **[t.me/rust_code](https://t.me/rust_code)** — основной канал по Rust на русском.
- **[t.me/books_englishhh](https://t.me/books_englishhh)** — англоязычные книги и материалы по Rust.
- **[t.me/ai_machinelearning_big_data](https://t.me/ai_machinelearning_big_data)** — AI, ML, Big Data (полезно при работе с `candle`, `burn`, `tch-rs`).
- **[Папка с обучающими ресурсами](https://t.me/addlist/2Ls-snqEeytkMDgy)** — большая подборка каналов для разработчиков.

### 📖 Статьи на русском (must-read для глубокого понимания)
- [Твой код на Rust компилируется, проходит тесты и является UB. Ты просто об этом не знаешь](https://habr.com/ru/articles/1033328/) — про скрытые ловушки undefined behavior в безопасном на вид Rust-коде.
- [Как Rust обманывает процессор: тайная жизнь niche-оптимизации, drop flags и MIR](https://habr.com/ru/articles/1031398/) — внутренности компилятора и неожиданные оптимизации.
- [Как Rust обманывает процессор. Часть 2: niche сквозь крейты, dropck, Pin и провенанс указателей](https://uproger.com/kak-rust-obmanyvaet-proczessor-chast-2-niche-skvoz-krejty-dropck-pin-i-provenans-ukazatelej/) — продолжение про Pin, dropck и провенанс.

### 📘 Официальная документация
- **[The Rust Book](https://doc.rust-lang.org/book/)** — главная книга, начинать отсюда. [Русский перевод](https://doc.rust-lang.ru/book/).
- **[Rust Book с интерактивными квизами](https://rust-book.cs.brown.edu)** — экспериментальная версия от Brown University.
- **[Rust by Example](https://doc.rust-lang.org/rust-by-example/)** — концепции через работающие примеры.
- **[The Rustonomicon](https://doc.rust-lang.org/nomicon/)** — тёмная сторона: unsafe, lifetimes, layout.
- **[The Rust Reference](https://doc.rust-lang.org/reference/)** — формальная спецификация языка.
- **[Rust Async Book](https://rust-lang.github.io/async-book/)** — асинхронность от создателей языка.
- **[Rust API Guidelines](https://rust-lang.github.io/api-guidelines/)** — как проектировать идиоматичные API.
- **[std documentation](https://doc.rust-lang.org/std/)** — стандартная библиотека (читать даже без запроса!).

### 🎓 Бесплатные книги и курсы онлайн
- **[Rust Atomics and Locks](https://marabos.nl/atomics/)** — Mara Bos, бесплатно онлайн. Про многопоточность и memory ordering.
- **[Comprehensive Rust](https://google.github.io/comprehensive-rust/)** — курс от Google, 4 дня интенсива.
- **[100 Exercises To Learn Rust](https://rust-exercises.com/)** — Luca Palmieri.
- **[Easy Rust](https://dhghomon.github.io/easy_rust/)** — простыми словами для начинающих.
- **[Too Many Linked Lists](https://rust-unofficial.github.io/too-many-lists/)** — учим Rust через реализацию связных списков (классика!).
- **[Writing an OS in Rust](https://os.phil-opp.com/)** — Philipp Oppermann, минимальная ОС с нуля.
- **[Crafting Interpreters на Rust](https://github.com/jeschkies/lox-rs)** — пишем интерпретатор.
- **[Zero To Production In Rust](https://www.zero2prod.com/)** — Luca Palmieri, веб-сервисы (платная, но стоит того).

### 💪 Практика и упражнения
- **[Rustlings](https://github.com/rust-lang/rustlings)** — 90+ интерактивных упражнений, обязательно в начале.
- **[Exercism Rust Track](https://exercism.org/tracks/rust)** — задачи с менторскими ревью.
- **[Rust Quiz](https://dtolnay.github.io/rust-quiz/)** — каверзные вопросы по семантике от dtolnay.
- **[Rust Playground](https://play.rust-lang.org/)** — экспериментировать без `cargo`, смотреть MIR/LLVM IR/ASM.
- **[Codewars](https://www.codewars.com/?language=rust)** — алгоритмические ката.
- **[Advent of Code](https://adventofcode.com/)** — ежегодный декабрьский марафон, отлично заходит на Rust.
- **[CodeCrafters](https://codecrafters.io/)** — пиши свой Redis/Git/Docker на Rust.

### 📰 Блоги и новости
- **[This Week in Rust](https://this-week-in-rust.org/)** — еженедельный дайджест экосистемы. Подписка обязательна.
- **[Inside Rust Blog](https://blog.rust-lang.org/inside-rust/)** — что происходит внутри команды разработчиков.
- **[fasterthanli.me](https://fasterthanli.me/)** — Amos, глубокие технические статьи.
- **[Without Boats](https://without.boats/)** — блог одного из ключевых дизайнеров async Rust.
- **[Niko Matsakis](https://smallcultfollowing.com/babysteps/)** — сооснователь языка, пишет про типовую систему.
- **[Manish Goregaokar](https://manishearth.github.io/)** — глубокие посты про unsafe и FFI.

### 🎥 YouTube-каналы
- **[Jon Gjengset](https://www.youtube.com/@jonhoo)** — многочасовые стримы по внутренностям (Crust of Rust).
- **[No Boilerplate](https://www.youtube.com/@NoBoilerplate)** — короткие мотивирующие видео про Rust.
- **[Logan Smith](https://www.youtube.com/@_noisecode)** — продвинутые темы доступным языком.
- **[Let's Get Rusty](https://www.youtube.com/@letsgetrusty)** — туториалы для начинающих.
- **[Code to the Moon](https://www.youtube.com/@codetothemoon)** — обзоры экосистемы и крейтов.
- **[chris biscardi](https://www.youtube.com/@chrisbiscardi)** — bevy, leptos, фронтенд на Rust.

### 🔧 Инструменты, без которых жить нельзя
- **[rust-analyzer](https://rust-analyzer.github.io/)** — LSP, основа продуктивности.
- **[cargo-edit](https://github.com/killercup/cargo-edit)** — `cargo add/rm/upgrade`.
- **[cargo-watch](https://github.com/watchexec/cargo-watch)** — авто-перезапуск при изменениях.
- **[cargo-expand](https://github.com/dtolnay/cargo-expand)** — посмотреть, во что разворачиваются макросы.
- **[cargo-nextest](https://nexte.st/)** — быстрый тест-раннер.
- **[bacon](https://github.com/Canop/bacon)** — фоновый компилятор-watcher.

### 🌐 Сообщество
- **[Rust Users Forum](https://users.rust-lang.org/)** — лучшее место для вопросов новичков.
- **[Rust Internals](https://internals.rust-lang.org/)** — обсуждение развития языка.
- **[r/rust](https://www.reddit.com/r/rust/)** — Reddit-сообщество.
- **[Rust Discord](https://discord.gg/rust-lang)** — официальный Discord.
- **[Rust Zulip](https://rust-lang.zulipchat.com/)** — где сидят разработчики компилятора.
- **[Are We X Yet?](https://wiki.mozilla.org/Areweyet)** — обзоры зрелости экосистемы (`Are We Web Yet?`, `Are We Game Yet?`, `Are We Async Yet?`).

### 🦀 Куратор-листы (awesome-lists)
- **[awesome-rust](https://github.com/rust-unofficial/awesome-rust)** — главный список крейтов и проектов.
- **[Not Yet Awesome Rust](https://github.com/not-yet-awesome-rust/not-yet-awesome-rust)** — чего пока не хватает в экосистеме (повод законтрибьютить).
- **[Awesome Embedded Rust](https://github.com/rust-embedded/awesome-embedded-rust)** — embedded.
- **[Awesome WebAssembly](https://github.com/mbasso/awesome-wasm)** — WASM.

---


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

---

## 🚀 Расширение roadmap: продвинутые темы

### Этап 12. Продвинутая типовая система
- Higher-Ranked Trait Bounds (`for<'a>`), GAT (Generic Associated Types).
- Associated types vs дженерики — когда что выбирать.
- `PhantomData`, zero-sized types, типобезопасные API.
- Type-state pattern (`Builder<Unbuilt>` → `Builder<Built>`).
- Sealed traits, расширяемые закрытые API.
- Newtype-паттерн и его роль в borrow checker.
- Variance: covariance, contravariance, invariance (`&mut T` инвариантен).
- Auto-traits, negative impls (`!Send`, `!Sync`), `?Sized`, DST.

### Этап 13. Глубокая работа с асинхронностью
- Внутренности `Future`: `Poll`, `Pin`, `Unpin`, self-referential типы.
- Устройство runtime: reactor, executor, waker, task scheduling.
- Cooperative scheduling, проблема блокирующих вызовов в async.
- `tokio::spawn` vs `spawn_blocking`, `LocalSet`, `JoinSet`.
- Cancellation safety и почему `select!` опасен.
- Структурированная конкурентность (`tokio-util`, `async-scoped`).
- Backpressure в стримах.
- `async-trait` и нативные async fn in traits (стабильны с 1.75).
- Отладка асинхронного кода: `tokio-console`, tracing-spans.

### Этап 14. Производительность и низкий уровень
- Cache-friendly структуры, false sharing, padding.
- SoA vs AoS (struct of arrays).
- Branch prediction, likely/unlikely.
- SIMD: `std::simd`, `std::arch`, крейт `wide`.
- Allocator API, кастомные аллокаторы (`bumpalo`, `mimalloc`, `jemallocator`).
- Arena-аллокация, typed-arena.
- Zero-copy парсинг (`nom`, `winnow`, `zerocopy`, `rkyv`).
- Профилирование: `perf`, `cargo-flamegraph`, `samply`, `tracy`, `dhat-rs`, `cargo-bloat`.
- Benchmark-driven development (`criterion`, `iai`).

### Этап 15. Безопасность и корректность
- Формальная верификация: `Kani`, `Prusti`, `Creusot`.
- Model checking: `loom`, `shuttle`.
- Sanitizers через nightly: `-Z sanitizer=address|thread|memory|leak`.
- Mutation testing (`cargo-mutants`).
- Аудит зависимостей: `cargo-audit`, `cargo-deny`, `cargo-vet`, `cargo-geiger`.
- Supply chain security, reproducible builds.
- Безопасный FFI: ABI-стабильность, panic-unwinding, `catch_unwind`.

### Этап 16. Архитектура и паттерны проектирования
- Идиоматичный Rust: builder, typestate, RAII-guards, newtype.
- Hexagonal/Clean architecture без DI-фреймворков.
- Error handling: библиотека (`thiserror`) vs приложение (`anyhow`, `eyre`, `color-eyre`).
- Антипаттерны: борьба с borrow checker, избыточное `Arc<Mutex<_>>`, clone-driven development.
- Композиция вместо наследования, enum-полиморфизм vs trait objects.

### Этап 17. Системное программирование и ОС
- `mio`, epoll/kqueue/io_uring напрямую.
- `tokio-uring`, `glommio` (thread-per-core).
- Shared memory, `mmap`, `memmap2`.
- Сигналы Unix (`signal-hook`), процессы, pipes.
- Свой аллокатор, свой `Future`-executor с нуля.
- Kernel-разработка: Rust в Linux kernel, `Redox OS`, `Theseus`.
- Bare-metal: `#![no_std]`, `core` vs `alloc` vs `std`, panic-handler, custom target.
- Bootloader и минимальная ОС (Philipp Oppermann «Writing an OS in Rust»).

### Этап 18. Распределённые системы и сеть
- Реализация Redis-протокола, Raft (`openraft`), gossip-протоколы.
- QUIC через `quinn`, HTTP/2 и HTTP/3.
- gRPC streaming, interceptors в `tonic`.
- Observability: OpenTelemetry, `tracing-opentelemetry`, метрики (`metrics`, `prometheus`).
- Идемпотентность, retry, circuit breaker (`tower`, `tower-http`).
- Event sourcing и CQRS на Rust.

### Этап 19. Базы данных и хранилища
- Глубже sqlx: compile-time проверка запросов, миграции (`sqlx-cli`, `refinery`).
- ORM (`diesel`) vs query-builder (`sea-query`).
- Connection pooling (`deadpool`, `bb8`).
- Embedded базы: `sled`, `redb`, `rocksdb`, `fjall`.
- Свой storage engine: B-Tree, LSM, WAL.
- Векторные БД и поиск: `qdrant`, `tantivy`.

### Этап 20. WebAssembly углублённо
- `wasm32-unknown-unknown` vs `wasm32-wasi` vs `wasm32-wasip2`.
- Component Model и WIT-интерфейсы.
- `wasmtime`, `wasmer` как embedder'ы.
- Plugin-системы на WASM (`extism`).
- Frontend: сравнение `leptos`, `dioxus`, `yew`, `sycamore` (signals vs vdom).
- SSR, hydration, server functions.
- Edge-runtime (Cloudflare Workers, Fastly Compute).

### Этап 21. Embedded и реальное время
- `embedded-hal` 1.0, HAL-крейты для конкретных МК.
- RTOS на Rust: `Embassy` (async-first), `RTIC` (priority-based).
- DMA, прерывания, критические секции (`critical-section`).
- `defmt` для эффективного логирования.
- `probe-rs` для отладки и прошивки.
- Безопасность памяти на `no_std`: `heapless`, статическое выделение.

### Этап 22. Геймдев и графика
- ECS в `bevy`: компоненты, системы, ресурсы, schedules.
- Шейдеры на WGSL, `wgpu` напрямую.
- Физика: `rapier`, `avian`.
- Networking для игр: `lightyear`, `bevy_replicon`.
- Альтернативы: `fyrox`, `macroquad`, `ggez`.
- Графика низкого уровня: `vulkano`, `ash` (Vulkan bindings).

### Этап 23. ML, AI и научные вычисления
- `candle` (HuggingFace) — minimalist tensor framework.
- `burn` — гибкий ML-фреймворк с разными бэкендами.
- `tch-rs` — биндинги к LibTorch.
- `ort` — ONNX Runtime.
- Линейная алгебра: `nalgebra`, `ndarray`, `faer`.
- Распараллеливание: `rayon`, GPU через `cust` (CUDA), `cubecl`.

### Этап 24. Инструментарий, DevEx и продакшн
- Workspaces, feature flags, conditional compilation.
- Build scripts (`build.rs`).
- `cargo`-расширения: `cargo-make`, `cargo-nextest`, `cargo-watch`, `cargo-expand`, `cargo-udeps`, `cargo-machete`.
- Кросс-компиляция: `cross`, `cargo-zigbuild`.
- Минимизация бинарника: LTO, `panic=abort`, `strip`, `opt-level="z"`.
- Docker: distroless и scratch-образы, multi-stage builds.
- CI/CD: GitHub Actions с кэшем `Swatinem/rust-cache`, matrix-сборки.
- Релизы: `cargo-release`, `cargo-dist`, `release-plz`.
- Семантическое версионирование: `cargo-semver-checks`.

### Этап 25. Контрибьюшн и развитие языка
- RFC-процесс.
- Структура `rust-lang/rust`: компилятор (`rustc`), стандартная библиотека, `cargo`.
- MIR, HIR, THIR.
- Borrow checker (NLL, Polonius).
- Edition-механизм (2015 → 2018 → 2021 → 2024).
- «This Week in Rust», Inside Rust Blog.
- Менторство в `rustc-dev-guide`.

---

## 💡 На что ещё стоит обратить внимание

**Метакогнитивные навыки.** Умение читать сообщения компилятора как документацию — половина продуктивности в Rust. `cargo expand`, `cargo asm`, Rust Playground с просмотром MIR/LLVM IR — must-have для понимания, что реально происходит.

**Чтение исходников** в порядке возрастания сложности: `itertools` → `serde` → `tokio` → `hyper` → `rustc`. Это даёт больше, чем десяток книг.

**Сообщество.** Rust Users Forum (users.rust-lang.org), Rust Internals, Discord/Zulip Rust, русскоязычный Telegram-чат. Ревью своего кода у опытных растовиков ускоряет рост на месяцы.

**Книги продвинутого уровня:**
- «Rust for Rustaceans» — Jon Gjengset.
- «Programming Rust» — Blandy / Orendorff / Tindall (O'Reilly).
- «Rust Atomics and Locks» — Mara Bos (обязательна перед серьёзной многопоточкой).
- «Zero To Production In Rust» — Luca Palmieri (практика веб-сервисов).
- «The Rustonomicon» — для unsafe.

**YouTube:** Jon Gjengset (стримы по внутренностям), `fasterthanli.me` (Amos), `Logan Smith`, `No Boilerplate`.


---

## 🎯 Советы по изучению языка

### Общая стратегия

**Не пытайся выучить всё сразу.** Rust большой: ownership, lifetimes, async, макросы, unsafe, типовая система. Если штурмовать всё параллельно — выгоришь. Двигайся по roadmap последовательно: каждый этап опирается на предыдущий.

**Пиши код каждый день, хотя бы 30 минут.** Чтение книг и просмотр видео даёт иллюзию понимания. Реальное знание приходит только через борьбу с компилятором. Маленькие ежедневные коммиты сильнее редких марафонов.

**Доверяй компилятору.** Сообщения об ошибках в Rust — это не препятствие, а самый честный учитель. Читай их полностью, переходи по ссылкам `--explain E0382`, не игнорируй warnings от `clippy`.

**Не борись с borrow checker — слушай его.** Если код не компилируется, чаще всего проблема в архитектуре, а не в правилах языка. Перепиши структуру данных, разорви циклические зависимости, подумай о владении заранее.

### Практические привычки

**Решай задачи на [Rustlings](https://github.com/rust-lang/rustlings)** в самом начале — это 90+ маленьких упражнений, закрывающих базовый синтаксис. После них пройди [Exercism Rust track](https://exercism.org/tracks/rust) с менторскими ревью.

**Читай официальную «The Rust Book»** ([rust-book.cs.brown.edu](https://rust-book.cs.brown.edu) — версия с интерактивными квизами). Затем «Rust by Example» для практических паттернов.

**Используй `cargo clippy` и `cargo fmt` с первого дня.** Clippy подсказывает идиоматичные конструкции — это ускоряет переход от «пишу как на C++/Python» к «пишу как растовик».

**Не клонируй ради тишины компилятора.** Если возникает желание поставить `.clone()` или `Arc<Mutex<_>>` — остановись. Чаще всего нужно поменять сигнатуру (`&str` вместо `String`, итератор вместо `Vec`) или переосмыслить владение.

**Объясняй код вслух (rubber duck debugging).** Проговаривание «здесь у меня владелец, здесь заёмщик, время жизни 'a связано с...» закрепляет ментальную модель быстрее, чем чтение.

### Работа с проектами

**Делай pet-проекты, а не туториалы.** Туториалы дают ложное чувство прогресса. Возьми идею, которую тебе реально хочется реализовать — CLI-утилиту, бота, парсер логов — и пиши её с нуля.

**Переписывай на Rust то, что уже знаешь.** Если у тебя есть скрипт на Python или сервис на Go — перепиши его. Сравнение даст глубокое понимание различий в моделях памяти и абстракциях.

**Читай чужой код.** Открывай исходники крейтов, которые используешь (`serde`, `tokio`, `clap`) — даже без полного понимания. Постепенно паттерны станут узнаваемы.

**Контрибьють в open-source раньше, чем кажется готовым.** Найди issue с лейблом `good-first-issue` или `E-easy` в любом популярном крейте. Ревью от мейнтейнеров — лучший ускоритель роста.

### Ментальные установки

**Принимай, что первые 1–3 месяца будут болезненными.** Кривая обучения у Rust крутая в начале и пологая дальше. Это нормально. Все растовики проходили через стадию «ничего не компилируется».

**Учись думать в терминах типов, а не в терминах действий.** Rust поощряет «сделай невалидное состояние непредставимым»: используй `enum` для состояний, newtype для разных единиц измерения, type-state для гарантий компиляции.

**Не путай «сложно» и «непривычно».** Lifetimes выглядят страшно, но они описывают то, что в C++ существует неявно — просто Rust заставляет это записать. Через пару месяцев они станут естественной частью мышления.

**Сравнивай Rust осознанно.** «В Go проще» или «в Haskell мощнее» — обе мысли верны и обе бесполезны. Rust выбирает компромисс между производительностью, безопасностью и эргономикой. Понимание этого компромисса важнее, чем выбор «лучшего языка».

### Где тормозить

**Не лезь в `unsafe` и FFI слишком рано.** Это мощные, но опасные инструменты. Освой safe Rust до автоматизма прежде, чем брать ответственность за инварианты, которые компилятор больше не проверяет.

**Не начинай с async.** Многие пытаются стартовать с tokio и веб-серверов. Async добавляет ещё один слой сложности (Pin, Future, runtime) поверх и без того нетривиальной модели владения. Сначала — синхронный код, потоки, каналы. Async — после уверенного владения этапами 1–6.

**Не увлекайся продвинутыми трейтами без необходимости.** GAT, HRTB, type-state — мощные инструменты, но в 95% production-кода их не нужно. Сначала научись писать обычный идиоматичный Rust.

### Полезные ресурсы для практики

- **[Rustlings](https://github.com/rust-lang/rustlings)** — упражнения по синтаксису.
- **[Exercism](https://exercism.org/tracks/rust)** — задачи с ментор-ревью.
- **[Rust by Example](https://doc.rust-lang.org/rust-by-example/)** — паттерны через примеры.
- **[Rust Playground](https://play.rust-lang.org/)** — экспериментировать без `cargo new`.
- **[Advent of Code](https://adventofcode.com/) на Rust** — отличная сезонная практика.
- **[Codewars](https://www.codewars.com/?language=rust)** и **[LeetCode](https://leetcode.com/) на Rust** — алгоритмические задачи.
- **[Rust Quiz](https://dtolnay.github.io/rust-quiz/)** — каверзные вопросы по семантике.
