# 🦀 Rust Roadmap (на русском)

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

- Установить `rustup` — официальный менеджер тулчейнов Rust. Он ставит компилятор (`rustc`), пакетный менеджер (`cargo`), форматтер (`rustfmt`), линтер (`clippy`) и позволяет переключаться между `stable` / `beta` / `nightly`.
- Настроить редактор: **VS Code + расширение rust-analyzer** — самая дружелюбная связка для новичков. Альтернативы: RustRover (бесплатен для некоммерческого использования), Helix, Zed, Neovim + LSP.
- Выучить ежедневные команды: `cargo new`, `cargo run`, `cargo build`, `cargo test`, `cargo check` (быстрая проверка типов без сборки бинаря), `cargo doc --open` (генерирует и открывает документацию проекта со всеми зависимостями).
- Создать первый проект и запустить его.

**Где изучать:**

- [rustup.rs](https://rustup.rs) — установка одной командой.
- [The Rust Book, гл. 1](https://doc.rust-lang.ru/book/ch01-00-getting-started.html) — «Введение».
- [Cargo Book](https://doc.rust-lang.org/cargo/) — официальный справочник по `cargo`.

**Зачем это нужно:** без рабочего окружения и правильных рефлексов (`cargo check` пока пишешь, `cargo run` для проверки, `cargo doc --open` чтобы читать доки оффлайн) Rust кажется сильно сложнее, чем есть. Настроишь этот этап правильно — дальше остаётся только учить язык.

**Мини-проект:** `"Hello, world!"`, который берёт твоё имя из `std::env::args` и здоровается по имени.

---

## Этап 1. Базовый синтаксис (2–3 недели)

**Что изучать:**

- Переменные: `let`, `mut`, константы (`const`), shadowing (повторное связывание имени через `let` — это идиома Rust, а не баг).
- Примитивные типы: целые (`i32`, `u64`, `usize`), плавающие (`f32`, `f64`), `bool`, `char` (4-байтный Unicode scalar value, **не байт**!), кортежи `(a, b)`, массивы фиксированного размера `[T; N]`.
- Управляющие конструкции: `if` / `else` (это **выражение**, а не оператор!), `loop`, `while`, `for`, `match` (мощный pattern matching Rust).
- Функции: параметры, типы возврата, разница между **выражением** (без точки с запятой, возвращает значение) и **оператором** (с точкой с запятой, ничего не возвращает).
- Модули и `use`: как разбить код на файлы, видимость через `pub`.
- Doc-комментарии (`///` для элементов, `//!` для модулей) — `cargo doc` превращает их в сайт.

**Где изучать:**

- [The Rust Book, гл. 3–7](https://doc.rust-lang.ru/book/ch03-00-common-programming-concepts.html) — все темы по порядку.
- [Rust by Example: Primitives, Custom Types, Variable Bindings, Types, Conversion, Expressions, Flow of Control, Functions, Modules](https://doc.rust-lang.org/rust-by-example/) — примеры под каждую тему.
- [Rustlings](https://github.com/rust-lang/rustlings) — секции `intro`, `variables`, `functions`, `if`, `primitive_types`, `vecs`, `move_semantics` (только лёгкие части), `structs`, `enums`, `strings`, `modules`, `hashmaps`. Обязательно.

**Зачем это нужно:** это кирпичики. Без них ничего не построишь, и они должны быть автоматическими — чтобы через три месяца не заглядывать в синтаксис `match`.

**Мини-проекты:** калькулятор (распарсить два числа и оператор), угадай число (классика из Rust Book), конвертер температур (C ↔ F), FizzBuzz через `match`.

---

## Этап 2. Владение, заимствование, времена жизни (3–4 недели) — самое важное!

**Что изучать:**

- Модель памяти: **стек** vs **куча**, что где аллоцируется и почему это важно для производительности.
- **Ownership** — центральная идея Rust: у каждого значения ровно один владелец; когда владелец выходит из области видимости, значение уничтожается (память освобождается) автоматически. Без GC, без ручного `free`.
- **Move-семантика**: присваивание или передача значения переносит владение. Использование исходной переменной после move — ошибка компиляции. Так Rust предотвращает use-after-free.
- `Copy` vs `Clone`: маленькие типы (`i32`, `bool`, `char`) реализуют `Copy` и копируются неявно; `Clone` — это явная, потенциально дорогая копия, которую ты вызываешь сам (`.clone()`).
- **Заимствование**: `&T` (общая, только для чтения ссылка) и `&mut T` (эксклюзивная, мутабельная ссылка). Borrow checker гарантирует: либо много читателей, либо один писатель — никогда одновременно. Так Rust ловит data races на этапе компиляции.
- **Срезы**: `&str` (вид в `String`), `&[T]` (вид в `Vec<T>` или массив). Срезы — способ передать «кусок данных» без копирования.
- **Времена жизни** (`'a`): аннотации, говорящие компилятору, как долго ссылка валидна. **В большинстве случаев их не пишут** — правила элизии выводят их сами. Учить нужно, чтобы понимать их, когда они появятся.
- **Строки**: разделение `String` (владеющая, на куче, растущая) vs `&str` (заимствованный вид). UTF-8 означает, что `s[0]` **не работает** — строки это последовательности байт, а не символов.

**Где изучать:**

- [The Rust Book, гл. 4 (Понимание владения) и гл. 10 §3 (Lifetimes)](https://doc.rust-lang.ru/book/ch04-00-understanding-ownership.html) — каноническое объяснение. Прочитать дважды.
- [Rust by Example: Scoping rules, Borrowing, Lifetimes](https://doc.rust-lang.org/rust-by-example/scope.html).
- [Rustlings: `move_semantics`, `lifetimes`, `strings`](https://github.com/rust-lang/rustlings) — пройти всё.
- [Rust Book с интерактивными квизами от Brown University](https://rust-book.cs.brown.edu/) — тот же материал плюс квизы, которые ловят типичные заблуждения именно про ownership. Очень рекомендую на этом этапе.
- YouTube: [Crust of Rust by Jon Gjengset — «Lifetime Annotations»](https://www.youtube.com/watch?v=rAl-9HwD858) когда почувствуешь готовность.

**Зачем это нужно:** именно это **то самое**, что делает Rust *Rust*. Почти все «почему мой код не компилируется?!» сводятся к ownership. Когда щёлкнет (а оно щёлкнет, обычно к 3-й неделе), остальной язык становится лёгким. Не торопи этот этап — вкладывайся, ты делаешь это один раз.

**Мини-проекты:** клон `wc` (считает строки/слова/байты в файле), парсер CSV без сторонних крейтов, счётчик частоты слов в текстовом файле.

---

## Этап 3. Структуры данных и абстракции (3 недели)

**Что изучать:**

- `struct` (с именованными полями), tuple-структуры (`struct Point(f64, f64)`), unit-структуры.
- `enum` — это **sum types** (мощнее, чем enums в C/Java). Так моделируются «значение — одно из этих вариантов».
- `Option<T>` и `Result<T, E>` — ответ Rust на `null` и исключения. Оба — обычные enum'ы из stdlib!
- **Pattern matching**: `match`, `if let`, `while let`, `let-else`. Так разбирают enum'ы и делают невалидные состояния непредставимыми.
- `impl`-блоки: методы (`&self`, `&mut self`, `self`), ассоциированные функции (без `self`, как `String::new()`).
- **Трейты**: интерфейсы Rust. Дефолтные реализации методов, авто-вывод через `#[derive(Debug, Clone, PartialEq)]`.
- **Дженерики**: `fn largest<T: Ord>(items: &[T]) -> &T`. Trait bounds (`T: Ord`), `where`-клаузы для читаемости.
- **Trait objects**: `dyn Trait`, `Box<dyn Trait>` — runtime-полиморфизм. Разница между **статическим диспатчем** (`impl Trait`, дженерики → мономорфизация, быстро) и **динамическим диспатчем** (`dyn Trait` → vtable, гибко).
- Стандартные коллекции: `Vec<T>` (растущий массив), `HashMap<K, V>`, `HashSet<T>`, `BTreeMap` (отсортированный), `VecDeque` (двусторонняя очередь).

**Где изучать:**

- [The Rust Book, гл. 5–6, 8, 10, 18](https://doc.rust-lang.ru/book/).
- [Rust by Example: Custom Types, Conversion, Generics, Traits](https://doc.rust-lang.org/rust-by-example/).
- [Rustlings: `structs`, `enums`, `options`, `error_handling`, `generics`, `traits`, `lifetimes`, `iterators`](https://github.com/rust-lang/rustlings).
- [Rust API Guidelines](https://rust-lang.github.io/api-guidelines/) — чтобы сразу делать идиоматичные типы.

**Зачем это нужно:** struct + enum + traits — это то, как ты *моделируешь* предметную область в Rust. Когда начнёшь использовать `enum` для состояний и `Option` вместо `null`, целые классы багов исчезают.

**Мини-проекты:** телефонная книга в CLI (потом сохранение/загрузка в JSON), CLI todo-list, простой граф (список смежности через `HashMap<NodeId, Vec<NodeId>>`).

---

## Этап 4. Обработка ошибок и тестирование (1–2 недели)

**Что изучать:**

- `panic!`, `unwrap`, `expect` — *быстрый и грязный* способ: программа падает при ошибке. Норм для прототипов и примеров из Rust Book, **не норм** для продакшена.
- Идиоматичная обработка: `Result<T, E>` везде, **оператор `?`** для проброса ошибки вверх по стеку одним символом.
- Свои типы ошибок через [`thiserror`](https://github.com/dtolnay/thiserror) — для **библиотек**, где потребителям нужно матчить конкретные варианты.
- [`anyhow`](https://github.com/dtolnay/anyhow) — для **приложений**, когда хочется «что-то пошло не так, вот цепочка причин». Не путать: библиотеки → `thiserror`, бинари → `anyhow`.
- Конверсии `From` / `Into` — как `?` автоматически превращает один тип ошибки в другой.
- **Тестирование**: функции с `#[test]`, `cargo test`, `assert!` / `assert_eq!`, `#[should_panic]`. Юнит-тесты живут рядом с кодом (в `mod tests`); **интеграционные** — в `tests/`. **Doc-тесты** — это исполняемый код в `///`-комментариях, одновременно документация и тест.
- Бенчмарки через [`criterion`](https://github.com/bheisler/criterion.rs) — нормальные статистические бенчи (встроенный `#[bench]` доступен только на nightly).

**Где изучать:**

- [The Rust Book, гл. 9, 11, 14 §3](https://doc.rust-lang.ru/book/ch09-00-error-handling.html).
- [README `thiserror`](https://github.com/dtolnay/thiserror) и [README `anyhow`](https://github.com/dtolnay/anyhow) — короткие, прочитать оба.
- Статья: [«Error Handling In Rust - A Deep Dive» Luca Palmieri](https://www.lpalmieri.com/posts/error-handling-rust/) — лучший разбор темы за один заход.

**Зачем это нужно:** ошибки — это половина боевого кода. `Result + ?` делает их обработку приятной *и* явной — ты буквально не можешь молча проигнорировать ошибку. Тесты дают уверенность для рефакторинга; без них любое изменение страшно.

**Мини-проект:** CLI-утилита для валидации файлов — читает, парсит, возвращает список ошибок с номерами строк. Свой тип ошибки через `thiserror` плюс интеграционные тесты.

---

## Этап 5. Умные указатели и интериор-мутабельность (2 недели)

**Что изучать:**

- `Box<T>` — аллокация на куче. Нужен, когда тип имеет известный размер на компиляции, но ты хочешь его на куче (классика — рекурсивные типы: `enum List { Cons(i32, Box<List>), Nil }`).
- `Rc<T>` — *Reference Counted*, разделяемое владение в одном потоке. Несколько владельцев, освобождается, когда последний уходит.
- `Arc<T>` — *Atomic Reference Counted*, потокобезопасная версия `Rc<T>`. Чуть медленнее из-за атомарных операций.
- **Интериор-мутабельность**: как менять данные через `&T` (общую ссылку). Обычное правило Rust говорит «нельзя» — эти типы являются спасательными люками:
  - `Cell<T>` — для `Copy`-типов, get/set по значению, без borrow.
  - `RefCell<T>` — runtime-проверка borrow, один поток.
  - `Mutex<T>` — как `RefCell`, но потокобезопасно (блокирует).
  - `RwLock<T>` — много читателей или один писатель, потокобезопасно.
  - `OnceLock<T>` / `OnceCell<T>` — записать один раз, читать много (ленивая инициализация).
- **Циклы ссылок** с `Rc` приводят к утечкам памяти. Решение — `Weak<T>` (не-владеющая ссылка, не удерживает значение).
- Трейты `Deref` и `Drop` — как работает `*box_value` и как RAII («захват ресурса = инициализация») даёт автоматическую уборку.
- `Cow<'a, T>` (Clone-on-Write) — «заимствую, если могу; клонирую, если надо», часто встречается в строковых функциях, которые могут модифицировать вход, а могут нет.

**Где изучать:**

- [The Rust Book, гл. 15 (умные указатели), гл. 16 (многопоточность, частичный overlap)](https://doc.rust-lang.ru/book/ch15-00-smart-pointers.html).
- [Rust by Example: Box, Rc, Cell/RefCell](https://doc.rust-lang.org/rust-by-example/std/box.html).
- Классика: [«Learn Rust With Entirely Too Many Linked Lists»](https://rust-unofficial.github.io/too-many-lists/) — реализуем 6 связных списков, каждый учит свой умный указатель. *Тот самый* туториал на этот этап.

**Зачем это нужно:** без умных указателей Rust ощущается зажатым («как мне это пошарить?!»). С ними понимаешь, *почему* обычные правила Rust такие строгие — они дают статические гарантии, а эти типы — аккуратно подписанные исключения.

**Мини-проект:** двусвязный список (через `Rc` + `RefCell` + `Weak`) или дерево, где дети знают своего родителя.

---

## Этап 6. Многопоточность (2–3 недели)

**Что изучать:**

- ОС-потоки через `std::thread::spawn`. Передача данных в поток через `move` (замыкание забирает владение).
- **Каналы** для передачи сообщений: `std::sync::mpsc` (встроенный), [`crossbeam`](https://github.com/crossbeam-rs/crossbeam) (быстрее, поддерживает multi-producer multi-consumer).
- **Маркер-трейты `Send` и `Sync`**: `Send` = «безопасно перенести в другой поток», `Sync` = «безопасно расшарить через `&` между потоками». Компилятор это форсирует — *fearless concurrency* в действии.
- **Атомики**: `AtomicUsize`, `AtomicBool` и т.д., и enum `Ordering` (`Relaxed`, `Acquire`, `Release`, `SeqCst`). Большинству кода хватит `SeqCst` или `Relaxed`; остальное — для перформанса.
- `Mutex` / `RwLock` для разделяемого состояния, когда message passing не подходит.
- **Scoped threads** (`std::thread::scope`, стабильно с 1.63) — позволяют потокам заимствовать стековые данные без требования `'static`.
- [`rayon`](https://github.com/rayon-rs/rayon) — data-параллелизм одной строкой: меняешь `.iter()` на `.par_iter()` и цикл идёт по всем ядрам.

**Где изучать:**

- [The Rust Book, гл. 16 (Fearless Concurrency)](https://doc.rust-lang.ru/book/ch16-00-concurrency.html).
- [Rust Atomics and Locks (Mara Bos)](https://marabos.nl/atomics/) — бесплатно онлайн, *та самая* книга по теме. Читать после главы Rust Book.
- [README `rayon` и его docs](https://docs.rs/rayon/) — короткие, сразу полезные.

**Зачем это нужно:** многопоточность — место, где большинство языков пропускают баги (data races, дедлоки). Rust ловит data races на компиляции. Это настоящая суперсила — пользуйся.

**Мини-проект:** параллельный word-counter (разбить большой текст на куски, посчитать на нескольких потоках, смержить), или параллельный обработчик изображений на `rayon`.

---

## Этап 7. Асинхронное программирование (3–4 недели)

**Что изучать:**

- Трейт `Future` — async-кирпичик Rust. Future — это *стейт-машина*, которую рантайм опрашивает до готовности. Ключевое: **future ничего не делает, пока ты не сделаешь `.await` или не отдашь его рантайму**.
- Синтаксис `async fn` и `.await` — эргономичный слой поверх `Future`.
- **Рантаймы**: [`tokio`](https://tokio.rs/) (де-факто стандарт, многопоточный), [`async-std`](https://async.rs/), [`smol`](https://github.com/smol-rs/smol). В 95% случаев — `tokio`.
- Стримы (`futures::Stream`) — асинхронные итераторы.
- `tokio::select!` — гонка нескольких future; первый завершившийся выигрывает. **Внимательно читай доки** — `select!` отменяет остальные ветки, что приводит к тонким проблемам корректности («cancellation safety»).
- `tokio::spawn` (асинхронные задачи) vs `tokio::task::spawn_blocking` (запустить блокирующий код, не заморозив рантайм).
- Async-сеть: TCP/UDP через `tokio::net`, HTTP-клиент [`reqwest`](https://github.com/seanmonstar/reqwest), HTTP-сервер [`axum`](https://github.com/tokio-rs/axum) или [`actix-web`](https://actix.rs/).

**Где изучать:**

- [The Rust Async Book](https://rust-lang.github.io/async-book/) — официально.
- [Tokio Tutorial](https://tokio.rs/tokio/tutorial) — дружелюбный, на практике. **Начинай с него.**
- [«Async Rust in 2024» Niko Matsakis](https://smallcultfollowing.com/babysteps/) и [Without Boats](https://without.boats/) — чтобы понять, *почему* async Rust устроен так.
- Статья: [«Async: What is blocking?» Alice Ryhl](https://ryhl.io/blog/async-what-is-blocking/) — отвечает на вопрос, который у каждого новичка.

**Зачем это нужно:** современные сервисы упираются в сеть. Async Rust даёт Go-подобную конкурентность с C-подобным потреблением ресурсов и без GC-пауз. И это самая *запутанная* часть языка для новичков — поэтому этот этап идёт *после* того, как ты уверенно владеешь ownership, потоками и умными указателями.

**Мини-проект:** асинхронный загрузчик HTTP (тащит N URL параллельно, сохраняет на диск), или простой async чат-сервер (TCP, broadcast между клиентами).

---

## Этап 8. Макросы и метапрограммирование (2 недели)

**Что изучать:**

- **Декларативные макросы** — `macro_rules!`. Pattern-match по токенам, разворачиваются в код. Ты уже пользуешься: `println!`, `vec!`, `assert_eq!` — это они.
- **Процедурные макросы** — три вида: `derive` (`#[derive(MyTrait)]`), атрибутные (`#[my_attribute]`), функциональные (`my_macro!(...)`). Живут в отдельном крейте с `proc-macro = true`.
- Тройка для проц-макросов: [`syn`](https://docs.rs/syn) (парсит Rust в AST), [`quote`](https://docs.rs/quote) (генерирует Rust по шаблону), [`proc-macro2`](https://docs.rs/proc-macro2) (стабильная обёртка над нестабильным `proc_macro`).
- `cargo expand` — посмотреть, во что разворачивается макрос. Незаменимо для отладки.

**Где изучать:**

- [The Rust Book, гл. 19 §6 (Макросы)](https://doc.rust-lang.ru/book/ch19-06-macros.html) — мягкое введение.
- [The Little Book of Rust Macros](https://veykril.github.io/tlborm/) — исчерпывающий справочник по `macro_rules!`.
- [Procedural Macros Workshop от dtolnay](https://github.com/dtolnay/proc-macro-workshop) — пошаговая практика.
- YouTube: [Crust of Rust: Declarative Macros](https://www.youtube.com/watch?v=q6paRBbLgNw) от Jon Gjengset.

**Зачем это нужно:** макросы ты будешь *использовать* постоянно (`derive`, `tokio::main`, `serde::Deserialize`); писать свои — только когда реально нужно убрать boilerplate. Не макросизируй преждевременно.

**Мини-проект:** свой `derive(Builder)` (генерит builder-паттерн для структуры) — каноническое упражнение по проц-макросам, оно же одно из заданий dtolnay-воркшопа.

---

## Этап 9. Unsafe Rust и FFI (2 недели)

**Что изучать:**

- Что реально открывает `unsafe`: разыменование сырых указателей, вызов `unsafe`-функций, доступ/изменение мутабельных статиков, реализация `unsafe`-трейтов, доступ к полям `union`. **Всё** — `unsafe` *не* отключает borrow checker.
- Сырые указатели `*const T` и `*mut T`. `transmute` (почти всегда не то, что нужно; крайний случай).
- **FFI с C**: `extern "C" fn`, структуры с `#[repr(C)]`, [`bindgen`](https://github.com/rust-lang/rust-bindgen) (генерит Rust-биндинги по C-хедерам), [`cbindgen`](https://github.com/mozilla/cbindgen) (наоборот).
- Атрибуты layout: `#[repr(C)]`, `#[repr(transparent)]`, `#[repr(packed)]`.
- [Miri](https://github.com/rust-lang/miri) — интерпретатор, который ловит UB в твоём `unsafe`-коде. Прогоняй тесты через `cargo +nightly miri test`.

**Где изучать:**

- [The Rustonomicon](https://doc.rust-lang.org/nomicon/) — *главная* книга про unsafe. Читать после уверенного safe Rust.
- [The Rust Book, гл. 19 §1 (Unsafe Rust)](https://doc.rust-lang.ru/book/ch19-01-unsafe-rust.html) — мягкое введение.
- [Rust FFI Omnibus](http://jakegoulding.com/rust-ffi-omnibus/) — практические FFI-рецепты.

**Зачем это нужно:** в `unsafe` ты берёшь на себя ответственность за инварианты, которые компилятор больше не проверяет. Большинство прикладного кода `unsafe` не использует. Авторам библиотек, которые пишут низкоуровневые абстракции, FFI-обвязки или кэш-эффективные структуры — нужен.

**Мини-проект:** безопасная Rust-обёртка над маленькой C-библиотекой (например, хеширование или сжатие) — генерим биндинги через `bindgen`, поверх делаем идиоматичный API.

---

## Этап 10. Экосистема и реальные проекты

**Что изучать — по доменам:**

- **Веб-бэкенды:** [`axum`](https://github.com/tokio-rs/axum) (рекомендую новичкам — построен на `tokio` + `tower`), [`actix-web`](https://actix.rs/), [`rocket`](https://rocket.rs/), [`warp`](https://github.com/seanmonstar/warp).
- **Базы данных:** [`sqlx`](https://github.com/launchbadge/sqlx) (async, проверка SQL на компиляции — современный дефолт), [`diesel`](https://diesel.rs/) (sync ORM с сильной типизацией), [`sea-orm`](https://www.sea-ql.org/SeaORM/) (async ORM), [`redis-rs`](https://github.com/redis-rs/redis-rs).
- **Сериализация:** [`serde`](https://serde.rs/) — *тот самый* фреймворк, поддерживает JSON, YAML, TOML, MessagePack, bincode и десятки других. [`serde_json`](https://github.com/serde-rs/json), [`bincode`](https://github.com/bincode-org/bincode), [`prost`](https://github.com/tokio-rs/prost) (Protocol Buffers).
- **CLI:** [`clap`](https://github.com/clap-rs/clap) (парсинг аргументов, derive-макросы), [`indicatif`](https://github.com/console-rs/indicatif) (прогресс-бары), [`console`](https://github.com/console-rs/console) (цветной вывод).
- **Логирование и трейсинг:** [`tracing`](https://github.com/tokio-rs/tracing) (структурные логи + spans, современный выбор), [`log`](https://github.com/rust-lang/log) (старее, проще).
- **gRPC:** [`tonic`](https://github.com/hyperium/tonic) — стандарт.
- **Embedded:** [`embedded-hal`](https://github.com/rust-embedded/embedded-hal), платы вроде ESP32 (через [`esp-rs`](https://github.com/esp-rs)), STM32 (через [`stm32-rs`](https://github.com/stm32-rs)).
- **WebAssembly:** [`wasm-bindgen`](https://github.com/rustwasm/wasm-bindgen), фронтенд-фреймворки [`yew`](https://yew.rs/), [`leptos`](https://leptos.dev/), [`dioxus`](https://dioxuslabs.com/).
- **Геймдев:** [`bevy`](https://bevyengine.org/) (data-driven ECS, самый популярный), [`macroquad`](https://github.com/not-fl3/macroquad), [`ggez`](https://ggez.rs/).
- **ML / AI:** [`candle`](https://github.com/huggingface/candle) (HuggingFace), [`burn`](https://github.com/tracel-ai/burn), [`tch-rs`](https://github.com/LaurentMazare/tch-rs).

**Где изучать:**

- Страница каждого крейта на `docs.rs` и его README на GitHub. Документация в Rust необычно хорошая — сначала туда.
- [Zero To Production In Rust](https://www.zero2prod.com/) (Luca Palmieri) — полный тур по `axum`+`sqlx`+`tracing`+Docker+CI/CD на примере боевого newsletter-сервиса. Платная, но самое связное введение в продакшн-Rust.
- [«Are We X Yet?»](https://wiki.mozilla.org/Areweyet) — быстрая проверка реальности: готов ли твой домен к Rust.

**Зачем это нужно:** язык даёт инструменты; экосистема — это то, что ты реально шипаешь. Выбери **один** домен, который тебя зажигает (web, embedded, игры, ML…), и сделай что-то настоящее. Туториалы не учат тому, чему учит зашипанный проект.

---

## Этап 11. Уровень профи

**Что изучать:**

- **Профилирование**: `perf` + [cargo-flamegraph](https://github.com/flamegraph-rs/flamegraph) (Linux), [Instruments](https://developer.apple.com/xcode/) (macOS), [`samply`](https://github.com/mstange/samply) (кросс-платформенно).
- **Оптимизация**: SIMD через `std::simd` (nightly) или `std::arch` (stable), хинты `#[inline]`, struct-of-arrays vs array-of-structs, cache-friendly layout.
- **Fuzz-тестирование**: [`cargo-fuzz`](https://github.com/rust-fuzz/cargo-fuzz) (libFuzzer), [`afl`](https://github.com/rust-fuzz/afl.rs). **Property-based**: [`proptest`](https://github.com/proptest-rs/proptest), [`quickcheck`](https://github.com/BurntSushi/quickcheck).
- **Чтение исходников** зрелых крейтов: `itertools` → `serde` → `tokio` → `hyper` → `rustc` (по нарастающей сложности). Это самый быстрый путь от среднего до продвинутого.
- **Open-source-вклад**: бери `E-easy` / `good-first-issue` в любом популярном крейте. Ревью от мейнтейнеров ускоряет рост сильнее любой книги.
- **Публикация своего крейта** на [crates.io](https://crates.io): хорошие доки, CI на GitHub Actions, [семвер](https://semver.org/), [`cargo-semver-checks`](https://github.com/obi1kenobi/cargo-semver-checks) перед каждым релизом.
- **Nightly-фичи и RFC-процесс**: читай [Inside Rust Blog](https://blog.rust-lang.org/inside-rust/), [`rust-lang/rfcs`](https://github.com/rust-lang/rfcs), подпишись на [This Week in Rust](https://this-week-in-rust.org/).

**Зачем это нужно:** на этом уровне ты перестаёшь *учить Rust* и начинаешь *думать на Rust*. Лучшие инженеры публикуют библиотеки, которыми пользуются другие, шлют фиксы апстрим и понимают компромиссы дизайнеров языка.

---

# 🚀 Расширение roadmap: продвинутые темы

## Этап 12. Продвинутая типовая система

- Higher-Ranked Trait Bounds (`for<'a>`), GAT (Generic Associated Types).
- Associated types vs дженерики — когда что выбирать.
- `PhantomData`, zero-sized types, типобезопасные API.
- Type-state pattern (`Builder<Unbuilt>` → `Builder<Built>`) — кодировать состояния протокола в типах, чтобы неправильное использование стало ошибкой компиляции.
- Sealed traits, расширяемые закрытые API.
- Newtype-паттерн (`struct UserId(u64)`) — разные типы для разных смыслов, без рантайм-стоимости.
- Variance: covariance, contravariance, invariance (`&mut T` инвариантен).
- Auto-traits, negative impls (`!Send`, `!Sync`), `?Sized`, dynamically sized types (DST).

## Этап 13. Глубокая работа с асинхронностью

- Внутренности `Future`: `Poll`, `Pin`, `Unpin`, self-referential типы.
- Устройство рантайма: reactor, executor, waker, task scheduling.
- Cooperative scheduling, проблема блокирующих вызовов в async.
- `tokio::spawn` vs `spawn_blocking`, `LocalSet`, `JoinSet`.
- **Cancellation safety** и почему `select!` опасен, если не понимать его.
- Структурированная конкурентность (`tokio-util`, `async-scoped`).
- Backpressure в стримах.
- `async-trait` и нативные async fn в трейтах (стабильны с 1.75).
- Отладка async-кода: [`tokio-console`](https://github.com/tokio-rs/console), `tracing` spans.

## Этап 14. Производительность и низкий уровень

- Cache-friendly структуры, false sharing, padding.
- SoA vs AoS (struct of arrays) — выбор раскладки, который часто доминирует в перформансе.
- Branch prediction, хинты `likely` / `unlikely`.
- SIMD: `std::simd` (nightly), `std::arch` (stable, платформенные интринсики), крейт [`wide`](https://github.com/Lokathor/wide).
- Allocator API, кастомные аллокаторы ([`bumpalo`](https://github.com/fitzgen/bumpalo), [`mimalloc`](https://github.com/microsoft/mimalloc), [`jemallocator`](https://github.com/tikv/jemallocator)).
- Arena-аллокация, [`typed-arena`](https://github.com/SimonSapin/rust-typed-arena).
- Zero-copy парсинг ([`nom`](https://github.com/rust-bakery/nom), [`winnow`](https://github.com/winnow-rs/winnow), [`zerocopy`](https://github.com/google/zerocopy), [`rkyv`](https://github.com/rkyv/rkyv)).
- Профилирование: `perf`, `cargo-flamegraph`, `samply`, `tracy`, `dhat-rs`, `cargo-bloat`.
- Benchmark-driven development ([`criterion`](https://github.com/bheisler/criterion.rs), [`iai`](https://github.com/bheisler/iai)).

## Этап 15. Безопасность и корректность

- Формальная верификация: [Kani](https://github.com/model-checking/kani), [Prusti](https://github.com/viperproject/prusti-dev), [Creusot](https://github.com/creusot-rs/creusot).
- Model checking конкурентности: [`loom`](https://github.com/tokio-rs/loom), [`shuttle`](https://github.com/awslabs/shuttle).
- Sanitizers через nightly: `-Z sanitizer=address|thread|memory|leak`.
- Mutation testing ([`cargo-mutants`](https://github.com/sourcefrog/cargo-mutants)).
- Аудит зависимостей: [`cargo-audit`](https://github.com/rustsec/rustsec), [`cargo-deny`](https://github.com/EmbarkStudios/cargo-deny), [`cargo-vet`](https://github.com/mozilla/cargo-vet), [`cargo-geiger`](https://github.com/geiger-rs/cargo-geiger).
- Supply-chain security, reproducible builds.
- Безопасный FFI: ABI-стабильность, panic-unwinding, `catch_unwind`.

## Этап 16. Архитектура и паттерны проектирования

- Идиоматичный Rust: builder, typestate, RAII-guards, newtype.
- Hexagonal/Clean architecture без DI-фреймворков.
- Error handling: библиотека (`thiserror`) vs приложение (`anyhow`, `eyre`, `color-eyre`).
- Антипаттерны: борьба с borrow checker, избыточное `Arc<Mutex<_>>`, clone-driven development.
- Композиция вместо наследования, enum-полиморфизм vs trait objects.

## Этап 17. Системное программирование и ОС

- `mio`, `epoll` / `kqueue` / `io_uring` напрямую.
- `tokio-uring`, [`glommio`](https://github.com/DataDog/glommio) (thread-per-core).
- Shared memory, `mmap`, [`memmap2`](https://github.com/RazrFalcon/memmap2-rs).
- Сигналы Unix ([`signal-hook`](https://github.com/vorner/signal-hook)), процессы, pipes.
- Свой аллокатор, свой `Future`-executor с нуля.
- Kernel-разработка: Rust в Linux kernel, [Redox OS](https://www.redox-os.org/), [Theseus](https://www.theseus-os.com/).
- Bare-metal: `#![no_std]`, `core` vs `alloc` vs `std`, panic-handler, custom target.
- Bootloader и минимальная ОС (Philipp Oppermann «[Writing an OS in Rust](https://os.phil-opp.com/)»).

## Этап 18. Распределённые системы и сеть

- Реализация Redis-протокола, Raft ([`openraft`](https://github.com/datafuselabs/openraft)), gossip-протоколы.
- QUIC через [`quinn`](https://github.com/quinn-rs/quinn), HTTP/2 и HTTP/3.
- gRPC streaming, interceptors в `tonic`.
- Observability: OpenTelemetry, [`tracing-opentelemetry`](https://github.com/tokio-rs/tracing-opentelemetry), метрики ([`metrics`](https://github.com/metrics-rs/metrics), [`prometheus`](https://github.com/tikv/rust-prometheus)).
- Идемпотентность, retry, circuit breaker ([`tower`](https://github.com/tower-rs/tower), [`tower-http`](https://github.com/tower-rs/tower-http)).
- Event sourcing и CQRS на Rust.

## Этап 19. Базы данных и хранилища

- Глубже `sqlx`: compile-time проверка запросов, миграции (`sqlx-cli`, [`refinery`](https://github.com/rust-db/refinery)).
- ORM (`diesel`) vs query-builder ([`sea-query`](https://github.com/SeaQL/sea-query)).
- Connection pooling ([`deadpool`](https://github.com/bikeshedder/deadpool), [`bb8`](https://github.com/djc/bb8)).
- Embedded базы: [`sled`](https://github.com/spacejam/sled), [`redb`](https://github.com/cberner/redb), [`rocksdb`](https://github.com/rust-rocksdb/rust-rocksdb), [`fjall`](https://github.com/fjall-rs/fjall).
- Свой storage engine: B-Tree, LSM, WAL.
- Векторные БД и поиск: [`qdrant`](https://github.com/qdrant/qdrant), [`tantivy`](https://github.com/quickwit-oss/tantivy).

## Этап 20. WebAssembly углублённо

- `wasm32-unknown-unknown` vs `wasm32-wasi` vs `wasm32-wasip2`.
- Component Model и WIT-интерфейсы.
- [`wasmtime`](https://github.com/bytecodealliance/wasmtime), [`wasmer`](https://github.com/wasmerio/wasmer) как embedder'ы.
- Plugin-системы на WASM ([`extism`](https://extism.org/)).
- Frontend: сравнение `leptos`, `dioxus`, `yew`, `sycamore` (signals vs vdom).
- SSR, hydration, server functions.
- Edge-runtime (Cloudflare Workers, Fastly Compute).

## Этап 21. Embedded и реальное время

- `embedded-hal` 1.0, HAL-крейты для конкретных МК.
- RTOS на Rust: [Embassy](https://embassy.dev/) (async-first), [RTIC](https://rtic.rs/) (priority-based).
- DMA, прерывания, критические секции (`critical-section`).
- `defmt` для эффективного логирования.
- [`probe-rs`](https://probe.rs/) для отладки и прошивки.
- Безопасность памяти на `no_std`: [`heapless`](https://github.com/rust-embedded/heapless), статическое выделение.

## Этап 22. Геймдев и графика

- ECS в `bevy`: компоненты, системы, ресурсы, schedules.
- Шейдеры на WGSL, [`wgpu`](https://github.com/gfx-rs/wgpu) напрямую.
- Физика: [`rapier`](https://github.com/dimforge/rapier), [`avian`](https://github.com/Jondolf/avian).
- Networking для игр: [`lightyear`](https://github.com/cBournhonesque/lightyear), [`bevy_replicon`](https://github.com/projectharmonia/bevy_replicon).
- Альтернативы: [`fyrox`](https://github.com/FyroxEngine/Fyrox), `macroquad`, `ggez`.
- Графика низкого уровня: [`vulkano`](https://github.com/vulkano-rs/vulkano), [`ash`](https://github.com/ash-rs/ash) (Vulkan bindings).

## Этап 23. ML, AI и научные вычисления

- `candle` (HuggingFace) — minimalist tensor framework.
- `burn` — гибкий ML-фреймворк с разными бэкендами.
- `tch-rs` — биндинги к LibTorch.
- [`ort`](https://github.com/pykeio/ort) — ONNX Runtime.
- Линейная алгебра: [`nalgebra`](https://nalgebra.org/), [`ndarray`](https://github.com/rust-ndarray/ndarray), [`faer`](https://github.com/sarah-quinones/faer-rs).
- Распараллеливание: `rayon`, GPU через [`cust`](https://github.com/Rust-GPU/Rust-CUDA) (CUDA), [`cubecl`](https://github.com/tracel-ai/cubecl).

## Этап 24. Инструментарий, DevEx и продакшн

- Workspaces, feature flags, conditional compilation.
- Build scripts (`build.rs`).
- cargo-расширения: `cargo-make`, `cargo-nextest`, `cargo-watch`, `cargo-expand`, `cargo-udeps`, `cargo-machete`.
- Кросс-компиляция: [`cross`](https://github.com/cross-rs/cross), [`cargo-zigbuild`](https://github.com/rust-cross/cargo-zigbuild).
- Минимизация бинарника: LTO, `panic=abort`, `strip`, `opt-level="z"`.
- Docker: distroless и scratch-образы, multi-stage builds.
- CI/CD: GitHub Actions с [`Swatinem/rust-cache`](https://github.com/Swatinem/rust-cache), matrix-сборки.
- Релизы: [`cargo-release`](https://github.com/crate-ci/cargo-release), [`cargo-dist`](https://github.com/axodotdev/cargo-dist), [`release-plz`](https://github.com/release-plz/release-plz).
- Семантическое версионирование: `cargo-semver-checks`.

## Этап 25. Контрибьюшн и развитие языка

- RFC-процесс.
- Структура `rust-lang/rust`: компилятор (`rustc`), стандартная библиотека, cargo.
- MIR, HIR, THIR.
- Borrow checker (NLL, Polonius).
- Edition-механизм (2015 → 2018 → 2021 → 2024).
- «This Week in Rust», Inside Rust Blog.
- Менторство в [`rustc-dev-guide`](https://rustc-dev-guide.rust-lang.org/).

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
![Uploading image.png…]()

