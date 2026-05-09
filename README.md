# rust-roadmap-ru
🦀 Полный roadmap по изучению Rust на русском + большой список ресурсов. Telegram: [t.me/rust_code](https://t.me/+HelhY88sArowYjgy)
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
