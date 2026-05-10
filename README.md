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


