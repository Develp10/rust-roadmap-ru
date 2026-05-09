# Урок 24. Паттерны проектирования в Rust

## Теория

### Особенности Rust для паттернов

В Rust многие классические GoF-паттерны выглядят иначе из-за owner­ship, отсутствия наследования и предпочтения композиции:

- наследование заменяется трейтами и композицией
- многие паттерны реализуются через enum и сопоставление с образцом
- интерфейсы — `trait` (статическая или динамическая диспетчеризация)
- общее владение — `Rc<RefCell<T>>` или `Arc<Mutex<T>>`

### Newtype

Защита от перепутанных типов и расширение чужих типов:

```rust
struct UserId(u64);
struct OrderId(u64);

fn cancel(id: OrderId) { /* ... */ }
```

### Builder

```rust
#[derive(Default)]
pub struct Request { url: String, method: String, body: Option<String> }

#[derive(Default)]
pub struct RequestBuilder { url: String, method: String, body: Option<String> }

impl RequestBuilder {
    pub fn url(mut self, u: impl Into<String>) -> Self { self.url = u.into(); self }
    pub fn method(mut self, m: impl Into<String>) -> Self { self.method = m.into(); self }
    pub fn body(mut self, b: impl Into<String>) -> Self { self.body = Some(b.into()); self }
    pub fn build(self) -> Request {
        Request { url: self.url, method: self.method, body: self.body }
    }
}
```

Альтернатива — крейт `derive_builder`.

### Typestate

Состояния как разные типы; недопустимые переходы — ошибка компиляции.

```rust
struct Closed;
struct Open;

struct File<S> { path: String, _s: std::marker::PhantomData<S> }

impl File<Closed> {
    fn open(self) -> File<Open> { File { path: self.path, _s: std::marker::PhantomData } }
}
impl File<Open> {
    fn read(&self) -> String { format!("data from {}", self.path) }
    fn close(self) -> File<Closed> { File { path: self.path, _s: std::marker::PhantomData } }
}
```

### Strategy через трейт

```rust
trait Compress { fn compress(&self, data: &[u8]) -> Vec<u8>; }
struct Gzip; struct Zstd;
impl Compress for Gzip { fn compress(&self, d: &[u8]) -> Vec<u8> { d.into() } }
impl Compress for Zstd { fn compress(&self, d: &[u8]) -> Vec<u8> { d.into() } }

fn pack<C: Compress>(c: &C, data: &[u8]) -> Vec<u8> { c.compress(data) }
```

### Observer / Publisher-subscriber

```rust
use std::sync::{Arc, Mutex};

trait Observer: Send + Sync { fn on(&self, event: &str); }

#[derive(Default)]
struct EventBus { subs: Mutex<Vec<Arc<dyn Observer>>> }
impl EventBus {
    fn subscribe(&self, o: Arc<dyn Observer>) { self.subs.lock().unwrap().push(o); }
    fn publish(&self, e: &str) {
        for s in self.subs.lock().unwrap().iter() { s.on(e); }
    }
}
```

### State

```rust
enum Light { Red, Yellow, Green }
impl Light {
    fn next(self) -> Self {
        match self { Light::Red => Light::Green, Light::Green => Light::Yellow, Light::Yellow => Light::Red }
    }
}
```

### Visitor через enum + match

```rust
enum Expr { Num(i64), Add(Box<Expr>, Box<Expr>), Mul(Box<Expr>, Box<Expr>) }

fn eval(e: &Expr) -> i64 {
    match e {
        Expr::Num(n) => *n,
        Expr::Add(a, b) => eval(a) + eval(b),
        Expr::Mul(a, b) => eval(a) * eval(b),
    }
}
```

### RAII / Drop

```rust
struct Lock<'a, T> { _g: std::sync::MutexGuard<'a, T> }
// освобождение происходит в Drop автоматически
```

### Interior mutability

`Cell`, `RefCell`, `Mutex`, `RwLock`, `OnceCell` — изменение состояния через `&self`.

### Антипаттерны

- `Rc<RefCell<...>>`-цепочки вместо нормальных API
- `unwrap()` в продакшен-коде
- избыточный `Box<dyn Trait>` там, где достаточно generic
- клонирование строк, чтобы обойти borrow checker
- `Arc<Mutex<HashMap>>` без необходимости — рассмотреть `DashMap`

## Практика

### 1. Newtype-обёртка

```rust
pub struct Email(String);
impl Email {
    pub fn parse(s: impl Into<String>) -> Result<Self, &'static str> {
        let s = s.into();
        if s.contains('@') { Ok(Self(s)) } else { Err("invalid email") }
    }
    pub fn as_str(&self) -> &str { &self.0 }
}
```

### 2. Builder для HTTP-клиента

```rust
let req = RequestBuilder::default()
    .url("https://api.io/v1")
    .method("POST")
    .body("{}")
    .build();
```

### 3. Typestate-машина TCP-соединения

```rust
struct Disconnected;
struct Connected;

struct Conn<S> { addr: String, _s: std::marker::PhantomData<S> }
impl Conn<Disconnected> {
    fn new(a: impl Into<String>) -> Self { Self { addr: a.into(), _s: Default::default() } }
    fn connect(self) -> Conn<Connected> { Conn { addr: self.addr, _s: Default::default() } }
}
impl Conn<Connected> {
    fn send(&self, _: &[u8]) {}
    fn close(self) -> Conn<Disconnected> { Conn { addr: self.addr, _s: Default::default() } }
}
```

### 4. Pipeline через итераторы

```rust
fn process(input: &[i32]) -> Vec<i32> {
    input.iter().filter(|x| **x > 0).map(|x| x * 2).take(10).collect()
}
```

### 5. Type-erased plugin

```rust
trait Plugin: Send + Sync {
    fn name(&self) -> &str;
    fn run(&self, input: &str) -> String;
}

struct Registry { plugins: Vec<Box<dyn Plugin>> }
impl Registry {
    fn add(&mut self, p: Box<dyn Plugin>) { self.plugins.push(p); }
    fn run_all(&self, input: &str) -> Vec<String> {
        self.plugins.iter().map(|p| p.run(input)).collect()
    }
}
```

### 6. State-машина enum

```rust
enum Order { New, Paid { ts: u64 }, Shipped { tracking: String }, Done }
fn step(o: Order) -> Order {
    match o {
        Order::New => Order::Paid { ts: 0 },
        Order::Paid { .. } => Order::Shipped { tracking: "T1".into() },
        Order::Shipped { .. } => Order::Done,
        Order::Done => Order::Done,
    }
}
```

### 7. Команда (Command)

```rust
trait Command { fn execute(&mut self); }
struct Print(String);
impl Command for Print { fn execute(&mut self) { println!("{}", self.0); } }

let mut q: Vec<Box<dyn Command>> = vec![Box::new(Print("hi".into()))];
while let Some(mut c) = q.pop() { c.execute(); }
```

## Тест

1. Чем заменяется наследование в Rust?
2. Что такое newtype?
3. Зачем нужен паттерн typestate?
4. Какой крейт автоматизирует builder?
5. Чем enum + match заменяет Visitor?
6. Что такое RAII в контексте Rust?
7. Какой тип использовать для interior mutability в однопоточной программе?
8. Почему `Rc<RefCell<...>>`-цепочки часто антипаттерн?
9. Когда лучше брать `Box<dyn Trait>` вместо generic?
10. Какой паттерн обеспечивает безопасный API через систему типов?

---

### Ответы

1. Трейтами и композицией.
2. Структура-обёртка над одним типом для повышения типобезопасности и расширения чужого типа.
3. Закодировать состояние объекта в типе и сделать недопустимые переходы ошибкой компиляции.
4. `derive_builder`.
5. Перечисление вариантов и сопоставление по образцу заменяют двойную диспетчеризацию.
6. Освобождение ресурсов через `Drop` — `MutexGuard`, `File`, `TcpStream` закрываются автоматически.
7. `RefCell` (или `Cell` для `Copy`-типов).
8. Сложно отслеживать владение, легко получить runtime-panic при пересечении borrow’ов, ухудшается читаемость.
9. Когда нужна гетерогенная коллекция или граница ABI/динамический выбор реализации в рантайме.
10. Typestate.
