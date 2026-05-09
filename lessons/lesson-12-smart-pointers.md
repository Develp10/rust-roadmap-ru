# Урок 12. Умные указатели и интериор-мутабельность

«Умный указатель» (smart pointer) — это структура, которая ведёт себя как указатель, но добавляет логику владения, подсчёт ссылок, динамическую диспетчеризацию или интериор-мутабельность. Самые используемые: `Box`, `Rc`/`Arc`, `RefCell`/`Cell`, `Mutex`/`RwLock` и пара `Weak`. Понимать их — значит уметь работать со «сложными» формами владения.

## Теория

### Что объединяет умные указатели

Все они реализуют один из трёх трейтов:

- `Deref` (или `DerefMut`) — позволяет писать `*p`, и компилятор сам подставит `(*p.deref())`. Также включается **deref coercion**: `&Box<String>` автоматически становится `&String`, который становится `&str`.
- `Drop` — кастомная логика при освобождении (RAII).
- Чаще всего ещё `AsRef`/`Borrow` для интеграции с API.

Именно эти трейты и делают тип «как бы указателем».

### Box<T> — владение в куче

Самый простой умный указатель. Размещает значение в куче, на стеке хранит только указатель.

```rust
let b = Box::new(5);
println!("{}", *b);    // 5 — разыменование через Deref
```

Когда нужен `Box`:

1. **Рекурсивные типы.** Без `Box` компилятор не может вычислить размер бесконечного типа.

   ```rust
   enum List { Cons(i32, Box<List>), Nil }
   let list = List::Cons(1, Box::new(List::Cons(2, Box::new(List::Nil))));
   ```

2. **Trait objects.** Размер `dyn Trait` неизвестен на стеке — нужен указатель.

   ```rust
   let animals: Vec<Box<dyn Animal>> = vec![Box::new(Dog), Box::new(Cat)];
   ```

3. **Большие значения**, которые не хочется копировать на стек.

4. **Передача владения** между потоками или функциями без копирования.

### Rc<T> — счётчик ссылок (однопоточный)

Несколько владельцев одного значения. Когда последний `Rc` дропается, значение освобождается.

```rust
use std::rc::Rc;

let a = Rc::new(String::from("hi"));
let b = Rc::clone(&a);              // +1 к счётчику, не глубокая копия
let c = a.clone();                  // эквивалентно
println!("{}", Rc::strong_count(&a));    // 3
```

Идиоматично писать `Rc::clone(&a)` вместо `a.clone()` — в коде сразу видно, что это «дешёвая» операция увеличения счётчика, а не глубокое копирование.

`Rc` **не реализует** `Send` и `Sync` — нельзя передавать между потоками, потому что счётчик не атомарен. Для многопоточности — `Arc`.

### Arc<T> — атомарный счётчик ссылок

Тот же `Rc`, но счётчик — атомик. Чуть медленнее (атомарные операции), зато потокобезопасен:

```rust
use std::sync::Arc;
use std::thread;

let data = Arc::new(vec![1, 2, 3]);
let handles: Vec<_> = (0..3).map(|i| {
    let d = Arc::clone(&data);
    thread::spawn(move || {
        let sum: i32 = d.iter().sum();
        println!("thread {i}: sum = {sum}");
    })
}).collect();
for h in handles { h.join().unwrap(); }
```

`Arc` **разделяет иммутабельные данные**. Чтобы менять — нужно сочетание с `Mutex` или `RwLock`.

### Интериор-мутабельность: зачем

Обычное правило: либо одна `&mut T`, либо много `&T`. Иногда нужно изменять данные через общую (иммутабельную) ссылку — например:

- кэш внутри иммутабельного объекта;
- внутреннее состояние, не видимое снаружи;
- разделяемые структуры через `Rc<RefCell<T>>`.

Для этого есть **interior mutability**: набор типов, которые проверяют правила заимствования **в рантайме**, а не на этапе компиляции.

### Cell<T> — для Copy-типов

`Cell` хранит значение и позволяет полностью заменять его без ссылок. Очень дешёвый, без локов:

```rust
use std::cell::Cell;

let c = Cell::new(5);
c.set(10);
println!("{}", c.get());      // 10
let old = c.replace(99);
println!("old = {old}, new = {}", c.get());
```

Главное ограничение: `get()` копирует значение, поэтому `Cell` хорош для `Copy`-типов.

### RefCell<T> — runtime borrow checker

`RefCell` хранит значение и предоставляет ссылки через `borrow()` (`Ref<T>`) и `borrow_mut()` (`RefMut<T>`). Те же правила, что и у обычных ссылок, но проверяются в рантайме — при нарушении паника:

```rust
use std::cell::RefCell;

let counter = RefCell::new(0);

*counter.borrow_mut() += 1;
*counter.borrow_mut() += 1;
println!("{}", counter.borrow());     // 2

{
    let r1 = counter.borrow();
    let r2 = counter.borrow();         // ок: несколько иммутабельных
    println!("{r1} {r2}");
}

// let _r1 = counter.borrow();
// let _r2 = counter.borrow_mut();      // ПАНИКА: уже есть immutable borrow
```

Когда выбрать `RefCell` вместо `Cell`: когда нужны ссылки внутрь данных, или тип не `Copy`.

### Rc<RefCell<T>> — совместное владение + изменяемость

Это самая частая комбинация в однопоточном коде, когда несколько мест хотят и читать, и писать одни данные:

```rust
use std::cell::RefCell;
use std::rc::Rc;

let shared = Rc::new(RefCell::new(vec![1, 2, 3]));
let clone = Rc::clone(&shared);

clone.borrow_mut().push(4);
shared.borrow_mut().push(5);

println!("{:?}", shared.borrow());      // [1, 2, 3, 4, 5]
```

Используйте осознанно: это «escape hatch» из правил borrow checker, и проверки переезжают в рантайм. Если можно обойтись без `RefCell` — лучше обойтись.

### Mutex<T> и RwLock<T>

Многопоточные аналоги `RefCell` и «множества читателей или один писатель» соответственно:

```rust
use std::sync::{Arc, Mutex};
use std::thread;

let counter = Arc::new(Mutex::new(0));
let handles: Vec<_> = (0..10).map(|_| {
    let c = Arc::clone(&counter);
    thread::spawn(move || {
        let mut guard = c.lock().unwrap();
        *guard += 1;
    })
}).collect();
for h in handles { h.join().unwrap(); }
println!("{}", *counter.lock().unwrap());   // 10
```

`lock()` возвращает `Result<MutexGuard<T>, _>` — `Err` если мьютекс «отравлен» (paniced поток держал лок). Гард автоматически освобождает мьютекс при дропе — RAII.

`RwLock` даёт `read()` (любое число читателей) и `write()` (один писатель, эксклюзивно). Используется, когда чтений много больше, чем записей.

### Weak<T> — слабая ссылка

Циклы из `Rc` (или `Arc`) **никогда** не освобождаются: счётчик не доходит до нуля, потому что узлы цикла держат друг друга. Это утечка памяти.

`Weak` — слабая ссылка, не увеличивающая strong count. Чтобы получить доступ — `upgrade()` -> `Option<Rc<T>>`:

```rust
use std::rc::{Rc, Weak};

let strong = Rc::new(42);
let weak: Weak<i32> = Rc::downgrade(&strong);

if let Some(s) = weak.upgrade() {
    println!("alive: {}", s);          // 42
}

drop(strong);
assert!(weak.upgrade().is_none());     // данные освобождены
```

Классический пример — дерево с обратной ссылкой на родителя:

```rust
use std::cell::RefCell;
use std::rc::{Rc, Weak};

struct Node {
    value: i32,
    parent: RefCell<Weak<Node>>,
    children: RefCell<Vec<Rc<Node>>>,
}
```

Дети — `Rc` (родитель владеет детьми). Родитель — `Weak` (ребёнок не должен продлевать жизнь родителю). Цикла нет — память освобождается корректно.

### OnceCell, OnceLock, LazyLock

С Rust 1.70+ доступны:

- `OnceCell<T>` (однопоточный) и `OnceLock<T>` (потокобезопасный) — ленивая инициализация значения **ровно один раз**.
- `LazyLock<T>` — то же, но с автоматической инициализацией при первом обращении.

```rust
use std::sync::OnceLock;

static CONFIG: OnceLock<String> = OnceLock::new();

fn config() -> &'static String {
    CONFIG.get_or_init(|| {
        std::env::var("APP_CONFIG").unwrap_or_else(|_| "default".into())
    })
}
```

Это аккуратная замена устаревшим макросам `lazy_static!` и `once_cell`-крейту в новых проектах.

### Cow — clone-on-write

`std::borrow::Cow<'a, T>` — «либо ссылка, либо владение». Полезен, когда чаще всего вы можете обойтись ссылкой, и только иногда — копировать:

```rust
use std::borrow::Cow;

fn normalize(s: &str) -> Cow<'_, str> {
    if s.contains(char::is_uppercase) {
        Cow::Owned(s.to_lowercase())     // выделяем строку только при необходимости
    } else {
        Cow::Borrowed(s)                  // иначе просто ссылка
    }
}
```

### Сводная таблица

| Тип | Где применять | Стоимость | Потокобезопасен |
|-----|---------------|-----------|------------------|
| `Box<T>` | владение в куче, рекурсия, dyn | один указатель | да |
| `Rc<T>` | shared ownership, single-thread | счётчик | нет |
| `Arc<T>` | shared ownership, multi-thread | атомарный счётчик | да |
| `Cell<T>` | runtime mutability для `Copy` | очень дёшево | нет |
| `RefCell<T>` | runtime borrow check | дёшево | нет |
| `Mutex<T>` | exclusive access, multi-thread | системный лок | да |
| `RwLock<T>` | read-many / write-one | системный лок | да |
| `Weak<T>` | избежать циклов | как `Rc`/`Arc` | да/нет |
| `OnceLock<T>` | ленивая инициализация | дёшево | да |

### Распространённые ошибки

1. **Утечка через цикл `Rc<RefCell<...>>`.** Решение: `Weak` для обратных ссылок.
2. **Паника от `borrow_mut` дважды.** Решение: ограничить scope первой `borrow_mut` через `{}`.
3. **Deadlock при `Mutex` и блокировках в неправильном порядке.** Решение: фиксированный порядок локов, либо `try_lock`.
4. **Lock guard живёт слишком долго.** Решение: брать `borrow`/`lock` в минимальном scope, дропать рано.

## Практика

### Задача 1. Box для рекурсивного списка

```rust
#[derive(Debug)]
enum List { Cons(i32, Box<List>), Nil }

fn sum(list: &List) -> i32 {
    match list {
        List::Cons(x, rest) => x + sum(rest),
        List::Nil => 0,
    }
}

fn main() {
    let l = List::Cons(1, Box::new(List::Cons(2, Box::new(List::Cons(3, Box::new(List::Nil))))));
    println!("{}", sum(&l));      // 6
}
```

### Задача 2. Trait object через Box

```rust
trait Shape {
    fn area(&self) -> f64;
}

struct Circle(f64);
struct Square(f64);

impl Shape for Circle { fn area(&self) -> f64 { std::f64::consts::PI * self.0 * self.0 } }
impl Shape for Square { fn area(&self) -> f64 { self.0 * self.0 } }

fn total(shapes: &[Box<dyn Shape>]) -> f64 {
    shapes.iter().map(|s| s.area()).sum()
}

fn main() {
    let shapes: Vec<Box<dyn Shape>> = vec![
        Box::new(Circle(1.0)),
        Box::new(Square(2.0)),
        Box::new(Circle(3.0)),
    ];
    println!("{:.3}", total(&shapes));
}
```

### Задача 3. Rc<RefCell> — общий счётчик

```rust
use std::cell::RefCell;
use std::rc::Rc;

fn main() {
    let counter = Rc::new(RefCell::new(0));
    let c1 = Rc::clone(&counter);
    let c2 = Rc::clone(&counter);

    *c1.borrow_mut() += 5;
    *c2.borrow_mut() += 10;

    println!("counter = {}", counter.borrow());     // 15
    println!("strong = {}", Rc::strong_count(&counter));   // 3
}
```

### Задача 4. Arc + Mutex — счётчик в потоках

```rust
use std::sync::{Arc, Mutex};
use std::thread;

fn main() {
    let counter = Arc::new(Mutex::new(0));
    let mut handles = vec![];
    for _ in 0..10 {
        let c = Arc::clone(&counter);
        handles.push(thread::spawn(move || {
            for _ in 0..1000 {
                *c.lock().unwrap() += 1;
            }
        }));
    }
    for h in handles { h.join().unwrap(); }
    println!("итог = {}", *counter.lock().unwrap());     // 10000
}
```

### Задача 5. RwLock для частых чтений

```rust
use std::sync::{Arc, RwLock};
use std::thread;

fn main() {
    let cfg = Arc::new(RwLock::new(String::from("v1")));
    let mut handles = vec![];

    for i in 0..4 {
        let c = Arc::clone(&cfg);
        handles.push(thread::spawn(move || {
            let val = c.read().unwrap();
            println!("читатель {i}: {}", *val);
        }));
    }

    {
        let mut w = cfg.write().unwrap();
        *w = String::from("v2");
    }

    for h in handles { h.join().unwrap(); }
}
```

### Задача 6. Дерево с Weak parent

```rust
use std::cell::RefCell;
use std::rc::{Rc, Weak};

struct Node {
    value: i32,
    parent: RefCell<Weak<Node>>,
    children: RefCell<Vec<Rc<Node>>>,
}

fn main() {
    let leaf = Rc::new(Node {
        value: 3,
        parent: RefCell::new(Weak::new()),
        children: RefCell::new(vec![]),
    });

    let branch = Rc::new(Node {
        value: 5,
        parent: RefCell::new(Weak::new()),
        children: RefCell::new(vec![Rc::clone(&leaf)]),
    });

    *leaf.parent.borrow_mut() = Rc::downgrade(&branch);

    println!("leaf parent value = {}", leaf.parent.borrow().upgrade().unwrap().value);
    println!("branch strong = {}", Rc::strong_count(&branch));
    println!("leaf strong = {}", Rc::strong_count(&leaf));
}
```

### Задача 7. OnceLock для ленивой инициализации

```rust
use std::sync::OnceLock;

fn primes_under_100() -> &'static Vec<u32> {
    static CACHE: OnceLock<Vec<u32>> = OnceLock::new();
    CACHE.get_or_init(|| {
        (2..100).filter(|&n| {
            (2..n).all(|i| n % i != 0)
        }).collect()
    })
}

fn main() {
    println!("{:?}", primes_under_100());
    // Второй вызов — данные не пересчитываются
    println!("len = {}", primes_under_100().len());
}
```

### Задача 8. Cow — выделять только при необходимости

```rust
use std::borrow::Cow;

fn lower(s: &str) -> Cow<'_, str> {
    if s.chars().any(|c| c.is_uppercase()) {
        Cow::Owned(s.to_lowercase())
    } else {
        Cow::Borrowed(s)
    }
}

fn main() {
    let a = lower("hello");        // Borrowed — без аллокации
    let b = lower("Hello World");  // Owned — выделили строку
    println!("{a:?}");
    println!("{b:?}");
}
```

## Тест

**1.** Что выведет?

```rust
use std::rc::Rc;
let a = Rc::new(5);
let b = Rc::clone(&a);
let c = a.clone();
println!("{}", Rc::strong_count(&a));
```

**2.** Что произойдёт?

```rust
use std::cell::RefCell;
let x = RefCell::new(0);
let a = x.borrow_mut();
let b = x.borrow_mut();
```

**3.** Когда выбрать `Rc`, а когда `Arc`?

**4.** Зачем нужен `Weak<T>`? Приведите пример сценария, где без него возникнет утечка.

**5.** В чём разница между `Box<T>` и `&T`? Зачем нужен `Box`, если есть ссылки?

**6.** Что напечатает программа?

```rust
use std::cell::Cell;
let c = Cell::new(5);
c.set(c.get() + 1);
c.set(c.get() + 1);
println!("{}", c.get());
```

**7.** В чём разница между `Mutex<T>` и `RwLock<T>`? Когда выбрать `RwLock`?

**8.** Что не так с этим кодом?

```rust
use std::cell::RefCell;
fn main() {
    let v = RefCell::new(vec![1, 2, 3]);
    let r = v.borrow();
    let mut m = v.borrow_mut();
    m.push(4);
    println!("{:?}", r);
}
```

**9.** Зачем нужен `Cow<'a, str>` и какой главный выигрыш он даёт?

**10.** Скомпилируется ли?

```rust
use std::rc::Rc;
use std::thread;
fn main() {
    let data = Rc::new(vec![1, 2, 3]);
    let d = Rc::clone(&data);
    thread::spawn(move || {
        println!("{:?}", d);
    }).join().unwrap();
}
```

---

### Ответы

1. `3`. `Rc::clone(&a)` и `a.clone()` — одно и то же: оба увеличивают strong count. Идиоматично писать `Rc::clone(&a)`, чтобы было видно: это не глубокое копирование, а лишь увеличение счётчика.

2. Паника во время выполнения: `already borrowed: BorrowMutError`. `RefCell` проверяет правила в рантайме: две `borrow_mut` одновременно запрещены. Чинится ограничением scope первой `borrow_mut`: `{ let a = x.borrow_mut(); ... }`.

3. `Rc` — однопоточный код, дешевле (не атомарные операции). `Arc` — когда данные шарятся между потоками. Компилятор не даст случайно использовать `Rc` многопоточно: он не реализует `Send`/`Sync`.

4. `Weak` нужен, чтобы избежать циклических ссылок, которые `Rc` не освободит. Пример — дерево, где узлы хранят и `children: Vec<Rc<Node>>`, и `parent: Rc<Node>`. Тогда родитель и ребёнок держат друг друга, strong count никогда не доходит до нуля, память утекает. Решение: `parent: Weak<Node>` — слабая ссылка, не увеличивающая strong count.

5. `&T` — заимствование, не владеет данными, ограничено lifetime источника. `Box<T>` — владеет данными в куче, может пережить любую функцию (если возвращён). `Box` нужен для рекурсивных типов (компилятор не может вычислить размер бесконечного типа), для trait objects (`Box<dyn Trait>`), и когда хочется передать владение тяжёлым значением, не копируя его на стек.

6. `7`. `Cell::set` заменяет значение целиком. Каждый `c.get() + 1` копирует текущее, прибавляет 1, кладёт обратно: 5 → 6 → 7.

7. `Mutex` даёт **эксклюзивный** доступ — один поток внутри. `RwLock` даёт **множество читателей или одного писателя**: `read()` могут держать многие потоки одновременно, `write()` — только один и эксклюзивно. Выбирают `RwLock`, когда чтений значительно больше, чем записей; в противном случае `Mutex` обычно быстрее (меньше накладных).

8. Паника в рантайме: уже есть иммутабельный `borrow` в `r`, и нельзя одновременно взять `borrow_mut`. Это нарушение правил заимствования, которое `RefCell` ловит в рантайме.

9. `Cow<'a, str>` («clone on write») — это enum «либо ссылка, либо владение». Главный выигрыш: вы избегаете аллокации в общем случае, выделяя строку только тогда, когда действительно нужно изменение. Полезен в API, где трансформация чаще всего «нет-операция»: парсинг конфигов, нормализация строк, обработка путей.

10. Не скомпилируется. `Rc<T>` не реализует `Send`, поэтому передать `Rc<Vec<i32>>` в `thread::spawn` нельзя — ошибка `Rc<...> cannot be sent between threads safely`. Чинится заменой `Rc` на `Arc`.
# Урок 12. Умные указатели и интериор-мутабельность

## Теория

«Умный указатель» — структура, которая ведёт себя как указатель, но добавляет логику владения, подсчёта ссылок или динамической диспетчеризации.

### `Box<T>` — владение в куче

Самый простой умный указатель. Размещает значение в куче, на стеке хранит указатель.

```rust
let b = Box::new(5);
println!("{}", *b); // разыменование через Deref
```

Когда нужен:
- Рекурсивные типы (`enum List { Cons(i32, Box<List>), Nil }`).
- Большие значения, которые не хотим копировать на стеке.
- Trait objects: `Box<dyn Trait>`.
- Перенос владения между потоками (вместе с `Send`).

### `Rc<T>` — счётчик ссылок (однопоточный)

Несколько владельцев одного значения. Когда последний `Rc` дропается — значение освобождается.

```rust
use std::rc::Rc;

let a = Rc::new(String::from("hi"));
let b = Rc::clone(&a); // не глубокое копирование, +1 к счётчику
println!("refs = {}", Rc::strong_count(&a)); // 2
```

`Rc` **не** реализует `Send` — нельзя передавать между потоками. Для многопоточного аналога — `Arc`.

### `Arc<T>` — атомарный счётчик ссылок

То же самое, но потокобезопасно. Чуть медленнее из-за атомиков.

```rust
use std::sync::Arc;
use std::thread;

let data = Arc::new(vec![1, 2, 3]);
let handles: Vec<_> = (0..3).map(|i| {
    let d = Arc::clone(&data);
    thread::spawn(move || println!("{i}: {:?}", d))
}).collect();
for h in handles { h.join().unwrap(); }
```

### Интериор-мутабельность

Обычно в Rust: либо одна `&mut T`, либо много `&T`. Иногда нужно изменять данные через общую ссылку — например, кэш внутри `Rc`. Для этого есть **interior mutability**.

- **`Cell<T>`** — для `Copy`-типов. Замена значения целиком, без ссылок. Очень дешёвый.
- **`RefCell<T>`** — runtime borrow checker. `borrow()` / `borrow_mut()` проверяют правила во время выполнения. При нарушении — паника.
- **`Mutex<T>` / `RwLock<T>`** — то же самое, но потокобезопасно. Используется с `Arc`.

```rust
use std::cell::RefCell;

let counter = RefCell::new(0);
*counter.borrow_mut() += 1;
*counter.borrow_mut() += 1;
println!("{}", counter.borrow()); // 2
```

### Типичная пара: `Rc<RefCell<T>>`

Когда нужен общий доступ + мутабельность в однопоточном коде. Это «escape hatch» из правил borrow checker — используйте осознанно.

```rust
use std::rc::Rc;
use std::cell::RefCell;

let shared = Rc::new(RefCell::new(vec![1, 2, 3]));
let clone = Rc::clone(&shared);
clone.borrow_mut().push(4);
println!("{:?}", shared.borrow()); // [1, 2, 3, 4]
```

Многопоточный аналог: `Arc<Mutex<T>>`.

### `Weak<T>` — слабая ссылка

Циклы из `Rc` не освобождаются никогда (счётчик не дойдёт до нуля). `Weak` не увеличивает strong count, нужно `upgrade()` для доступа.

```rust
use std::rc::{Rc, Weak};

let strong = Rc::new(42);
let weak: Weak<i32> = Rc::downgrade(&strong);
if let Some(s) = weak.upgrade() {
    println!("alive: {}", s);
}
```

Классический пример: дерево с `parent: Weak<Node>` и `children: Vec<Rc<Node>>`.

### `Deref` и `Drop`

Умные указатели работают за счёт двух трейтов:
- `Deref` — позволяет `*p` и автодереф (`p.method()` работает, как если бы `p` был `T`).
- `Drop` — кастомная логика при освобождении (RAII).

## Практика

Граф друзей: каждый узел знает соседей. Используем `Rc<RefCell<...>>`.

```rust
use std::cell::RefCell;
use std::rc::Rc;

type Node = Rc<RefCell<Person>>;

struct Person {
    name: String,
    friends: Vec<Node>,
}

fn new_person(name: &str) -> Node {
    Rc::new(RefCell::new(Person {
        name: name.to_string(),
        friends: vec![],
    }))
}

fn befriend(a: &Node, b: &Node) {
    a.borrow_mut().friends.push(Rc::clone(b));
    b.borrow_mut().friends.push(Rc::clone(a));
}

fn main() {
    let alice = new_person("Alice");
    let bob   = new_person("Bob");
    let carol = new_person("Carol");

    befriend(&alice, &bob);
    befriend(&bob, &carol);

    let names: Vec<_> = bob.borrow().friends.iter()
        .map(|f| f.borrow().name.clone())
        .collect();
    println!("друзья Bob: {:?}", names);
}
```

## Тест

**1.** Что выведет?
```rust
use std::rc::Rc;
let a = Rc::new(5);
let b = Rc::clone(&a);
let c = a.clone();
println!("{}", Rc::strong_count(&a));
```

**2.** Что произойдёт?
```rust
use std::cell::RefCell;
let x = RefCell::new(0);
let a = x.borrow_mut();
let b = x.borrow_mut();
```

**3.** Когда выбрать `Rc`, а когда `Arc`?

**4.** Зачем нужен `Weak<T>`? Приведите пример.

**5.** В чём разница между `Box<T>` и `&T`? Зачем нужен `Box`, если есть ссылки?

---

### Ответы

**1.** `3`. `Rc::clone` и `.clone()` на `Rc` — это одно и то же, оба увеличивают счётчик. Идиоматично писать `Rc::clone(&a)`, чтобы было видно: это не глубокое копирование.

**2.** Паника во время выполнения: `already borrowed: BorrowMutError`. `RefCell` проверяет правила во время выполнения, а не компиляции — две `borrow_mut` одновременно запрещены.

**3.** `Rc` — однопоточный код, дешевле (не атомарные операции). `Arc` — когда данные шарятся между потоками. Компилятор не даст случайно использовать `Rc` многопоточно: он не реализует `Send`/`Sync`.

**4.** Чтобы избежать циклических ссылок, которые `Rc` не освободит. Пример: дерево, где у узлов есть и `children` (`Rc`), и `parent` (`Weak`). Иначе `parent ↔ child` создаст цикл, и память утечёт.

**5.** `&T` — это заимствование, не владеет данными, ограничено lifetime источника. `Box<T>` — владеет данными в куче, может пережить любую функцию (если возвращён). `Box` нужен для рекурсивных типов (компилятор не может вычислить размер бесконечного типа), для trait objects (`Box<dyn Trait>`), и когда хочется передать владение тяжёлым значением, не копируя его.
