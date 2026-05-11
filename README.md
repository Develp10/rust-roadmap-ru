🦀 Rust Roadmap (на русском)

Полный курс по языку программирования Rust на русском языке: от первой строчки кода до асинхронности, embedded и WebAssembly. Каждый урок состоит из теории, практики и теста с ответами.

📢 [t.me/rust_code](https://t.me/+HelhY88sArowYjgy) — основной Telegram-канал с уроками и разбором кода для Rust-разработчиков.

## 📚 О курсе

- **25 уроков** — от базового синтаксиса до прикладной разработки.
- Каждый урок построен по одной схеме: **теория с примерами → практика с задачами → тест → разбор ответов**.
- Только **Rust 2021 / 2024** — современный идиоматичный код, актуальные крейты экосистемы.
- Без воды — только практически полезный материал.

## 🗺️ Дорожная карта

### Часть 1. Основы (уроки 1–8)

| № | Урок | Темы |
|---|------|------|
| 1 | Введение в Rust | установка, cargo, hello world, структура проекта |
| 2 | Переменные и типы | `let` / `mut`, целые / плавающие, `bool`, `char`, кортежи, массивы, преобразования, переполнение |
| 3 | Управление потоком | `if` / `else`, `loop`, `while`, `for`, `match`, `let-else`, loop labels |
| 4 | Владение (ownership) | `move`, `Copy`, `Clone`, scope, `Drop`, замыкания и захват |
| 5 | Заимствование и ссылки | `&T`, `&mut T`, NLL, reborrow, elision, времена жизни |
| 6 | Slices, String, Vec | срезы, UTF-8, capacity, итерация по строкам |
| 7 | Structs и enums | tuple / unit-структуры, `derive`, `match` на enum, niche, builder |
| 8 | Generics и traits | мономорфизация, `dyn` vs `impl`, trait bounds, ассоциированные типы, перегрузка операторов |

### Часть 2. Углублённые темы (уроки 9–14)

| № | Урок | Темы |
|---|------|------|
| 9 | Lifetimes | элизия, `'static`, HRTB, структуры со ссылками |
| 10 | Обработка ошибок | `Result` / `Option`, оператор `?`, `panic`, `thiserror`, `anyhow`, FFI |
| 11 | Коллекции и итераторы | `HashMap`, `BTreeMap`, `VecDeque`, `BinaryHeap`, кастомные итераторы |
| 12 | Умные указатели | `Box`, `Rc`, `Arc`, `RefCell`, `Mutex` / `RwLock`, `OnceLock`, `Cow`, `Weak` |
| 13 | Многопоточность | потоки, `Send` / `Sync`, каналы, scoped threads |
| 14 | Async и Tokio | `Future`, `async` / `await`, рантайм tokio, `select!`, каналы |

### Часть 3. Продвинутые возможности (уроки 15–19)

| № | Урок | Темы |
|---|------|------|
| 15 | Макросы | `macro_rules!`, процедурные макросы, `derive` |
| 16 | Тестирование | unit, integration, doc-tests, property-based |
| 17 | Экосистема Cargo | workspaces, features, profiles, публикация |
| 18 | Unsafe и FFI | `unsafe`, raw-указатели, `extern "C"`, `bindgen` |
| 19 | Веб с axum | роутинг, extractors, middleware, JSON, state |

### Часть 4. Прикладные направления (уроки 20–25)

| № | Урок | Темы |
|---|------|------|
| 20 | Базы данных: sqlx и diesel | пулы, миграции, транзакции, repository |
| 21 | gRPC с tonic | protobuf, streaming, interceptors, TLS |
| 22 | Embedded и `no_std` | `core` / `alloc`, `embedded-hal`, RTIC, Embassy |
| 23 | WebAssembly | `wasm-bindgen`, `web-sys`, Yew / Leptos, WASI |
| 24 | Паттерны проектирования | newtype, builder, typestate, strategy, RAII |
| 25 | CLI-приложения | clap, anyhow, tracing, indicatif, ratatui |

## 🚀 Как учиться

1. Установите Rust с [rustup.rs](https://rustup.rs) и любой редактор с поддержкой `rust-analyzer` (VS Code, Helix, Zed, RustRover).
2. Идите по урокам последовательно: каждый следующий опирается на предыдущий.
3. Для каждого урока: **прочитайте теорию → разберите код практики → решите задачи самостоятельно → пройдите тест и сверьтесь с ответами**.
4. Создавайте `cargo new` песочницу под каждый урок и экспериментируйте.

## 🛠️ Что понадобится

- **Rust 1.75+** (`rustup update stable`).
- **cargo** — встроенный пакетный менеджер.
- **rust-analyzer** — LSP для подсветки и автодополнения.
- Терминал, базовое знакомство с Git и любым другим языком (для контекста).

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

---

# 📚 Полезные ресурсы

## 📺 Telegram-каналы и подборки

- [t.me/rust_code](https://t.me/+HelhY88sArowYjgy) — основной канал по Rust на русском, уроки и разборы кода.
- [t.me/books_englishhh](https://t.me/books_englishhh) — англоязычные книги и материалы по Rust.
- [t.me/ai_machinelearning_big_data](https://t.me/ai_machinelearning_big_data) — AI, ML, Big Data (полезно при работе с `candle`, `burn`, `tch-rs`).
- [Папка с обучающими ресурсами](https://t.me/addlist/8vDja6L4G_dlMjli) — большая подборка каналов для разработчиков.

## 📖 Статьи на русском (must-read для глубокого понимания)

- [Твой код на Rust компилируется, проходит тесты и является UB. Ты просто об этом не знаешь](https://habr.com/ru/articles/848134/) — про скрытые ловушки undefined behavior в безопасном на вид Rust-коде.
- [Как Rust обманывает процессор: тайная жизнь niche-оптимизации, drop flags и MIR](https://habr.com/ru/articles/871556/) — внутренности компилятора и неожиданные оптимизации.
- [Как Rust обманывает процессор. Часть 2: niche сквозь крейты, dropck, Pin и провенанс указателей](https://habr.com/ru/articles/873330/) — продолжение про Pin, dropck и провенанс.

## 📘 Официальная документация

- [The Rust Book](https://doc.rust-lang.org/book/) — главная книга, начинать отсюда. [Русский перевод](https://doc.rust-lang.ru/book/).
- [Rust Book с интерактивными квизами](https://rust-book.cs.brown.edu/) — экспериментальная версия от Brown University.
- [Rust by Example](https://doc.rust-lang.org/rust-by-example/) — концепции через работающие примеры.
- [The Rustonomicon](https://doc.rust-lang.org/nomicon/) — тёмная сторона: `unsafe`, lifetimes, layout.
- [The Rust Reference](https://doc.rust-lang.org/reference/) — формальная спецификация языка.
- [Rust Async Book](https://rust-lang.github.io/async-book/) — асинхронность от создателей языка.
- [Rust API Guidelines](https://rust-lang.github.io/api-guidelines/) — как проектировать идиоматичные API.
- [std documentation](https://doc.rust-lang.org/std/) — стандартная библиотека (читать даже без запроса!).

## 🎓 Бесплатные книги и курсы онлайн

- [Rust Atomics and Locks](https://marabos.nl/atomics/) — Mara Bos, бесплатно онлайн. Про многопоточность и memory ordering.
- [Comprehensive Rust](https://google.github.io/comprehensive-rust/) — курс от Google, 4 дня интенсива.
- [100 Exercises To Learn Rust](https://rust-exercises.com/) — Luca Palmieri.
- [Easy Rust](https://dhghomon.github.io/easy_rust/) — простыми словами для начинающих.
- [Too Many Linked Lists](https://rust-unofficial.github.io/too-many-lists/) — учим Rust через реализацию связных списков (классика!).
- [Writing an OS in Rust](https://os.phil-opp.com/) — Philipp Oppermann, минимальная ОС с нуля.
- [Crafting Interpreters](https://www.craftinginterpreters.com/) — пишем интерпретатор (оригинал на Java/C, есть множество Rust-портов).
- [Zero To Production In Rust](https://www.zero2prod.com/) — Luca Palmieri, веб-сервисы (платная, но стоит того).

## 🎓 Продвинутые платные курсы 

- [Rust продвинутый: лайфтаймы, async, unsafe и производительность](https://stepik.org/a/285608/pay?promo=e1a7a808ed40a7be) — Внутри: async, unsafe, gRPC, lock-free, observability, Kafka, NATS, axum, tower, CI/CD и канареечный деплой. 21 модуль, 84 урока, 400+ проверочных шагов.
- [Rust полный курс разработчика! С нуля до профи"](https://stepik.org/a/269250/pay?promo=b36f020f77bfb2d5) — 6 модулей, 50 уроков, 143 теста. Ownership, borrowing, traits, async, Tokio, Axum, макросы, WASM — всё разложено по полочкам и закреплено практикой.
- 
## 💪 Практика и упражнения

- [Rustlings](https://github.com/rust-lang/rustlings) — 90+ интерактивных упражнений, обязательно в начале.
- [Exercism Rust Track](https://exercism.org/tracks/rust) — задачи с менторскими ревью.
- [Rust Quiz](https://dtolnay.github.io/rust-quiz/) — каверзные вопросы по семантике от dtolnay.
- [Rust Playground](https://play.rust-lang.org/) — экспериментировать без `cargo`, смотреть MIR / LLVM IR / ASM.
- [Codewars](https://www.codewars.com/) — алгоритмические ката.
- [Advent of Code](https://adventofcode.com/) — ежегодный декабрьский марафон, отлично заходит на Rust.
- [CodeCrafters](https://codecrafters.io/) — пиши свой Redis / Git / Docker на Rust.

## 📰 Блоги и новости

- [This Week in Rust](https://this-week-in-rust.org/) — еженедельный дайджест экосистемы. Подписка обязательна.
- [Inside Rust Blog](https://blog.rust-lang.org/inside-rust/) — что происходит внутри команды разработчиков.
- [fasterthanli.me](https://fasterthanli.me/) — Amos, глубокие технические статьи.
- [Without Boats](https://without.boats/) — блог одного из ключевых дизайнеров async Rust.
- [Niko Matsakis](https://smallcultfollowing.com/babysteps/) — сооснователь языка, пишет про типовую систему.
- [Manish Goregaokar](https://manishearth.github.io/) — глубокие посты про unsafe и FFI.

## 🎥 YouTube-каналы

- [Jon Gjengset](https://www.youtube.com/c/JonGjengset) — многочасовые стримы по внутренностям (Crust of Rust).
- [No Boilerplate](https://www.youtube.com/c/NoBoilerplate) — короткие мотивирующие видео про Rust.
- [Logan Smith](https://www.youtube.com/@_noisecode) — продвинутые темы доступным языком.
- [Let's Get Rusty](https://www.youtube.com/c/LetsGetRusty) — туториалы для начинающих.
- [Code to the Moon](https://www.youtube.com/@codetothemoon) — обзоры экосистемы и крейтов.
- [chris biscardi](https://www.youtube.com/@chrisbiscardi) — bevy, leptos, фронтенд на Rust.

## 🔧 Инструменты, без которых жить нельзя

- [`rust-analyzer`](https://rust-analyzer.github.io/) — LSP, основа продуктивности.
- [`cargo-edit`](https://github.com/killercup/cargo-edit) — `cargo add` / `rm` / `upgrade`.
- [`cargo-watch`](https://github.com/watchexec/cargo-watch) — авто-перезапуск при изменениях.
- [`cargo-expand`](https://github.com/dtolnay/cargo-expand) — посмотреть, во что разворачиваются макросы.
- [`cargo-nextest`](https://nexte.st/) — быстрый тест-раннер.
- [`bacon`](https://dystroy.org/bacon/) — фоновый компилятор-watcher.

## 🌐 Сообщество

- [@rust_chats](https://t.me/rust_chats) — чат русскоязычных Rust-разработчиков (вопросы, помощь, обсуждение).
- [Группа ВК «Rust IT»](https://vk.com/rust_it) — русскоязычное сообщество ВКонтакте: новости, статьи, вакансии.
- [Rust Users Forum](https://users.rust-lang.org/) — лучшее место для вопросов новичков.
- [Rust Internals](https://internals.rust-lang.org/) — обсуждение развития языка.
- [r/rust](https://www.reddit.com/r/rust/) — Reddit-сообщество.
- [Rust Discord](https://discord.gg/rust-lang) — официальный Discord.
- [Rust Zulip](https://rust-lang.zulipchat.com/) — где сидят разработчики компилятора.
- [Are We X Yet?](https://wiki.mozilla.org/Areweyet) — обзоры зрелости экосистемы (Are We Web Yet?, Are We Game Yet?, Are We Async Yet?).

## 🦀 Куратор-листы (awesome-lists)

- [awesome-rust](https://github.com/rust-unofficial/awesome-rust) — главный список крейтов и проектов.
- [Not Yet Awesome Rust](https://github.com/not-yet-awesome-rust/not-yet-awesome-rust) — чего пока не хватает в экосистеме (повод законтрибьютить).
- [Awesome Embedded Rust](https://github.com/rust-embedded/awesome-embedded-rust) — embedded.
- [Awesome WebAssembly](https://github.com/mbasso/awesome-wasm) — WASM.

---

# 🦀 Roadmap по изучению Rust: от нуля до профи

Этот путь рассчитан примерно на **6–12 месяцев** активного обучения. Каждый этап отвечает на три вопроса: **что** изучать, **где** изучать и **зачем** это нужно. Если ты совсем новичок — не перепрыгивай, порядок выбран не случайно.

> 💡 **Общее правило для новичков:** прочитай тему в *The Rust Book* → прочитай её же в *Rust by Example* → реши соответствующие упражнения в *Rustlings*. Три угла зрения на одну идею закрепляют её сильно лучше, чем один источник.

## Этап 0. Подготовка окружения (1–3 дня)

**Что изучать:**

- Установить [`rustup`](https://rustup.rs) — официальный менеджер тулчейнов. Он ставит компилятор (`rustc`), пакетный менеджер (`cargo`), форматтер (`rustfmt`), линтер (`clippy`) и позволяет переключаться между `stable` / `beta` / `nightly` (`rustup default stable`, `rustup toolchain install nightly`).
- Зафиксировать версию для проекта через файл `rust-toolchain.toml` в корне репозитория — чтобы все участники и CI собирали одним и тем же компилятором.
- **Редактор:** VS Code + расширение [rust-analyzer](https://rust-analyzer.github.io/) — самая дружелюбная связка для новичков. Альтернативы: [RustRover](https://www.jetbrains.com/rust/) (бесплатен для некоммерческого использования), Helix, Zed, Neovim + LSP. Включи в `settings.json` `"editor.formatOnSave": true` и `"rust-analyzer.check.command": "clippy"` — будет ловить проблемы прямо в редакторе.
- **Ежедневные команды cargo:**
  - `cargo new` / `cargo init` — создание проекта;
  - `cargo check` — быстрая проверка типов без кодогенерации (используй пока пишешь, в 5–10 раз быстрее `build`);
  - `cargo build` (debug) / `cargo build --release` (с оптимизациями, разница в скорости часто 10–100×);
  - `cargo run` / `cargo run --release -- arg1 arg2` (всё после `--` уходит в твою программу);
  - `cargo test` / `cargo test -- --nocapture` (показывать вывод даже у прошедших тестов);
  - `cargo doc --open` — генерирует и открывает документацию проекта со всеми зависимостями (включая stdlib);
  - `cargo add serde --features derive` — добавить зависимость без ручной правки `Cargo.toml`;
  - `cargo clippy -- -D warnings` — линтер; флаг превращает все варнинги в ошибки (полезно в CI).
- **`Cargo.toml` минимум, который надо понимать:** `[dependencies]`, `[dev-dependencies]`, `[build-dependencies]`, секции `[profile.dev]` / `[profile.release]`, фичи (`features = ["derive", "json"]`).
- **Полезные cargo-расширения сразу:** `cargo install cargo-watch cargo-edit cargo-expand`. `cargo watch -x check -x test` — пересборка/тесты на сохранение файла, экономит часы.

**Где изучать:**

- [rustup.rs](https://rustup.rs) — установка одной командой.
- [The Rust Book, гл. 1](https://doc.rust-lang.ru/book/ch01-00-getting-started.html).
- [The Cargo Book](https://doc.rust-lang.org/cargo/) — особенно главы «Specifying Dependencies», «Cargo.toml format», «Profiles».
- Шпаргалка по cargo-командам: [cheats.rs/#cargo](https://cheats.rs/#cargo).

**Зачем это нужно:** без рабочего окружения и правильных рефлексов (`cargo check` пока пишешь, `cargo doc --open` чтобы читать доки оффлайн, `cargo clippy` перед коммитом) Rust кажется сильно сложнее, чем есть. Один час, потраченный сейчас на настройку, экономит десятки часов на следующих этапах.

**Мини-проект:** `Hello, world!`, который читает имя из `std::env::args`, валидирует, что аргумент задан (если нет — выводит usage и завершает с кодом 1), и здоровается. Бонус: добавь зависимость `anyhow` через `cargo add` и используй `anyhow::Result` как тип возврата `main`.

---

## Этап 1. Базовый синтаксис (2–3 недели)

**Что изучать:**

- **Переменные:** `let` (по умолчанию неизменяемые), `mut`, константы (`const NAME: T = ...` — всегда с типом, должны быть compile-time), `static` (живёт всю программу). Shadowing — повторное связывание имени через `let` — **идиома, а не баг**: позволяет менять тип (`let x = "5"; let x: u32 = x.parse()?;`).
- **Целые типы:** `i8/i16/i32/i64/i128/isize` и `u8/u16/u32/u64/u128/usize`. `usize`/`isize` зависят от платформы (32 или 64 бит) и используются для индексации. **Переполнение:** в debug-сборке — `panic`, в release — wrap (тихая обёртка). Для явного поведения — `checked_add`, `saturating_add`, `wrapping_add`, `overflowing_add`.
- **Числовые литералы:** `1_000_000` (подчёркивания для читаемости), `0xff`, `0b1010`, `0o755`, суффиксы `100u32`, `1.5f64`.
- **`bool`, `char`** (4-байтный Unicode scalar value, **не байт!**), кортежи `(i32, &str)`, массивы `[T; N]` (размер в типе), срезы `&[T]` / `&str` (динамический размер, fat pointer).
- **Преобразования:** `as` — низкоуровневый каст (может молча терять биты); `From`/`Into` — безопасное расширение (`u32 → u64`); `TryFrom`/`TryInto` — может не получиться (`i64 → i32`). Идиома: предпочитай `From`/`TryFrom` вместо `as` везде, кроме horizontalных кастов внутри числовой работы.
- **Строки:** `String` (владеющая, на куче, мутабельная) vs `&str` (срез, обычно ссылка на литерал или часть `String`). Литералы `"..."` имеют тип `&'static str`. **Грабли:** `String` нельзя индексировать как `s[0]` — UTF-8 переменной длины; используй `.chars()`, `.bytes()`, `.char_indices()`.
- **Управляющие конструкции — это выражения, не операторы:**
  ```rust
  let max = if a > b { a } else { b };
  let sum = loop { break 42; };
  ```
  `if`, `match`, `loop`, блоки `{ ... }` — все возвращают значение. Точка с запятой превращает выражение в оператор (отбрасывает значение).
- **`match`** — главный инструмент Rust для разбора данных. Должен быть **исчерпывающим**: компилятор требует покрыть все варианты (или `_`).
- **Функции:** возврат — последнее выражение без `;`, либо `return`. Параметры по значению переносят владение (move), по `&T` — занимают.
- **Модули и видимость:** `mod`, `pub`, `pub(crate)`, `pub(super)`. `use crate::foo::Bar`. Файловая раскладка: `mod foo;` ищет `foo.rs` или `foo/mod.rs` (новый стиль — `foo.rs` + `foo/`).
- **Doc-комментарии:** `///` для элементов, `//!` для модулей/крейтов. `cargo doc` рендерит в HTML; примеры внутри них автоматически становятся **doc-тестами** и запускаются на `cargo test`.

**Где изучать:**

- [The Rust Book, гл. 3–7](https://doc.rust-lang.ru/book/ch03-00-common-programming-concepts.html) — основа основ. Прочитать до конца, не перепрыгивая.
- [Rust by Example](https://doc.rust-lang.org/rust-by-example/) — параллельно с книгой, как «практический режим».
- [Rustlings](https://github.com/rust-lang/rustlings) — упражнения, включая `variables`, `functions`, `primitive_types`, `vecs`, `move_semantics`, `structs`, `enums`, `strings`. Делать **все**, не пропуская.
- [cheats.rs](https://cheats.rs/) — однострочная шпаргалка по всему синтаксису, держать открытой.

**Зачем это нужно:** на этом этапе закладывается ритм работы с компилятором: пишешь → `cargo check` → читаешь ошибку → правишь. Все остальные этапы предполагают, что синтаксис у тебя уже не вызывает заминок.

**Мини-проекты:** калькулятор в CLI с `+ - * /`, конвертер температур °C↔°F с валидацией ввода, FizzBuzz, угадай-число (с `std::io` для ввода и `rand` для случайного числа).

---

## Этап 2. Владение, заимствование, времена жизни (3–4 недели) — самое важное!

**Что изучать:**

- **Модель памяти.** Стек (быстрый, фиксированный размер) vs куча (`Box`, `Vec`, `String` — динамический размер, аллокация дороже). Понимание, что где лежит, объясняет 90% «почему компилятор недоволен».
- **Ownership — центральная идея языка:** у каждого значения ровно один владелец; когда владелец выходит из области видимости, вызывается `Drop`, ресурс освобождается. Без GC, без ручного `free`. Это работает для **любого ресурса**, не только памяти: файловые дескрипторы, сокеты, блокировки — всё освобождается автоматически (RAII).
- **Move-семантика.** Присваивание или передача значения по типу, не реализующему `Copy`, **переносит** владение; использование исходной переменной после move — ошибка компиляции. Так Rust ловит use-after-free и double-free на этапе сборки.
- **`Copy` vs `Clone`.** `Copy` — побитовое копирование, неявное (`i32`, `bool`, `char`, фиксированные массивы из `Copy`-типов). `Clone` — явная, потенциально дорогая копия (`.clone()`, всегда видно в коде). Тип не может быть `Copy`, если содержит ресурс с `Drop` (например, `String`).
- **Заимствование.** `&T` — общая ссылка (только чтение, можно много); `&mut T` — эксклюзивная ссылка (один писатель). **Borrow checker гарантирует:** либо много читателей, либо один писатель. Это устраняет data races на этапе компиляции.
- **Времена жизни (lifetimes).** Каждой ссылке компилятор приписывает «живёт не дольше, чем». В простых функциях лайфтаймы выводятся автоматически (**lifetime elision rules** — три правила в Rust Reference). Явная аннотация `fn longest<'a>(x: &'a str, y: &'a str) -> &'a str` нужна, когда выходная ссылка зависит от нескольких входных.
- **Особый лайфтайм `'static`** — живёт всю программу. У строковых литералов тип `&'static str`. Bound `T: 'static` означает «тип не содержит ссылок с более коротким временем жизни» (это **не** «тип живёт вечно», частая путаница).
- **Слайсы.** `&[T]`, `&mut [T]`, `&str` — нормальная форма передачи коллекций в функции; принимай слайсы вместо `&Vec<T>` / `&String`. Это работает и для `Vec`, и для массивов, и для подсрезов `&v[2..5]`.
- **NLL (Non-Lexical Lifetimes).** Современный borrow checker отслеживает использование, а не лексические скоупы. Поэтому `let r = &v; println!("{r}"); v.push(1);` компилируется (последнее использование `r` — в `println!`).
- **Что *нельзя* в safe Rust из-за ownership:** self-referential structs (структура со ссылкой на собственное поле — нужны `Pin` и продвинутые паттерны), произвольные графы со взаимными ссылками без `Rc`/`Weak`, мутабельные глобалы без `Mutex`/`OnceLock`. Это не баги, а сознательные ограничения.

**Где изучать:**

- [The Rust Book, гл. 4 (Понимание владения) и гл. 10 §3 (Lifetimes)](https://doc.rust-lang.ru/book/ch04-00-understanding-ownership.html) — каноническое объяснение. Прочитать **дважды**, с интервалом в неделю.
- [Rust by Example: Scoping rules, Borrowing, Lifetimes](https://doc.rust-lang.org/rust-by-example/scope.html).
- [Rustlings: `move_semantics`, `lifetimes`, `strings`](https://github.com/rust-lang/rustlings).
- [Rust Book с интерактивными квизами от Brown University](https://rust-book.cs.brown.edu/) — те же главы плюс квизы, ловящие типичные заблуждения именно про ownership. Сильно рекомендую.
- [Crust of Rust: «Lifetime Annotations» (Jon Gjengset)](https://www.youtube.com/watch?v=rAl-9HwD858) — когда почувствуешь готовность к более глубокому разбору.
- The Rust Reference: [Lifetime elision](https://doc.rust-lang.org/reference/lifetime-elision.html) — формальные правила.

**Зачем это нужно:** именно ownership и lifetimes делают Rust *Rust*. Почти все «почему мой код не компилируется?!» сводятся к ним. Когда щёлкнет (обычно к 3-й неделе ежедневной практики), остальной язык становится лёгким.

**Типичные грабли на этом этапе:**

- **«Я просто скопирую через `.clone()`».** Работает, но это анти-паттерн в горячем коде. Сначала попытайся передать ссылку.
- **«Не могу мутировать `Vec`, пока итерирую».** Правильно: либо собрать индексы и потом изменить, либо использовать `Vec::retain` / `drain_filter`.
- **`return &local_var`** — возврат ссылки на локальную переменную. Не компилируется, и слава богу.

**Мини-проекты:** функция `longest(&str, &str) -> &str` (из книги, но дописать варианты с разными лайфтаймами), парсер CSV (передача слайсов между функциями без копирования), стековый калькулятор на `Vec<f64>` с обработкой ошибок.

---

## Этап 3. Структуры данных и абстракции (3 недели)

**Что изучать:**

- **`struct`:** с именованными полями, tuple-структуры (`struct Point(f64, f64)`), unit-структуры (для маркеров и реализации трейтов). Деструктуризация: `let Point { x, y } = p;`.
- **`enum`** — это **sum types**, а не «нумерация констант». Каждый вариант может нести данные разного типа: `enum Event { Click { x: i32, y: i32 }, Scroll(f32), Resize }`. Так моделируются «значение — одно из этих вариантов» — основной инструмент для конечных автоматов и доменных моделей.
- **`Option<T>` и `Result<T, E>`** — ответ Rust на `null` и исключения. Оба — **обычные enum'ы из stdlib**, никакой магии. `Option::None` вместо `null`, `Result::Err` вместо `throw`.
- **Pattern matching:** `match`, `if let`, `while let`, `let-else` (Rust 1.65+: `let Some(x) = opt else { return; };`). Так разбирают enum'ы и **делают невалидные состояния непредставимыми**.
- **Exhaustive matching и `#[non_exhaustive]`.** `match` по `enum` обязан покрыть все варианты. Атрибут `#[non_exhaustive]` на публичном enum заставляет потребителей всегда писать `_ => ...` — позволяет добавлять варианты без breaking-изменений.
- **`impl`-блоки:** методы (`&self`, `&mut self`, `self` — последний потребляет значение), ассоциированные функции (без `self`, например `String::new()`, `Vec::with_capacity(10)`).
- **Трейты — интерфейсы Rust.** Дефолтные реализации методов; авто-вывод стандартных через `#[derive(Debug, Clone, PartialEq, Eq, Hash, Default)]`. Важные стандартные: `Debug`/`Display`, `PartialEq`/`Eq`, `PartialOrd`/`Ord`, `Hash`, `Default`, `Iterator`, `From`/`Into`.
- **`From` / `Into` / `TryFrom` / `TryInto`.** Реализуй `From` — `Into` появится автоматически. Идиома: всегда `impl From<X> for Y`, никогда `impl Into` (orphan rule + удобство для пользователей).
- **Дженерики:** `fn largest<T: Ord>(items: &[T]) -> &T`. Trait bounds (`T: Ord`), `where`-клаузы для читаемости при многих ограничениях, `impl Trait` в позиции аргумента/возврата (для краткости).
- **Trait objects:** `dyn Trait`, `Box<dyn Trait>`, `&dyn Trait` — runtime-полиморфизм. **Статический диспатч** (дженерики, `impl Trait`) — компилятор делает копии под каждый тип, нет накладных расходов; **динамический** (`dyn Trait`) — vtable + указатель, удобно для гетерогенных коллекций `Vec<Box<dyn Trait>>`.
- **Object safety.** Не каждый трейт можно превратить в `dyn Trait`: запрещены generic-методы, `Self` в позиции возврата, ассоциированные константы. Если `dyn Trait` не компилируется — почти всегда нарушено object safety.
- **Orphan rule и blanket impls.** Чтобы реализовать `Trait for Type`, либо trait, либо type должны быть из твоего крейта. Blanket impl: `impl<T: Display> ToString for T` — реализация для всех подходящих типов сразу.
- **Итераторы и замыкания.** `Iterator` — самый используемый трейт. Цепочки `.iter().filter().map().collect()` — идиоматичный Rust. Замыкания: `Fn` (читает захваты), `FnMut` (мутирует), `FnOnce` (потребляет — например, `move`-замыкание со строкой).
- **Коллекции stdlib:** `Vec<T>` (динамический массив), `HashMap<K, V>` (хеш-таблица), `HashSet<T>`, `BTreeMap`/`BTreeSet` (упорядоченные), `VecDeque` (двухсторонняя очередь). Знай асимптотику основных операций.

**Где изучать:**

- [The Rust Book, гл. 5–10, 13](https://doc.rust-lang.ru/book/) — структуры, enum'ы, generics, trait'ы, lifetimes (повторно), итераторы и замыкания.
- [Rust by Example: Custom Types, Conversion, Generics, Traits, Closures, Iterators](https://doc.rust-lang.org/rust-by-example/).
- [Rustlings: `structs`, `enums`, `options`, `error_handling`, `generics`, `traits`, `lifetimes`, `iterators`](https://github.com/rust-lang/rustlings).
- [Rust API Guidelines](https://rust-lang.github.io/api-guidelines/) — чтобы сразу делать идиоматичные публичные типы.
- [«Effective Rust»](https://www.lurklurk.org/effective-rust/) — главы 1–8 идеально ложатся именно на этот этап.

**Зачем это нужно:** struct + enum + traits + iterators — это то, как ты *моделируешь* предметную область в Rust. Когда начнёшь использовать `enum` для состояний и `Option` вместо `null`, целые классы багов исчезают на уровне типов.

**Мини-проекты:** телефонная книга в CLI (потом — сохранение/загрузка в JSON через `serde`), CLI todo-list, простой граф (`HashMap<NodeId, Vec<NodeId>>`) с поиском в ширину/глубину, парсер арифметических выражений через `enum Expr { Num(f64), Add(Box<Expr>, Box<Expr>), ... }` с интерпретатором.

---

## Этап 4. Обработка ошибок и тестирование (1–2 недели)

**Что изучать:**

- **`panic!`, `unwrap`, `expect`** — *быстрый и грязный* способ: программа падает при ошибке. Норм для прототипов, примеров из Rust Book, инвариантов «здесь по построению точно `Some`». **Не норм** для production-логики и публичных API библиотек.
- **Идиоматичная обработка:** `Result<T, E>` везде, **оператор `?`** для проброса ошибки одним символом. `?` автоматически вызывает `From`, конвертируя один тип ошибки в другой.
- **Свои типы ошибок через [`thiserror`](https://github.com/dtolnay/thiserror)** — для **библиотек**, где потребителям нужно матчить конкретные варианты:
  ```rust
  #[derive(thiserror::Error, Debug)]
  pub enum MyError {
      #[error("io: {0}")]
      Io(#[from] std::io::Error),
      #[error("parse: bad value at line {line}")]
      Parse { line: usize },
  }
  ```
- **[`anyhow`](https://github.com/dtolnay/anyhow)** — для **приложений**: `anyhow::Result<T>`, `anyhow!` для создания ошибки из строки, `.context("loading config")` для добавления контекста. Конкурент: [`color-eyre`](https://github.com/eyre-rs/eyre) — то же самое, но с красивым выводом и поддержкой кастомных хендлеров.
- **Правило большого пальца:** библиотеки → `thiserror` (точные enum-ошибки); бинари / приложения → `anyhow`/`eyre` (контекст + цепочка причин). Не смешивай в одном крейте.
- **Конверсии `From` / `Into`** — как `?` автоматически превращает `io::Error` в `MyError`. `#[from]` в `thiserror` генерирует `impl From` за тебя.
- **`Result::map_err`, `.context()`, `.with_context(|| ...)`** — добавление контекста к ошибке без потери исходной причины (`source` chain).
- **Panic vs Result — когда что:** Panic — невосстановимая ошибка (битый инвариант, баг). Result — ожидаемая (файла нет, parse failed). `unwrap()` в production допустим только там, где доказать, что значение есть, проще, чем переписать код.
- **Тестирование:**
  - Юнит-тесты в `mod tests { #[test] fn name() { ... } }` рядом с кодом.
  - Интеграционные — в `tests/` (отдельные крейты, видят только публичный API).
  - **Doc-тесты** — примеры в `///`-комментариях запускаются как тесты на `cargo test`. Сразу и документация, и проверка.
  - `#[should_panic(expected = "...")]`, `#[ignore]` (для долгих, запуск через `cargo test -- --ignored`).
- **Property-based тестирование:** [`proptest`](https://github.com/proptest-rs/proptest) или [`quickcheck`](https://github.com/BurntSushi/quickcheck). Вместо «проверить пару примеров» — задаёшь свойство и крейт генерирует сотни случайных входов, ищет минимальный контрпример.
- **Snapshot-тесты:** [`insta`](https://github.com/mitsuhiko/insta) — особенно удобно для сложного вывода (JSON, HTML, дерева AST). Запустил, проверил глазами, утвердил — дальше тест ловит регрессии.
- **Бенчмарки:** [`criterion`](https://github.com/bheisler/criterion.rs) — статистически грамотные бенчи (встроенный `#[bench]` доступен только на nightly и проигрывает `criterion` по выводу и шуму).

**Где изучать:**

- [The Rust Book, гл. 9 (ошибки), 11 (тесты), 14 §3 (doc-тесты)](https://doc.rust-lang.ru/book/ch09-00-error-handling.html).
- [README `thiserror`](https://github.com/dtolnay/thiserror) и [README `anyhow`](https://github.com/dtolnay/anyhow) — короткие, прочитать оба.
- Статья: [«Error Handling In Rust — A Deep Dive» (Luca Palmieri)](https://www.lpalmieri.com/posts/error-handling-rust/) — лучший разбор за один заход.
- [«Beginner's guide to property-based testing in Rust»](https://altsysrq.github.io/proptest-book/intro.html).

**Зачем это нужно:** ошибки — это половина боевого кода. `Result + ?` делает их обработку приятной *и* явной — ты буквально не можешь молча проигнорировать ошибку. Тесты дают уверенность для рефакторинга; без них любое нетривиальное изменение страшно.

**Мини-проект:** CLI-утилита для валидации конфигов (JSON/TOML) — читает, парсит, возвращает список ошибок с номерами строк. Свой тип ошибки через `thiserror`, интеграционный тест на `tests/`, doc-тесты на публичные функции, `proptest` на парсер.

---

## Этап 5. Умные указатели и интериор-мутабельность (2 недели)

**Что изучать:**

- **`Box<T>`** — аллокация на куче. Нужен, когда тип имеет известный размер на компиляции, но ты хочешь его на куче. Классика — рекурсивные типы:
  ```rust
  enum List { Cons(i32, Box<List>), Nil }
  ```
  Без `Box` рекурсивный enum имел бы бесконечный размер. Также используется для `Box<dyn Trait>` и для возврата больших значений без копирования стека.
- **`Rc<T>`** — *Reference Counted*, разделяемое владение **в одном потоке**. Несколько владельцев, освобождается, когда последний уходит. `Rc::clone(&rc)` — дешёвая операция (инкремент счётчика), не глубокое копирование.
- **`Arc<T>`** — *Atomic Reference Counted*, потокобезопасная версия `Rc`. Чуть медленнее из-за атомарных операций. Используй ровно тогда, когда данные пересекают границу потоков.
- **Циклические ссылки и `Weak<T>`.** `Rc`/`Arc` **не освобождает** циклы — это утечка памяти (не UB, но плохо). Для деревьев с обратными ссылками (parent ← child) используй `Weak` для одного направления; `Weak::upgrade()` возвращает `Option<Rc<T>>`.
- **Интериор-мутабельность.** Как менять данные через `&T` (общую ссылку). Обычное правило Rust говорит «нельзя» — эти типы являются спасательными люками с runtime-проверкой:
  - **`Cell<T>`** — для `Copy`-типов, `get`/`set` по значению, без borrow. Очень дёшево, но только для `Copy`.
  - **`RefCell<T>`** — runtime-проверка borrow (`borrow()` / `borrow_mut()`); один поток. **Грабли:** `borrow_mut` panics, если уже есть активный borrow — это runtime-аналог borrow checker'а.
  - **`Mutex<T>`** — потокобезопасный, блокирует. `mutex.lock().unwrap()` — `unwrap` потому что lock возвращает `Result` (poisoning при панике в другом потоке).
  - **`RwLock<T>`** — много читателей или один писатель, потокобезопасно. На сильной contention может страдать от writer starvation — для горячих структур иногда лучше [`parking_lot::RwLock`](https://github.com/Amanieu/parking_lot) или `Mutex`.
  - **`OnceLock<T>`** (stable с 1.70) / **`OnceCell<T>`** (одно-поточная) — записать один раз, читать многократно. Идеальная замена `lazy_static!` для глобалов.
  - **`LazyLock<T>`** (stable с 1.80) / **`LazyCell<T>`** — ленивая инициализация при первом доступе. Современная замена `once_cell::Lazy`.
- **`Cow<'_, T>`** (Clone-On-Write) — `Cow::Borrowed(&str)` или `Cow::Owned(String)`. Полезно для функций, которые **обычно** не модифицируют вход, но иногда должны (например, нормализация строк).
- **`Pin<T>`** — гарантия, что значение не сдвинется в памяти. Нужен для self-referential типов (typically — для async-future). На этом этапе достаточно знать, что он существует и зачем; глубокое погружение — на этапе 13.
- **Drop, RAII, drop order.** Поля структуры дропаются в порядке объявления. `std::mem::drop(x)` — явный ранний дроп. Не реализуй `Drop` ради привычки — большинству типов он не нужен.
- **Когда что выбирать (шпаргалка):**
  - один владелец на стеке → обычное значение;
  - один владелец на куче → `Box<T>`;
  - несколько владельцев в потоке → `Rc<T>`;
  - несколько владельцев между потоками → `Arc<T>`;
  - мутация через общую ссылку, один поток → `RefCell` (или `Cell` для `Copy`);
  - мутация между потоками → `Mutex` / `RwLock` / атомики.

**Где изучать:**

- [The Rust Book, гл. 15 (умные указатели), гл. 16 (overlap с многопоточностью)](https://doc.rust-lang.ru/book/ch15-00-smart-pointers.html).
- [Rust by Example: Box, Rc, Cell/RefCell](https://doc.rust-lang.org/rust-by-example/std/box.html).
- Классика: [«Learn Rust With Entirely Too Many Linked Lists»](https://rust-unofficial.github.io/too-many-lists/) — реализуем 6 связных списков, каждый учит свой умный указатель. *Тот самый* туториал на этот этап, обязательно.
- [docs.rs/once_cell](https://docs.rs/once_cell/) — переходное чтение перед `OnceLock`/`LazyLock` из stdlib.

**Зачем это нужно:** без умных указателей Rust ощущается зажатым («как мне это пошарить?!»). С ними понимаешь, *почему* обычные правила Rust такие строгие — они дают статические гарантии, а эти типы — аккуратно подписанные исключения с явной runtime-стоимостью.

**Мини-проект:** двусвязный список (`Rc` + `RefCell` + `Weak`) или дерево, где дети знают родителя; кэш с `OnceLock` для ленивого вычисления тяжёлых констант.

---

## Этап 6. Многопоточность (2–3 недели)

**Что изучать:**

- **`std::thread::spawn`** — создаёт OS-поток. Замыкание захватывает данные; без `move` Rust не даст захватить переменные с лайфтаймом (нет гарантий, что они переживут поток).
- **`std::thread::scope`** (stable с 1.63) — **scoped threads**. Позволяет потокам безопасно заимствовать данные из родителя; scope ждёт завершения всех порождённых потоков. Современный дефолт для параллельной обработки локальных данных.
- **`Send` и `Sync`** — два маркерных трейта, на которых строится thread safety:
  - `T: Send` — значение можно **передать** в другой поток.
  - `T: Sync` — на значение можно **сослаться** из другого потока (`&T: Send`).
  - `Rc` — **не** `Send` (счётчик не атомарный); `Arc` — `Send`. Компилятор не даст пошарить `Rc` между потоками — гарантия, не предупреждение.
- **Разделяемое состояние:** `Arc<Mutex<T>>`, `Arc<RwLock<T>>` — каноническая обёртка для shared mutable state. Стандартные `Mutex`/`RwLock` на Linux реализованы через futex и разумно быстры; для интенсивно конкурируемых случаев — [`parking_lot`](https://github.com/Amanieu/parking_lot) (короче, быстрее, без poisoning).
- **Каналы (message passing).** Идиома Rust: «не шарьте память, передавайте сообщения».
  - `std::sync::mpsc` — multi-producer single-consumer, в stdlib.
  - [`crossbeam-channel`](https://github.com/crossbeam-rs/crossbeam) — быстрее, поддерживает `select`, bounded/unbounded.
  - [`flume`](https://github.com/zesterer/flume) — современная альтернатива `mpsc` с async-API.
- **Атомики:** `AtomicUsize`, `AtomicBool`, `AtomicPtr` — целочисленные операции без блокировок. **Memory ordering** (`Relaxed`, `Acquire`, `Release`, `AcqRel`, `SeqCst`) — глубокая тема; **новичкам** хватит правила «если не уверен — `SeqCst`, не теряя в корректности; оптимизировать упорядочения — после освоения этапов 13–14».
- **Параллельные итераторы — [`rayon`](https://github.com/rayon-rs/rayon).** `v.par_iter().map(...).sum()` — прозрачное распараллеливание data-parallel вычислений на work-stealing thread pool. Чаще всего этого достаточно: не надо вручную поднимать потоки.
- **Условные переменные и барьеры:** `Condvar`, `Barrier` — низкоуровневая синхронизация. На практике редки; обычно лучше канал.
- **Деадлоки.** Rust **не защищает** от deadlocks (защищает только от data races). Правило: всегда захватывай мьютексы в одном порядке; не вызывай чужой код, держа lock; используй `tokio::sync::Mutex` или `parking_lot::Mutex` без poisoning, если poisoning не нужен.
- **`thread_local!`** — данные, локальные для потока. Полезно для thread-local буферов и счётчиков.

**Где изучать:**

- [The Rust Book, гл. 16 (Бесстрашная многопоточность)](https://doc.rust-lang.ru/book/ch16-00-concurrency.html).
- [Rust Atomics and Locks (Mara Bos)](https://marabos.nl/atomics/) — бесплатно онлайн, *та самая* книга по теме. Читать после главы Rust Book — в этой книге плотный практический разбор атомиков с нуля и реализация `Mutex` своими руками.
- [README `rayon`](https://github.com/rayon-rs/rayon) и [его docs](https://docs.rs/rayon/) — короткие, сразу полезные.
- [«Crust of Rust: Atomics and Memory Ordering» (Jon Gjengset)](https://www.youtube.com/watch?v=rMGWeSjctlY).

**Зачем это нужно:** многопоточность — место, где большинство языков пропускают баги (data races, dangling references). Rust ловит data races на компиляции через `Send`/`Sync` — это настоящая суперсила, ради которой многие выбирают язык.

**Мини-проект:** параллельный word-counter (`rayon::par_iter` + объединение через `fold`/`reduce`); thread-pool с очередью задач через `crossbeam-channel`; параллельный обработчик изображений (поток-производитель читает с диска, пул-потребителей применяет фильтр).

---

## Этап 7. Асинхронное программирование (3–4 недели)

**Что изучать:**

- **Трейт `Future`** — async-кирпичик Rust. Future — это *стейт-машина*, которую рантайм опрашивает (`poll`) до готовности. Ключевое: **future ничего не делает, пока ты не сделаешь `.await` или не отдашь его рантайму** (`tokio::spawn`). Это отличается от JS-промисов, которые исполняются eager.
- **Синтаксис `async fn` и `.await`.** `async fn foo() -> T` — это `fn foo() -> impl Future<Output = T>`. `.await` приостанавливает выполнение, возвращая контроль рантайму, до готовности future.
- **Рантаймы.** В 95% случаев — [`tokio`](https://tokio.rs/) (де-факто стандарт; многопоточный планировщик, богатая экосистема). Альтернативы: [`async-std`](https://async.rs/) (API ближе к stdlib, развитие замедлилось), [`smol`](https://github.com/smol-rs/smol) (минималистичный). Не смешивай рантаймы в одном бинаре.
- **`tokio::main`:**
  ```rust
  #[tokio::main]
  async fn main() -> anyhow::Result<()> { ... }
  ```
  Под капотом — `Builder` для рантайма; для production-сервисов часто пишут конфигурацию рантайма вручную.
- **`tokio::spawn` vs `tokio::task::spawn_blocking`.** `spawn` — для async-задач (требует `Send + 'static` от future). `spawn_blocking` — переносит **синхронную** работу в отдельный thread pool, чтобы не заморозить async-планировщик. Любая блокирующая операция (`std::fs`, тяжёлый CPU-кранч, синхронный SDK) обязана идти через `spawn_blocking`.
- **Стримы (`futures::Stream`, `tokio_stream`)** — асинхронные итераторы. `while let Some(x) = stream.next().await { ... }`. Backpressure обрабатывается естественно через `.await`.
- **`tokio::select!`** — гонка нескольких future; первый завершившийся выигрывает, остальные **отменяются**. Это источник тонких багов «cancellation safety»: если внутри ветки была накопленная работа без `.await`-точек до commit'а — она теряется. Читай документацию `select!` целиком, не по диагонали.
- **`JoinSet`** (`tokio::task::JoinSet`) — динамический набор задач, можно `spawn` новые в процессе и `join_next().await` для забора результатов. Современный способ организовать «обработать N запросов параллельно с лимитом».
- **`Send + 'static` ловушки.** Future, передаваемая в `spawn`, должна быть `Send + 'static`. Если внутри держится `Rc` или non-`Send`-тип через `.await` — компилятор объяснит, но сообщение бывает длинное. Обычное лекарство: заменить `Rc` на `Arc`, либо вытащить non-`Send` за пределы `.await`.
- **Async-сеть и HTTP:** TCP/UDP через `tokio::net`; HTTP-клиент [`reqwest`](https://github.com/seanmonstar/reqwest); сервер — [`axum`](https://github.com/tokio-rs/axum) (на `tokio` + `tower`) или [`actix-web`](https://actix.rs/).
- **Каналы для async:** `tokio::sync::mpsc` / `oneshot` / `broadcast` / `watch` — каждая со своим назначением. `oneshot` — для «отправить один ответ»; `watch` — для «последнее значение видно всем».
- **Async-Mutex.** `tokio::sync::Mutex` нужен только если lock держится **через `.await`**. В остальных случаях — `std::sync::Mutex` или `parking_lot::Mutex` быстрее (нет async-overhead).
- **Логирование и диагностика:** [`tracing`](https://github.com/tokio-rs/tracing) + `tracing-subscriber` — стандарт для async; spans корректно прослеживаются через task'и. [`tokio-console`](https://github.com/tokio-rs/console) — интерактивный отладчик задач (вид процессов в `top`, но для tokio).

**Где изучать:**

- [The Rust Book, гл. 17 (Async и await — в новых редакциях)](https://doc.rust-lang.ru/book/) — вводное.
- [Asynchronous Programming in Rust](https://rust-lang.github.io/async-book/) — официальная книга по async.
- [Tokio Tutorial](https://tokio.rs/tokio/tutorial) — пошаговое создание Mini-Redis.
- Видео: [«The What and How of Futures and async/await in Rust» (Jon Gjengset)](https://www.youtube.com/watch?v=9_3krAQtD2k).
- [Alice Ryhl — «Actors with Tokio»](https://ryhl.io/blog/actors-with-tokio/) и «Shared mutable state in Rust» — короткие практические заметки от core-разработчика `tokio`.

**Зачем это нужно:** большинство production-Rust сервисов — async (web-бэкенды, прокси, сетевые тулзы, БД-клиенты). Поверхностного знания tokio мало: cancellation safety, `Send`-ловушки и `spawn_blocking` регулярно ломают код тех, кто ими не озаботился.

**Мини-проекты:** многопоточный TCP-эхо-сервер (`tokio::net::TcpListener`); HTTP-агрегатор, который параллельно опрашивает 10 API через `reqwest` + `JoinSet` и собирает ответы; mini-Redis (TCP + RESP-протокол + `HashMap` под `Mutex`).

---

## Этап 8. Макросы и метапрограммирование (2 недели)

**Что изучать:**

- **Декларативные макросы — `macro_rules!`.** Pattern-match по токенам, разворачиваются в код. Ты уже пользуешься: `println!`, `vec!`, `assert_eq!`, `format!` — это они.
  - Фрагменты: `expr`, `ident`, `ty`, `pat`, `block`, `stmt`, `path`, `tt` (token tree), `item`. Не путай — каждый позволяет ровно своё.
  - Повторители: `$( ... ),*` — ноль или больше через запятую; `$( ... )+` — один или больше.
  - **Гигиена.** `macro_rules!` гигиеничен по умолчанию: имена, объявленные внутри макроса, не пересекаются с внешними. Это не `#define` из C.
- **Процедурные макросы** — три вида:
  - **derive** (`#[derive(MyTrait)]`) — добавляет реализацию трейта;
  - **атрибутные** (`#[my_attribute] fn ...`) — преобразуют элемент целиком (используется в `#[tokio::main]`, `#[tracing::instrument]`);
  - **функциональные** (`my_macro!(...)`) — выглядят как обычные макросы, но работают с любыми токенами.
  Живут в **отдельном крейте** с `proc-macro = true` в `Cargo.toml`.
- **Тройка для проц-макросов:**
  - [`syn`](https://docs.rs/syn) — парсит входящие токены в AST Rust;
  - [`quote`](https://docs.rs/quote) — генерирует Rust по шаблону (`quote! { fn #name() {} }`);
  - [`proc-macro2`](https://docs.rs/proc-macro2) — стабильная обёртка над нестабильным `proc_macro` API; нужна, чтобы код проц-макроса можно было юнит-тестировать без `rustc`.
- **[`darling`](https://github.com/TedDriggs/darling)** — высокоуровневая обёртка над `syn` для парсинга атрибутов `#[my_macro(field = "value", flag)]`. Сильно сокращает derive-макрос.
- **[`trybuild`](https://github.com/dtolnay/trybuild)** — тестирование, что неправильное использование макроса даёт ожидаемое сообщение об ошибке. Стандарт для проверки UI proc-макросов.
- **`cargo expand`** (из `cargo install cargo-expand`) — посмотреть, во что разворачивается макрос. Незаменимо при отладке. Работает и для `macro_rules!`, и для `derive`.
- **Когда стоит писать макрос:** когда устранение бойлерплейта реально окупает усложнение чтения. Часто лучше **функция или дженерик**, а не макрос.

**Где изучать:**

- [The Rust Book, гл. 19 §6 (Макросы)](https://doc.rust-lang.ru/book/ch19-06-macros.html) — мягкое введение.
- [The Little Book of Rust Macros](https://veykril.github.io/tlborm/) — исчерпывающий справочник по `macro_rules!`, читать целиком.
- [`proc-macro-workshop` от David Tolnay](https://github.com/dtolnay/proc-macro-workshop) — упражнения, через которые проходят все, кто всерьёз пишет проц-макросы. Материал на 1–2 выходных.
- Документация [`syn`](https://docs.rs/syn) и [`quote`](https://docs.rs/quote) с примерами.

**Зачем это нужно:** даже если своих макросов писать не будешь, чтение чужих и понимание, что генерируется при `#[derive(Serialize, Deserialize)]` или `#[tokio::main]`, делает экосистему прозрачной. И сокращает «магия Rust» до «это вот такой код, я могу его прочитать через `cargo expand`».

**Мини-проект:** свой `derive(Builder)` — для `struct User { name: String, age: u32 }` сгенерировать `UserBuilder` с цепочечными сеттерами и методом `build() -> Result<User, _>`. Это упражнение #2 из `proc-macro-workshop`.

---

## Этап 9. Unsafe Rust и FFI (2 недели)

**Что изучать:**

- **Что реально открывает `unsafe`** (всего пять «суперсил»): разыменование сырых указателей; вызов `unsafe`-функций; доступ/изменение мутабельных `static`; реализация `unsafe`-трейтов; доступ к полям `union`. **Всё.** `unsafe` *не* отключает borrow checker и *не* делает Rust C-подобным — он просто говорит «здесь я беру на себя инварианты, которые компилятор не может проверить».
- **Контракт безопасности.** Каждая `unsafe fn` обязана иметь блок `/// # Safety` в doc-комментарии, перечисляющий инварианты вызывающего. Это **часть API**, а не пожелание.
- **Сырые указатели:** `*const T`, `*mut T`. Не имеют гарантий лайфтайма, могут быть null, могут указывать на освобождённую память. Создание сырого указателя — safe; разыменование — unsafe.
- **`MaybeUninit<T>`** — корректный способ работать с неинициализированной памятью. `mem::uninitialized()` deprecated и UB. Паттерн: `let mut x = MaybeUninit::<T>::uninit(); ptr::write(x.as_mut_ptr(), value); let x = unsafe { x.assume_init() };`.
- **`mem::transmute`** — почти всегда **не то**, что нужно. Крайний случай. Сначала смотри `as`, `From`/`Into`, `bytemuck`/`zerocopy`.
- **Strict Provenance API** (стабилизируется): `ptr.addr()`, `ptr::with_addr`, `expose_addr`. Аккуратная работа с указателями как с числами; устраняет класс UB при оптимизациях.
- **FFI с C:**
  - `extern "C" fn` для функций, видимых из C;
  - `#[repr(C)]` для структур, чья раскладка должна совпадать с C;
  - [`bindgen`](https://github.com/rust-lang/rust-bindgen) — генерирует Rust-биндинги по C-хедерам;
  - [`cbindgen`](https://github.com/mozilla/cbindgen) — наоборот, генерирует C-хедер по Rust-API;
  - [`cxx`](https://github.com/dtolnay/cxx) — безопасный мост Rust↔C++ с генерацией биндингов через декларацию типов.
- **Атрибуты layout:** `#[repr(C)]` (как в C), `#[repr(transparent)]` (как у внутреннего поля — для newtype-обёрток без overhead), `#[repr(packed)]` (без выравнивания — осторожно: ссылки на packed-поля недопустимы), `#[repr(u8)]` для enum-discriminant.
- **Panic через границу FFI — UB.** Оборачивай экспортируемые функции в `std::panic::catch_unwind`, превращая панику в код ошибки.
- **Miri** — интерпретатор Rust, который ловит UB в твоём `unsafe`-коде (use-after-free, неинициализированные чтения, выход за границу, нарушение алиасинга). Прогоняй критичные тесты через `cargo +nightly miri test`. Это самый дешёвый способ найти ошибку `unsafe`.
- **AddressSanitizer / ThreadSanitizer / MemorySanitizer / LeakSanitizer** через nightly: `RUSTFLAGS="-Z sanitizer=address" cargo +nightly test`. Дополняет Miri (ловит то, что Miri не моделирует, например, реальные heap corruption).

**Где изучать:**

- [The Rustonomicon](https://doc.rust-lang.org/nomicon/) — *главная* книга про unsafe. Читать после уверенного safe Rust. Особое внимание главам про aliasing, variance, drop check.
- [The Rust Reference: Unsafety](https://doc.rust-lang.org/reference/unsafety.html) — формальный справочник.
- [«Learn Rust the Dangerous Way»](https://cliffle.com/p/dangerust/) — пошаговый перенос C-программы в `unsafe` Rust, потом — итеративная замена на safe.
- [Документация `bindgen`](https://rust-lang.github.io/rust-bindgen/) и [`cxx`](https://cxx.rs/).
- [Miri README](https://github.com/rust-lang/miri).

**Зачем это нужно:** `unsafe` нужен на стыке с C-библиотеками, для нестандартных структур данных (свои аллокаторы, lock-free очереди), для перформанс-критичного кода (zero-copy парсеры, FFI без копирования). 99% Rust-кода — safe; `unsafe` — это инструмент, к которому подходят с осторожностью и доказательством корректности.

**Мини-проект:** написать **safe**-обёртку над небольшой C-библиотекой (например, [`libsodium`](https://libsodium.org/) или `zlib`) через `bindgen`, валидируя инварианты на границе и закрывая `unsafe` блоком `#![deny(unsafe_op_in_unsafe_fn)]`. Прогон тестов под Miri.

---

## Этап 10. Экосистема и реальные проекты

**Что изучать — по доменам:**

- **Веб-бэкенды.** [`axum`](https://github.com/tokio-rs/axum) — рекомендую как дефолт (`tokio` + `tower`, отличная типизация, активная разработка); [`actix-web`](https://actix.rs/) (зрелый, очень быстрый); [`rocket`](https://rocket.rs/) (приятный декларативный API); [`poem`](https://github.com/poem-web/poem). HTTP-крейты под капотом: [`hyper`](https://github.com/hyperium/hyper), [`tower`](https://github.com/tower-rs/tower) (composable middleware: rate-limiting, retry, timeout, load shedding), [`tower-http`](https://github.com/tower-rs/tower-http) (CORS, compression, tracing, auth).
- **Базы данных.** [`sqlx`](https://github.com/launchbadge/sqlx) — async, **проверка SQL на компиляции** (макрос `query!` ходит в живую БД во время сборки), современный дефолт; [`diesel`](https://diesel.rs/) (sync ORM с сильной типизацией и миграциями); [`sea-orm`](https://www.sea-ql.org/SeaORM/) (async ORM, ближе к ActiveRecord); [`redis`](https://github.com/redis-rs/redis-rs); [`mongodb`](https://github.com/mongodb/mongo-rust-driver). Миграции — `sqlx-cli`, `diesel migration`, [`refinery`](https://github.com/rust-db/refinery).
- **Сериализация.** [`serde`](https://serde.rs/) — *тот самый* фреймворк, де-факто стандарт. Поддерживает JSON ([`serde_json`](https://github.com/serde-rs/json)), YAML ([`serde_yaml`](https://github.com/dtolnay/serde-yaml)), TOML ([`toml`](https://github.com/toml-rs/toml)), MessagePack, [`bincode`](https://github.com/bincode-org/bincode), [`postcard`](https://github.com/jamesmunns/postcard) (для embedded), [`prost`](https://github.com/tokio-rs/prost) и [`protobuf`](https://github.com/stepancheg/rust-protobuf) (Protocol Buffers).
- **CLI.** [`clap`](https://github.com/clap-rs/clap) (парсинг аргументов, derive-API, генерация completions для bash/zsh/fish), [`structopt`](https://github.com/TeXitoi/structopt) (исторический предок `clap` derive — теперь не нужен, всё в `clap v4`), [`dialoguer`](https://github.com/console-rs/dialoguer) (интерактивные промпты), [`indicatif`](https://github.com/console-rs/indicatif) (прогресс-бары), [`console`](https://github.com/console-rs/console) (цвета, стили в терминале).
- **Логирование и трассировка.** [`tracing`](https://github.com/tokio-rs/tracing) + `tracing-subscriber` — современный дефолт (структурированные логи, spans, OpenTelemetry-интеграция). Старый [`log`](https://github.com/rust-lang/log) ещё жив и хорош для библиотек (нейтральный фасад). Не путай: `tracing` для приложений, `log` для библиотек, которые не хотят форсить выбор системы логирования.
- **Конфигурация:** [`config`](https://github.com/mehcode/config-rs) (слои: файл + env + override), [`figment`](https://github.com/SergioBenitez/Figment) (от автора Rocket, гибче). Загрузка `.env` — [`dotenvy`](https://github.com/allan2/dotenvy) (форк `dotenv`, который не поддерживается).
- **HTTP-клиент.** [`reqwest`](https://github.com/seanmonstar/reqwest) — дефолт (async, JSON, мультипарт, прокси). Альтернативы: [`ureq`](https://github.com/algesten/ureq) (sync, минимум зависимостей), [`hyper`](https://github.com/hyperium/hyper) (низкий уровень, под нестандартные задачи).
- **WebSockets:** [`tokio-tungstenite`](https://github.com/snapview/tokio-tungstenite), сервер на `axum` через `axum::extract::WebSocketUpgrade`.
- **gRPC:** [`tonic`](https://github.com/hyperium/tonic) — async-gRPC на `tokio` + `prost`.
- **CLI-утилиты для разработчиков:** `cargo-edit`, `cargo-watch`, `cargo-expand`, `cargo-outdated`, `cargo-audit` (CVE), `cargo-deny` (банлисты лицензий и крейтов), `cargo-nextest` (быстрый тест-раннер, в 2–3 раза быстрее `cargo test` на больших проектах), `cargo-llvm-cov` (покрытие кода).
- **Дата/время:** [`chrono`](https://github.com/chronotope/chrono) (классика, много API), [`time`](https://github.com/time-rs/time) (современнее, с `no_std`), [`jiff`](https://github.com/BurntSushi/jiff) (новый, с правильной работой с таймзонами от автора `ripgrep`).
- **Регулярки и парсинг:** [`regex`](https://github.com/rust-lang/regex), [`nom`](https://github.com/rust-bakery/nom) (комбинаторный парсер), [`winnow`](https://github.com/winnow-rs/winnow) (наследник `nom` v8 с переписанным API), [`pest`](https://github.com/pest-parser/pest) (PEG, грамматика отдельным файлом).
- **Шаблонизаторы:** [`askama`](https://github.com/djc/askama) (compile-time, типобезопасен), [`tera`](https://github.com/Keats/tera) (runtime, как Jinja2), [`maud`](https://github.com/lambda-fairy/maud) (HTML как макрос Rust).

**Где изучать:**

- [Awesome Rust](https://github.com/rust-unofficial/awesome-rust) — каталог крейтов и проектов.
- [lib.rs](https://lib.rs/) — альтернатива crates.io с более удобной навигацией и категоризацией.
- [«Zero To Production In Rust» (Luca Palmieri)](https://www.zero2prod.com/) — *лучшая* книга по практическому веб-бэкенду на Rust (`actix-web` + `sqlx` + `tracing` + Docker). Если делаешь web — must-read.
- [«Rust for Rustaceans» (Jon Gjengset)](https://nostarch.com/rust-rustaceans) — для перехода с уверенного среднего на продвинутый.
- Официальная документация каждого крейта на [docs.rs](https://docs.rs/).

**Зачем это нужно:** одно дело — знать язык, другое — собрать рабочий сервис. Этот этап превращает «я могу написать функцию» в «я могу выкатить REST-API в Docker с миграциями БД, логами в JSON и метриками в Prometheus».

**Мини-проект:** REST-API на `axum` + `sqlx` + Postgres с миграциями, аутентификацией (JWT через `jsonwebtoken`), `tracing` для логов, `tower-http` для CORS/compression, тестами в `tests/` и Dockerfile на multi-stage. **Это** — шипуемый проект уровня джуна-мидла.

---

## Этап 11. Уровень профи (4–8 недель и далее)

**Что изучать:**

- **Профилирование на уровне CPU и памяти.** Снятие flamegraph'ов, поиск горячих точек, анализ кеш-промахов и аллокаций. Инструменты: `perf` + [cargo-flamegraph](https://github.com/flamegraph-rs/flamegraph) (Linux), [Instruments](https://developer.apple.com/xcode/) (macOS), [`samply`](https://github.com/mstange/samply) (кросс-платформенно), [`heaptrack`](https://github.com/KDE/heaptrack), [`dhat`](https://docs.rs/dhat/).
- **Микрооптимизации и low-level perf.** SIMD через `std::simd` (nightly) или `std::arch` (stable), хинты `#[inline]` / `#[cold]`, struct-of-arrays vs array-of-structs, cache-friendly layout, выравнивание (`#[repr(C, align(64))]`).
- **Fuzz-тестирование и property-based.** [`cargo-fuzz`](https://github.com/rust-fuzz/cargo-fuzz) (libFuzzer), [`afl.rs`](https://github.com/rust-fuzz/afl.rs); property-based — [`proptest`](https://github.com/proptest-rs/proptest), [`quickcheck`](https://github.com/BurntSushi/quickcheck), снапшот-тесты — [`insta`](https://github.com/mitsuhiko/insta).
- **Чтение исходников зрелых крейтов** по нарастающей сложности: `itertools` → `serde` → `tokio` → `hyper` → `rustc`. Это самый быстрый путь от среднего уровня к продвинутому.
- **Open-source-вклад.** Бери `E-easy` / `good-first-issue` в любом популярном крейте. Ревью от мейнтейнеров — лучший ускоритель роста.
- **Публикация своего крейта на [crates.io](https://crates.io):** хорошие доки, CI на GitHub Actions, [семвер](https://semver.org/), [`cargo-semver-checks`](https://github.com/obi1kenobi/cargo-semver-checks) перед каждым релизом.
- **Nightly-фичи и RFC-процесс.** Читай [Inside Rust Blog](https://blog.rust-lang.org/inside-rust/), [`rust-lang/rfcs`](https://github.com/rust-lang/rfcs), подпишись на [This Week in Rust](https://this-week-in-rust.org/).

**Где изучать:**

- The Rust Performance Book — <https://nnethercote.github.io/perf-book/>
- «Writing High-Performance Rust» (видеосерия Logan Smith и [fasterthanli.me](https://fasterthanli.me/)).
- Зеркала лекций по `rustc` от разработчиков компилятора на YouTube.
- Канал [@rust_code](https://t.me/+HelhY88sArowYjgy) — разборы кода и идиом, по которым удобно сверяться.

**Зачем это нужно:** на этом уровне ты перестаёшь *учить Rust* и начинаешь *думать на Rust*. Лучшие инженеры публикуют библиотеки, которыми пользуются другие, шлют фиксы апстрим и понимают компромиссы дизайнеров языка.

**Мини-проект:**

- Возьми любой свой крейт уровня этапа 10, прогоняй его через `cargo-fuzz`, `proptest`, `criterion` — найди и почини хотя бы одну реальную проблему.
- Сделай PR в популярный крейт (например, `tokio`, `serde`, `clap`, `reqwest`) — пусть даже небольшой фикс доки или теста.

---

# 🚀 Расширение roadmap: продвинутые темы

> Этапы ниже не обязательно идти строго по порядку — выбирай ту ветку, которая ближе к твоей предметной области (web, embedded, ML, gamedev, distributed systems). Но все они опираются на уверенное прохождение этапов 1–11.

## Этап 12. Продвинутая типовая система

**Что изучать:**

- **HRTB (Higher-Ranked Trait Bounds, `for<'a>`)** и **GAT (Generic Associated Types)** — позволяют выражать «лайфтайм-семейства» и абстракции, которые на обычных дженериках просто не записываются (например, лениво возвращающиеся итераторы со ссылками на собственные данные).
- **Associated types vs дженерики.** Когда выбирать `type Item;` (одна реализация на тип), а когда `<T>` (много реализаций для одного типа). Рассмотри это на примерах `Iterator` и `Add<T>`.
- **`PhantomData`, zero-sized types, type-level состояния.** Кодируй состояния протокола в типах (`Builder<Unbuilt>` → `Builder<Built>`), чтобы неправильное использование становилось ошибкой компиляции. Это паттерн **typestate**.
- **Sealed traits** — закрытые расширяемые API: только модуль-владелец может реализовать трейт у новых типов, потребители — нет.
- **Newtype-паттерн** (`struct UserId(u64);`) — разные типы для разных смыслов без рантайм-стоимости. Защищает от смешивания `UserId` и `OrderId`.
- **Variance:** covariance, contravariance, invariance. Почему `&mut T` инвариантен, а `&T` ковариантен.
- **Auto-traits, negative impls (`!Send`, `!Sync`), `?Sized`, DST** (dynamically sized types).

**Где изучать:**

- The Rustonomicon — <https://doc.rust-lang.org/nomicon/> (главы про variance и DST).
- The Rust Reference — <https://doc.rust-lang.org/reference/> (раздел про trait bounds).
- Серия [«Pretty State Machine Patterns in Rust»](https://hoverbear.org/blog/rust-state-machine-pattern/) — про typestate.
- Jon Gjengset, плейлист «Crust of Rust» на YouTube — лучший разбор GAT, HRTB и subtyping на практике.

**Зачем это нужно:** даёт инструменты для «компилятор-как-теорема» подхода: API становятся такими, что неправильное использование в принципе невозможно. Это отличает middle от senior'а в Rust.

**Мини-проект:**

- Спроектируй type-state API для конечного автомата (HTTP-парсер, протокол хендшейка) — пусть некорректные переходы становятся ошибкой компиляции.
- Реализуй sealed trait для расширяемого API.

## Этап 13. Глубокая работа с асинхронностью

**Что изучать:**

- **Внутренности `Future`:** `Poll::Ready` / `Poll::Pending`, `Pin`, `Unpin`, self-referential типы и почему они вообще нужны.
- **Устройство async-рантайма:** reactor (epoll/kqueue/io_uring), executor, waker, task scheduling. Прочитай минимальный executor (например, [`futures::executor`](https://docs.rs/futures/latest/futures/executor/index.html)) от и до.
- **Cooperative scheduling** и проблема блокирующих вызовов в async. Когда нужен `tokio::task::spawn_blocking`, `LocalSet`, `JoinSet`.
- **Cancellation safety** и почему `tokio::select!` опасен, если не понимать его. Что значит «фьючи могут быть брошены в любой await-точке».
- **Структурированная конкурентность.** [`tokio-util`](https://github.com/tokio-rs/tokio), [`async-scoped`](https://github.com/rmanoka/async-scoped), `JoinSet`. Идея: ни одна задача не переживает свой scope.
- **Backpressure в стримах** (`futures::Stream`, `tokio_stream`). Как не перегрузить медленного потребителя.
- **`async fn` в трейтах** (стабильны с 1.75) и `#[async_trait]` — когда нужен какой.
- **Отладка async:** [`tokio-console`](https://github.com/tokio-rs/console), `tracing` spans, `tokio::time::pause()` для детерминированных тестов.

**Где изучать:**

- «Asynchronous Programming in Rust» — <https://rust-lang.github.io/async-book/>
- Tokio Tutorial — <https://tokio.rs/tokio/tutorial>
- Серия Alice Ryhl «Actors with Tokio» и «Shared mutable state in Rust».
- [Without.Boats](https://without.boats/) — блог одного из дизайнеров async/await.

**Зачем это нужно:** большинство production-Rust проектов — async (web, сеть, БД). Поверхностного знания tokio мало: cancellation safety и Pin регулярно ломают код тех, кто их не понял.

**Мини-проект:**

- Напиши свой простейший async-executor на базе `futures::task::Waker` — поймёшь, как всё устроено внутри.
- Реализуй mini-Redis (TCP-сервер с pub/sub) на чистом `tokio`, добавь `tracing` и `tokio-console`.

## Этап 14. Производительность и низкий уровень

**Что изучать:**

- **Cache-friendly структуры, false sharing, padding.** Почему `Vec<Struct>` часто медленнее, чем struct-of-arrays.
- **SoA vs AoS** (struct of arrays) — выбор раскладки, который часто доминирует в перформансе горячих циклов.
- **Branch prediction**, хинты `std::hint::likely` / `unlikely`, `black_box` для бенчмарков.
- **SIMD:** `std::simd` (nightly, переносимый), `std::arch` (stable, платформенные интринсики), крейт [`wide`](https://github.com/Lokathor/wide) и [`pulp`](https://github.com/sarah-quinones/pulp).
- **Allocator API**, кастомные аллокаторы: [`bumpalo`](https://github.com/fitzgen/bumpalo), [`mimalloc`](https://github.com/microsoft/mimalloc), [`jemallocator`](https://github.com/tikv/jemallocator).
- **Arena-аллокация:** [`typed-arena`](https://github.com/SimonSapin/rust-typed-arena), [`bumpalo`](https://github.com/fitzgen/bumpalo) — критично для парсеров и компиляторов.
- **Zero-copy парсинг:** [`nom`](https://github.com/rust-bakery/nom), [`winnow`](https://github.com/winnow-rs/winnow), [`zerocopy`](https://github.com/google/zerocopy), [`rkyv`](https://github.com/rkyv/rkyv).
- **Профилирование и анализ:** `perf`, `cargo-flamegraph`, `samply`, [`hyperfine`](https://github.com/sharkdp/hyperfine), [`cargo-bloat`](https://github.com/RazrFalcon/cargo-bloat), [`cargo-show-asm`](https://github.com/pacak/cargo-show-asm).
- **Benchmark-driven development** через [`criterion`](https://github.com/bheisler/criterion.rs) и [`iai`](https://github.com/bheisler/iai).

**Где изучать:**

- The Rust Performance Book — <https://nnethercote.github.io/perf-book/>.
- «Algorithmica» (Sergey Slotin) — <https://en.algorithmica.org/hpc/> — общие принципы HPC, применимы к Rust.
- Видео Logan Smith и Amos (fasterthanli.me) на YouTube.

**Зачем это нужно:** Rust выбирают там, где важна производительность (БД, бэкенды, парсеры, игровые движки, embedded). Без этого этапа Rust используется как «более безопасный Go», а не как замена C++.

**Мини-проект:**

- Возьми один из своих крейтов, замерь baseline через `criterion`, попробуй ускорить в 2–10× через SIMD, arena-аллокацию или zero-copy парсинг.
- Реализуй простой ray-tracer или JSON-парсер и оптимизируй до уровня serde_json/sonic-rs.

## Этап 15. Безопасность и корректность

**Что изучать:**

- **Формальная верификация:** [Kani](https://github.com/model-checking/kani) (model checker от AWS), [Prusti](https://github.com/viperproject/prusti-dev) (на основе Viper), [Creusot](https://github.com/creusot-rs/creusot).
- **Model checking конкурентности:** [`loom`](https://github.com/tokio-rs/loom) — перебор всех возможных переплетений потоков; [`shuttle`](https://github.com/awslabs/shuttle) — randomized concurrency testing.
- **Sanitizers через nightly:** `-Z sanitizer=address|thread|memory|leak`. Помогают найти UB в `unsafe` коде.
- **Mutation testing:** [`cargo-mutants`](https://github.com/sourcefrog/cargo-mutants) — оценивает, насколько твои тесты реально проверяют логику.
- **Аудит зависимостей:** [`cargo-audit`](https://github.com/rustsec/rustsec) (CVE), [`cargo-deny`](https://github.com/EmbarkStudios/cargo-deny) (лицензии + банлисты), [`cargo-vet`](https://github.com/mozilla/cargo-vet), [`cargo-geiger`](https://github.com/geiger-rs/cargo-geiger) (поиск `unsafe`).
- **Supply-chain security:** reproducible builds, SBOM, проверки на typo-squatting.
- **Безопасный FFI:** ABI-стабильность, `#[repr(C)]`, корректное распространение panic'ов через границу языка, `catch_unwind`.

**Где изучать:**

- The Rustonomicon — <https://doc.rust-lang.org/nomicon/> (про UB и sound `unsafe`).
- RustSec Advisory DB — <https://rustsec.org/>.
- Туториалы Kani и Loom в их репозиториях.
- [«Learn Rust the Dangerous Way»](https://cliffle.com/p/dangerust/) — пошаговое освоение `unsafe`.

**Зачем это нужно:** в инфраструктурных и крипто-крейтах ошибки означают потерю денег и данных. Эти инструменты — стандарт в крупных Rust-кодовых базах (AWS, Google, Mozilla, Cloudflare).

**Мини-проект:**

- Возьми любой свой `unsafe`-блок и докажи его корректность через Kani или хотя бы прогон под Miri и AddressSanitizer.
- Настрой CI с `cargo-audit`, `cargo-deny`, `cargo-mutants` для одного из своих крейтов.

## Этап 16. Архитектура и паттерны проектирования

**Что изучать:**

- **Идиоматичный Rust:** builder, typestate, RAII-guards (`MutexGuard`, `Drop`), newtype.
- **Hexagonal / Clean architecture без DI-фреймворков.** В Rust DI делается через дженерики и трейты, а не через рантайм-контейнеры. Изучи, как структурировать большой сервис без Spring-подобных фреймворков.
- **Error handling: библиотека vs приложение.** В библиотеках — `thiserror` + конкретные enum-ошибки; в приложениях — `anyhow` / `eyre` / `color-eyre` + контекст. Эти две стратегии нельзя смешивать.
- **Антипаттерны:**
  - борьба с borrow checker через `.clone()` везде («clone-driven development»);
  - избыточное `Arc<Mutex<_>>` там, где хватило бы канала или `&mut`;
  - `unwrap()` в production-коде;
  - god-структуры с десятками полей и lifetimes.
- **Композиция вместо наследования.** Enum-полиморфизм vs trait objects (`dyn Trait`) — когда что выбирать.
- **Workspace-структура:** разделение `-core`, `-cli`, `-server`, `-macros` крейтов; чёткие границы абстракций.

**Где изучать:**

- [Rust Design Patterns](https://rust-unofficial.github.io/patterns/) — каноническое собрание идиом.
- [«Rust API Guidelines»](https://rust-lang.github.io/api-guidelines/) — как делать публичные API.
- [«Effective Rust»](https://www.lurklurk.org/effective-rust/) Дэвида Дрисдейла — must-read после первого реального проекта.
- Доклады с RustConf и EuroRust на YouTube.

**Зачем это нужно:** код, который компилируется и проходит тесты, ещё не идиоматичен. Хорошая архитектура делает Rust-проект приятным в поддержке через год. Без этого ты пишешь «Rust как Go» или «Rust как C++» и теряешь половину преимуществ.

**Мини-проект:**

- Перепиши один из своих крейтов в hexagonal-стиле: ядро без I/O + адаптеры (БД, HTTP, CLI) как отдельные модули.
- Сделай аудит чужого open-source-крейта с точки зрения API guidelines.

## Этап 17. Системное программирование и ОС

**Что изучать:**

- **Прямая работа с syscalls:** [`mio`](https://github.com/tokio-rs/mio), `epoll` / `kqueue` / `io_uring` напрямую через крейты [`io-uring`](https://github.com/tokio-rs/io-uring), [`rio`](https://github.com/spacejam/rio).
- **Async-рантаймы для I/O:** [`tokio-uring`](https://github.com/tokio-rs/tokio-uring), [`glommio`](https://github.com/DataDog/glommio) (thread-per-core, полезен для БД и сетевых сервисов с предсказуемой латентностью), [`monoio`](https://github.com/bytedance/monoio).
- **Shared memory, `mmap`:** [`memmap2`](https://github.com/RazrFalcon/memmap2-rs) для memory-mapped файлов; работа с большими данными без копирования.
- **Сигналы Unix:** [`signal-hook`](https://github.com/vorner/signal-hook), `nix` крейт для POSIX API; процессы, pipes, fork/exec.
- **Свой аллокатор и свой `Future`-executor с нуля.** Лучший способ понять, как работает Rust-рантайм.
- **Kernel-разработка:** Rust в Linux kernel ([Rust for Linux](https://rust-for-linux.com/)), [Redox OS](https://www.redox-os.org/), [Theseus](https://www.theseus-os.com/).
- **Bare-metal:** `#![no_std]`, `core` vs `alloc` vs `std`, panic-handler, custom target. Bootloader и минимальная ОС — Philipp Oppermann «[Writing an OS in Rust](https://os.phil-opp.com/)».

**Где изучать:**

- «The Linux Programming Interface» (Michael Kerrisk) — фундамент по UNIX API, переносится на Rust один в один.
- «Writing an OS in Rust» — <https://os.phil-opp.com/> (одна из лучших обучающих серий вообще).
- Документация Rust for Linux — <https://rust-for-linux.com/>.

**Зачем это нужно:** Rust изначально создавался для системного программирования. Эта ветка ведёт к работе над ядрами, гипервизорами, БД, рантаймами и низкоуровневой инфраструктурой — нишам, где Rust сейчас вытесняет C/C++.

**Мини-проект:**

- Напиши свой простейший event loop поверх `io_uring` или `epoll`.
- Сделай минимальный `#![no_std]` бинарь под QEMU, который выводит «hello» в VGA-буфер.

## Этап 18. Распределённые системы и сеть

**Что изучать:**

- **Реализация сетевых протоколов с нуля:** Redis-протокол (RESP), HTTP/1.1 — отличные учебные задачи.
- **Алгоритмы консенсуса:** Raft через [`openraft`](https://github.com/datafuselabs/openraft), gossip-протоколы (например, SWIM).
- **QUIC и HTTP/3:** [`quinn`](https://github.com/quinn-rs/quinn); HTTP/2 streaming, HPACK.
- **gRPC:** streaming, interceptors, middleware в [`tonic`](https://github.com/hyperium/tonic).
- **Observability в распределёнке:** OpenTelemetry, [`tracing-opentelemetry`](https://github.com/tokio-rs/tracing-opentelemetry), метрики через [`metrics`](https://github.com/metrics-rs/metrics) или [`prometheus`](https://github.com/tikv/rust-prometheus).
- **Надёжность:** идемпотентность, retry с jitter, circuit breaker, rate limiting через [`tower`](https://github.com/tower-rs/tower) и [`tower-http`](https://github.com/tower-rs/tower-http).
- **Архитектурные паттерны:** event sourcing и CQRS, saga, outbox pattern.

**Где изучать:**

- «Designing Data-Intensive Applications» (Martin Kleppmann) — теория, применимая к любому языку.
- Официальный Tokio Tutorial раздел про Mini-Redis.
- Доклады с RustConf про `tower` и `tonic`.
- Реальные кодовые базы: `tikv`, `materialize`, `databend`, `risingwave` — это лучшие учебники по распределённому Rust.

**Зачем это нужно:** Rust популярен в инфраструктурных стартапах (TiKV, Materialize, Databend, Linkerd, Vector, Tremor). Это самая «денежная» ниша Rust в индустрии.

**Мини-проект:**

- Реализуй mini-Redis с поддержкой репликации (одна нода-лидер, несколько read-replica).
- Подними gRPC-сервис на `tonic` с трассировкой через `tracing` + Jaeger/Tempo.

## Этап 19. Базы данных и хранилища

**Что изучать:**

- **Глубже `sqlx`:** compile-time проверка запросов через `query!`/`query_as!`, миграции через `sqlx-cli` или [`refinery`](https://github.com/rust-db/refinery).
- **ORM vs query-builder:** [`diesel`](https://diesel.rs/) (классический type-safe ORM), [`sea-orm`](https://www.sea-ql.org/SeaORM/), [`sea-query`](https://github.com/SeaQL/sea-query) (query builder без ORM-магии).
- **Connection pooling:** [`deadpool`](https://github.com/bikeshedder/deadpool), [`bb8`](https://github.com/djc/bb8), встроенный пул в `sqlx`.
- **Embedded базы данных:** [`sled`](https://github.com/spacejam/sled) (LSM), [`redb`](https://github.com/cberner/redb) (B-Tree, ACID), [`rocksdb`](https://github.com/rust-rocksdb/rust-rocksdb), [`fjall`](https://github.com/fjall-rs/fjall).
- **Свой storage engine:** B-Tree, LSM, WAL, MVCC. Полезный учебный путь — реализовать упрощённый Bitcask или LSM-engine.
- **Векторные БД и поиск:** [`qdrant`](https://github.com/qdrant/qdrant) (vector DB, целиком на Rust), [`tantivy`](https://github.com/quickwit-oss/tantivy) (полнотекстовый поиск, аналог Lucene).

**Где изучать:**

- Курс CMU 15-445 «Database Systems» — <https://15445.courses.cs.cmu.edu/> (необязательно делать на C++, можно на Rust).
- «Database Internals» (Alex Petrov).
- Доки и блог [Materialize](https://materialize.com/) и [TiKV](https://tikv.org/).
- Туториал «Build your own Redis» / «Build your own SQLite» на сайте codecrafters.io.

**Зачем это нужно:** Rust очень силён в storage-движках и БД (RocksDB-клиенты, TiKV, Materialize, Databend, Quickwit, Qdrant). Понимание устройства БД — топовый навык для любого backend-инженера.

**Мини-проект:**

- Напиши свой key-value движок с WAL, простейшим LSM или B-Tree, поддержкой транзакций.
- Сделай мини-аналог Elasticsearch на `tantivy` с REST-API на `axum`.

## Этап 20. WebAssembly углублённо

**Что изучать:**

- **Цели компиляции:** `wasm32-unknown-unknown` (браузер/embedder без stdlib), `wasm32-wasi` / `wasm32-wasip2` (с системным API).
- **Component Model и WIT-интерфейсы** — будущее композируемых WASM-модулей.
- **WASM embedder'ы:** [`wasmtime`](https://github.com/bytecodealliance/wasmtime), [`wasmer`](https://github.com/wasmerio/wasmer) — как встраивать WASM-движок в свой Rust-сервис.
- **Plugin-системы на WASM:** [`extism`](https://extism.org/) — упрощённый фреймворк для расширений.
- **Frontend на Rust:** сравнение [`leptos`](https://leptos.dev/), [`dioxus`](https://dioxuslabs.com/), [`yew`](https://yew.rs/), [`sycamore`](https://sycamore-rs.netlify.app/) — signals vs vdom.
- **SSR, hydration, isomorphic functions** — Leptos и Dioxus поддерживают это нативно.
- **Edge-runtime:** Cloudflare Workers ([`worker`](https://github.com/cloudflare/workers-rs) крейт), Fastly Compute, Vercel Edge.

**Где изучать:**

- «The `wasm-bindgen` Guide» — <https://rustwasm.github.io/wasm-bindgen/>.
- «Rust and WebAssembly» — <https://rustwasm.github.io/docs/book/>.
- Документация Leptos и Dioxus.
- Bytecode Alliance — <https://bytecodealliance.org/> (WASI и Component Model).

**Зачем это нужно:** WASM — главная тема последних нескольких лет в Rust-экосистеме. Edge-вычисления, плагины, фронтенд, sandboxing — везде Rust + WASM становится дефолтным выбором.

**Мини-проект:**

- Сделай SPA на Leptos / Dioxus с SSR + hydration; задеплой на Cloudflare Workers или Fly.io.
- Встрой WASM-плагины в свой Rust-сервис через `wasmtime` (например, пользовательские фильтры/трансформации).

## Этап 21. Embedded и реальное время

**Что изучать:**

- **Hardware Abstraction Layer:** [`embedded-hal`](https://github.com/rust-embedded/embedded-hal) 1.0 как общий интерфейс; HAL-крейты для конкретных МК ([`stm32-hal`](https://github.com/David-OConnor/stm32-hal), [`esp-hal`](https://github.com/esp-rs/esp-hal), [`rp2040-hal`](https://github.com/rp-rs/rp-hal), [`nrf-hal`](https://github.com/nrf-rs/nrf-hal)).
- **RTOS и async-фреймворки на embedded:** [Embassy](https://embassy.dev/) (async-first, очень удобный) — де-факто стандарт для нового кода; [RTIC](https://rtic.rs/) (priority-based, без аллокатора).
- **DMA, прерывания, критические секции** через крейт [`critical-section`](https://github.com/rust-embedded/critical-section).
- **Эффективное логирование на МК:** [`defmt`](https://github.com/knurling-rs/defmt) — компактный бинарный формат логов с decoding на хосте.
- **Отладка и прошивка:** [`probe-rs`](https://probe.rs/) — единый инструмент для GDB/RTT/flashing/SWO.
- **Безопасность памяти на `no_std`:** [`heapless`](https://github.com/rust-embedded/heapless) (`Vec`, `String`, очереди фиксированного размера), статическое выделение, отсутствие аллокатора.

**Где изучать:**

- The Embedded Rust Book — <https://docs.rust-embedded.org/book/>.
- Discovery Book (на STM32F3 Discovery) — <https://docs.rust-embedded.org/discovery/>.
- Документация Embassy — <https://embassy.dev/book/>.
- Канал [Knurling-rs](https://knurling.ferrous-systems.com/) с туториалами по `defmt`/`probe-rs`.

**Зачем это нужно:** Rust в embedded — одна из самых быстрорастущих ниш. Производители (Espressif, Nordic, ST) официально поддерживают Rust. Async на МК через Embassy реально удобен и отличается от классического C-подхода.

**Мини-проект:**

- Возьми ESP32 / RP2040 / STM32, напиши на Embassy прошивку, которая по Wi-Fi/BLE отдаёт данные с датчика.
- Реализуй простой PID-регулятор и управление шаговым мотором / сервоприводом через PWM + DMA.

## Этап 22. Геймдев и графика

**Что изучать:**

- **ECS-движок [Bevy](https://bevyengine.org/):** компоненты, системы, ресурсы, schedules, plugins. Идиоматический ECS на Rust.
- **Шейдеры и графика:** WGSL, [`wgpu`](https://github.com/gfx-rs/wgpu) напрямую (cross-API: Vulkan/Metal/DX12/WebGPU), [`naga`](https://github.com/gfx-rs/naga) для трансляции шейдеров.
- **Физика:** [`rapier`](https://github.com/dimforge/rapier) (2D/3D физика), [`avian`](https://github.com/Jondolf/avian) (нативная для Bevy).
- **Сетевой код для игр:** [`lightyear`](https://github.com/cBournhonesque/lightyear), [`bevy_replicon`](https://github.com/projectharmonia/bevy_replicon), client-side prediction, server reconciliation.
- **Альтернативные движки:** [`fyrox`](https://github.com/FyroxEngine/Fyrox) (бывший rg3d, классический сцен-граф), [`macroquad`](https://github.com/not-fl3/macroquad) (быстрый старт для 2D), [`ggez`](https://github.com/ggez/ggez).
- **Низкоуровневая графика:** [`vulkano`](https://github.com/vulkano-rs/vulkano), [`ash`](https://github.com/ash-rs/ash) (raw Vulkan bindings).

**Где изучать:**

- The Bevy Book — <https://bevyengine.org/learn/book/>.
- The wgpu tutorial — <https://sotrh.github.io/learn-wgpu/>.
- «Learn OpenGL» (Joey de Vries) — переносится на wgpu один в один.
- YouTube-канал Logic Projects, разборы Bevy от Chris Biscardi.

**Зачем это нужно:** геймдев на Rust ещё не доминирует индустрию, но Bevy быстро становится зрелым. Это интересная ветка для тех, кто любит игры и графику, и при этом отличный стресс-тест продвинутой типовой системы и перформанса.

**Мини-проект:**

- Сделай 2D-платформер или top-down-шутер на Bevy с физикой через `avian`.
- Реализуй простой ray-tracer на `wgpu` compute-шейдерах.

## Этап 23. ML, AI и научные вычисления

**Что изучать:**

- **Tensor-фреймворки:** [`candle`](https://github.com/huggingface/candle) (HuggingFace, минималистичный, для inference), [`burn`](https://github.com/tracel-ai/burn) (гибкий, разные бэкенды: CPU / wgpu / CUDA / Metal).
- **Биндинги к C++ ML-стекам:** [`tch-rs`](https://github.com/LaurentMazare/tch-rs) — биндинги к LibTorch; [`ort`](https://github.com/pykeio/ort) — ONNX Runtime для inference.
- **Линейная алгебра и научка:** [`nalgebra`](https://nalgebra.org/) (геометрия, графика), [`ndarray`](https://github.com/rust-ndarray/ndarray) (numpy-подобный), [`faer`](https://github.com/sarah-quinones/faer-rs) (быстрый dense linalg).
- **Распараллеливание:** [`rayon`](https://github.com/rayon-rs/rayon) (data parallelism), GPU через [`cust`](https://github.com/Rust-GPU/Rust-CUDA) (CUDA bindings) и [`cubecl`](https://github.com/tracel-ai/cubecl) (универсальный GPU compute).
- **LLM и inference:** [`mistral.rs`](https://github.com/EricLBuehler/mistral.rs), [`llama.cpp`-биндинги](https://github.com/utilityai/llama-cpp-rs), запуск open-weight моделей на Rust-стеке.
- **Эмбеддинги и vector search:** связка `candle` / `ort` + `qdrant` / `tantivy`.

**Где изучать:**

- Документация `candle` и `burn` (с примерами).
- Курс fast.ai (Python), но реализуй упражнения на Rust + `burn`.
- «Deep Learning» (Goodfellow et al.) — теоретический фундамент.
- Блог HuggingFace о `candle`.

**Зачем это нужно:** Rust используется как infra-слой для ML-сервисов: токенизация, inference, vector search, серверы для моделей. Скорость и предсказуемая память делают его идеальным для production-ML, где Python-стек слишком тяжёл.

**Мини-проект:**

- Сделай локальный LLM-чат с inference на `candle` или `mistral.rs` и REST-API на `axum`.
- Реализуй semantic search: эмбеддинги через `candle` + индекс через `qdrant` или `tantivy`.

## Этап 24. Инструментарий, DevEx и продакшн

**Что изучать:**

- **Workspaces, feature flags, conditional compilation.** Как разбивать большой проект на крейты, не плодя дубликаты, и контролировать набор фич.
- **Build scripts (`build.rs`).** Кодогенерация, биндинги через [`bindgen`](https://github.com/rust-lang/rust-bindgen) / [`cxx`](https://github.com/dtolnay/cxx), линковка с C/C++.
- **Полезные cargo-расширения:** [`cargo-make`](https://github.com/sagiegurari/cargo-make), [`cargo-nextest`](https://github.com/nextest-rs/nextest) (быстрый параллельный тест-раннер), [`cargo-watch`](https://github.com/watchexec/cargo-watch), [`cargo-expand`](https://github.com/dtolnay/cargo-expand) (раскрытие макросов), [`cargo-udeps`](https://github.com/est31/cargo-udeps), [`cargo-machete`](https://github.com/bnjbvr/cargo-machete) (поиск неиспользуемых зависимостей), [`cargo-outdated`](https://github.com/kbknapp/cargo-outdated).
- **Кросс-компиляция:** [`cross`](https://github.com/cross-rs/cross), [`cargo-zigbuild`](https://github.com/rust-cross/cargo-zigbuild) (через `zig` как линкер — отличный UX).
- **Минимизация бинарника:** LTO (`lto = "fat"`), `panic = "abort"`, `strip = true`, `opt-level = "z"`, `codegen-units = 1`.
- **Контейнеризация:** distroless и scratch-образы, multi-stage builds, статически линкованные бинари (`x86_64-unknown-linux-musl`).
- **CI/CD:** GitHub Actions с [`Swatinem/rust-cache`](https://github.com/Swatinem/rust-cache), matrix-сборки, артефакты, релизы.
- **Релизы:** [`cargo-release`](https://github.com/crate-ci/cargo-release), [`cargo-dist`](https://github.com/axodotdev/cargo-dist), [`release-plz`](https://github.com/release-plz/release-plz) (auto-PR с changelog).
- **Семвер и совместимость:** [`cargo-semver-checks`](https://github.com/obi1kenobi/cargo-semver-checks).

**Где изучать:**

- The Cargo Book — <https://doc.rust-lang.org/cargo/>.
- «Zero To Production In Rust» (Luca Palmieri) — лучшая книга по практическому продакшну на Rust.
- Документация и README цитированных выше cargo-расширений.
- Блог [`fasterthanli.me`](https://fasterthanli.me/) — про реальный продакшн на Rust.

**Зачем это нужно:** «работает на моей машине» в Rust — это ещё полпути до продакшна. Хороший DevEx (тесты, кросс-сборка, бинарники, релизы) экономит десятки часов и делает крейт привлекательным для контрибьюторов.

**Мини-проект:**

- Настрой полный CI для одного из своих крейтов: тесты на Linux/macOS/Windows, `cargo-deny`, `cargo-semver-checks`, релизы через `cargo-dist`.
- Сделай Docker-образ <10 MB на musl + scratch.

## Этап 25. Контрибьюшн и развитие языка

**Что изучать:**

- **RFC-процесс.** Как предлагается изменение языка/стандартной библиотеки: <https://github.com/rust-lang/rfcs>. Прочитай несколько принятых и отклонённых RFC, чтобы понять стиль аргументации.
- **Структура `rust-lang/rust`:** компилятор (`rustc`), стандартная библиотека (`core`/`alloc`/`std`), `cargo`, `rustdoc`, `miri`, `clippy`, `rustfmt`.
- **Внутренние представления компилятора:** AST → HIR → THIR → MIR → LLVM IR. Каждый уровень решает свою задачу: парсинг, проверка типов, borrow checking, оптимизации.
- **Borrow checker:** старая версия (lexical lifetimes) → NLL (Non-Lexical Lifetimes) → [Polonius](https://rust-lang.github.io/polonius/) — будущий движок на Datalog.
- **Edition-механизм** (2015 → 2018 → 2021 → 2024). Как Rust развивается без поломки совместимости.
- **Информационные источники для core-разработки:** «[This Week in Rust](https://this-week-in-rust.org/)», [Inside Rust Blog](https://blog.rust-lang.org/inside-rust/), [Rust Internals](https://internals.rust-lang.org/), Zulip-чаты команд (`compiler`, `lang`, `libs`).
- **Менторство:** [`rustc-dev-guide`](https://rustc-dev-guide.rust-lang.org/) — официальное руководство по разработке самого компилятора. Лейбл `E-mentor` в issue-трекере `rust-lang/rust`.

**Где изучать:**

- The `rustc` Dev Guide — <https://rustc-dev-guide.rust-lang.org/>.
- The Rust Reference — <https://doc.rust-lang.org/reference/>.
- Доклады RustConf «What's new in rustc» и «Inside Polonius».
- Книги: «Rustc Codegen Cranelift», блог-посты Niko Matsakis.

**Зачем это нужно:** это вершина пути. Понимание устройства компилятора и языкового процесса делает тебя «Rust-инженером с большой буквы», даёт долгосрочное влияние на экосистему и открывает доступ к самым сложным и интересным задачам.

**Мини-проект:**

- Найди `E-mentor` или `E-easy` issue в `rust-lang/rust` или `rust-lang/cargo`, сделай PR.
- Напиши свой clippy-lint или мини-инструмент анализа на базе `rustc_driver` / `rustc_lint`.

---

# 💡 На что ещё стоит обратить внимание

- **Метакогнитивные навыки.** Умение читать сообщения компилятора как документацию — половина продуктивности в Rust. `cargo expand`, `cargo asm`, Rust Playground с просмотром MIR / LLVM IR — must-have для понимания, что реально происходит.
- **Чтение исходников в порядке возрастания сложности:** `itertools` → `serde` → `tokio` → `hyper` → `rustc`. Это даёт больше, чем десяток книг.
- **Сообщество.** Rust Users Forum (users.rust-lang.org), Rust Internals, Discord/Zulip Rust, русскоязычный чат [@rust_chats](https://t.me/rust_chats) и [группа ВК Rust IT](https://vk.com/rust_it). Ревью своего кода у опытных растовиков ускоряет рост на месяцы.
- **Книги продвинутого уровня:**
  - *Rust for Rustaceans* — Jon Gjengset.
  - *Programming Rust* — Blandy / Orendorff / Tindall (O'Reilly).
  - *Rust Atomics and Locks* — Mara Bos (обязательна перед серьёзной многопоточкой).
  - *Zero To Production In Rust* — Luca Palmieri (практика веб-сервисов).
  - *The Rustonomicon* — для unsafe.
- **YouTube:** Jon Gjengset (стримы по внутренностям), fasterthanli.me (Amos), Logan Smith, No Boilerplate.

---

# 🎯 Советы по изучению языка

## Общая стратегия

- **Не пытайся выучить всё сразу.** Rust большой: ownership, lifetimes, async, макросы, unsafe, типовая система. Если штурмовать всё параллельно — выгоришь. Двигайся по roadmap последовательно: каждый этап опирается на предыдущий.
- **Пиши код каждый день, хотя бы 30 минут.** Чтение книг и просмотр видео даёт иллюзию понимания. Реальное знание приходит только через борьбу с компилятором. Маленькие ежедневные коммиты сильнее редких марафонов.
- **Доверяй компилятору.** Сообщения об ошибках в Rust — это не препятствие, а самый честный учитель. Читай их полностью, переходи по ссылкам `--explain E0382`, не игнорируй warnings от clippy.
- **Не борись с borrow checker — слушай его.** Если код не компилируется, чаще всего проблема в архитектуре, а не в правилах языка. Перепиши структуру данных, разорви циклические зависимости, подумай о владении заранее.

## Практические привычки

- **Решай задачи на Rustlings в самом начале** — это 90+ маленьких упражнений, закрывающих базовый синтаксис. После них пройди Exercism Rust track с менторскими ревью.
- **Читай официальную «The Rust Book»** ([rust-book.cs.brown.edu](https://rust-book.cs.brown.edu/) — версия с интерактивными квизами). Затем «Rust by Example» для практических паттернов.
- **Используй `cargo clippy` и `cargo fmt` с первого дня.** Clippy подсказывает идиоматичные конструкции — это ускоряет переход от «пишу как на C++/Python» к «пишу как растовик».
- **Не клонируй ради тишины компилятора.** Если возникает желание поставить `.clone()` или `Arc<Mutex<_>>` — остановись. Чаще всего нужно поменять сигнатуру (`&str` вместо `String`, итератор вместо `Vec`) или переосмыслить владение.
- **Объясняй код вслух (rubber duck debugging).** Проговаривание «здесь у меня владелец, здесь заёмщик, время жизни `'a` связано с...» закрепляет ментальную модель быстрее, чем чтение.

## Работа с проектами

- **Делай pet-проекты, а не туториалы.** Туториалы дают ложное чувство прогресса. Возьми идею, которую тебе реально хочется реализовать — CLI-утилиту, бота, парсер логов — и пиши её с нуля.
- **Переписывай на Rust то, что уже знаешь.** Если у тебя есть скрипт на Python или сервис на Go — перепиши его. Сравнение даст глубокое понимание различий в моделях памяти и абстракциях.
- **Читай чужой код.** Открывай исходники крейтов, которые используешь (`serde`, `tokio`, `clap`) — даже без полного понимания. Постепенно паттерны станут узнаваемы.
- **Контрибьють в open-source раньше, чем кажется готовым.** Найди issue с лейблом `good-first-issue` или `E-easy` в любом популярном крейте. Ревью от мейнтейнеров — лучший ускоритель роста.

## Ментальные установки

- **Принимай, что первые 1–3 месяца будут болезненными.** Кривая обучения у Rust крутая в начале и пологая дальше. Это нормально. Все растовики проходили через стадию «ничего не компилируется».
- **Учись думать в терминах типов, а не в терминах действий.** Rust поощряет «сделай невалидное состояние непредставимым»: используй enum для состояний, newtype для разных единиц измерения, type-state для гарантий компиляции.
- **Не путай «сложно» и «непривычно».** Lifetimes выглядят страшно, но они описывают то, что в C++ существует неявно — просто Rust заставляет это записать. Через пару месяцев они станут естественной частью мышления.
- **Сравнивай Rust осознанно.** «В Go проще» или «в Haskell мощнее» — обе мысли верны и обе бесполезны. Rust выбирает компромисс между производительностью, безопасностью и эргономикой. Понимание этого компромисса важнее, чем выбор «лучшего языка».

## Где тормозить

- **Не лезь в `unsafe` и FFI слишком рано.** Это мощные, но опасные инструменты. Освой safe Rust до автоматизма прежде, чем брать ответственность за инварианты, которые компилятор больше не проверяет.
- **Не начинай с async.** Многие пытаются стартовать с `tokio` и веб-серверов. Async добавляет ещё один слой сложности (`Pin`, `Future`, runtime) поверх и без того нетривиальной модели владения. Сначала — синхронный код, потоки, каналы. Async — после уверенного владения этапами 1–6.
- **Не увлекайся продвинутыми трейтами без необходимости.** GAT, HRTB, type-state — мощные инструменты, но в 95% production-кода их не нужно. Сначала научись писать обычный идиоматичный Rust.

## Полезные ресурсы для практики

- [Rustlings](https://github.com/rust-lang/rustlings) — упражнения по синтаксису.
- [Exercism](https://exercism.org/tracks/rust) — задачи с ментор-ревью.
- [Rust by Example](https://doc.rust-lang.org/rust-by-example/) — паттерны через примеры.
- [Rust Playground](https://play.rust-lang.org/) — экспериментировать без `cargo new`.
- [Advent of Code на Rust](https://adventofcode.com/) — отличная сезонная практика.
- [Codewars](https://www.codewars.com/) и [LeetCode](https://leetcode.com/) на Rust — алгоритмические задачи.
- [Rust Quiz](https://dtolnay.github.io/rust-quiz/) — каверзные вопросы по семантике.

---

## 🤝 Вклад

Нашли опечатку, неточность или хотите добавить пример? Открывайте issue или присылайте pull request.

## 📜 Лицензия

Материалы курса распространяются под лицензией MIT. Используйте, копируйте, модифицируйте и распространяйте свободно.

---

Удачи в изучении Rust! 🦀
<img width="853" height="1280" alt="image" src="https://github.com/user-attachments/assets/d5396f5c-c938-4fb4-8e63-cd48067e9a64" />


