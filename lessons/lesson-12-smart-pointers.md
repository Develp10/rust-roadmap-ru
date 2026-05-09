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
