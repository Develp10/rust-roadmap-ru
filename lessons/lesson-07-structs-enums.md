# Урок 7. Структуры и перечисления

Структуры (`struct`) и перечисления (`enum`) — это основные способы определять собственные типы в Rust. Структуры — про «и», перечисления — про «или». Освоить идиомы их использования — половина успеха в моделировании предметной области.

## Теория

### Обычные структуры

```rust
struct User {
    name: String,
    email: String,
    active: bool,
    sign_in_count: u32,
}

let u = User {
    name: String::from("Alice"),
    email: String::from("a@b"),
    active: true,
    sign_in_count: 1,
};
```

Особенности:

- Поля иммутабельны вместе со структурой: чтобы менять, нужен `let mut u`.
- Имена полей и переменных можно сокращать: `User { name, email, active, sign_in_count: 1 }` (если переменные совпадают по имени).
- Шаблон обновления: `User { name: new_name, ..u }` — забирает остальные поля у `u` (потенциально с move).

### Кортежные структуры

```rust
struct Point(i32, i32);
struct Color(u8, u8, u8);

let p = Point(3, 4);
let c = Color(255, 128, 0);
println!("{} {} {} {}", p.0, p.1, c.0, c.1);
```

Удобны для маленьких типов и new-type паттерна:

```rust
struct UserId(u64);
struct OrderId(u64);
```

Так вы получите компилятор-страховку: `UserId` и `OrderId` — разные типы, перепутать невозможно, хотя оба внутри `u64`.

### Unit-структуры

```rust
struct Marker;
```

Без полей. Используются как маркерные типы для трейтов, для type-state pattern и для документирования.

### Методы и impl

```rust
impl User {
    // ассоциированная функция (нет &self) — работает как «конструктор»
    fn new(name: String, email: String) -> Self {
        Self { name, email, active: true, sign_in_count: 0 }
    }

    // метод по ссылке
    fn greet(&self) -> String {
        format!("Привет, {}!", self.name)
    }

    // метод с мутабельной ссылкой
    fn deactivate(&mut self) {
        self.active = false;
    }

    // метод по значению — забирает self
    fn into_email(self) -> String {
        self.email
    }
}

let mut u = User::new("Аня".into(), "a@b".into());
println!("{}", u.greet());
u.deactivate();
let email = u.into_email();   // u недоступен дальше
```

Запомните три формы:

- `&self` — заимствование, можно вызывать сколько угодно раз.
- `&mut self` — мутабельное заимствование.
- `self` — забирает владение (typed builders, конверсии).

`Self` (с большой буквы) — алиас типа, в котором мы внутри `impl`.

### Несколько impl-блоков

Можно разносить методы по разным `impl`-блокам — это часто удобно для больших типов или условных реализаций (`impl<T: Clone> Foo<T>`).

### Производные трейты

```rust
#[derive(Debug, Clone, PartialEq, Eq, Hash)]
struct Point { x: i32, y: i32 }
```

`#[derive(...)]` автоматически реализует трейты. Самые ходовые:

- `Debug` — для `{:?}`-форматирования (всегда добавляйте);
- `Clone` — глубокая копия;
- `Copy` — побитовое копирование (только если все поля `Copy`);
- `PartialEq`, `Eq` — сравнение;
- `Hash` — для `HashMap`;
- `Default` — `Type::default()` со «значениями по умолчанию»;
- `PartialOrd`, `Ord` — сортировка.

### Перечисления (enum)

Enum в Rust — это **сумма-тип**: значение всегда находится в одном из вариантов.

```rust
enum Shape {
    Circle(f64),                    // вариант с данными (кортежный)
    Rectangle { w: f64, h: f64 },   // вариант со структурными полями
    None,                            // вариант без данных (unit)
}
```

Самые важные стандартные enum'ы:

```rust
enum Option<T> { Some(T), None }
enum Result<T, E> { Ok(T), Err(E) }
```

`Option<T>` заменяет «null» — нельзя забыть проверить «есть ли значение». `Result<T, E>` — стандартный способ возвращать ошибки, без исключений.

### Pattern matching по enum

```rust
fn area(s: &Shape) -> f64 {
    match s {
        Shape::Circle(r) => std::f64::consts::PI * r * r,
        Shape::Rectangle { w, h } => w * h,
        Shape::None => 0.0,
    }
}
```

Компилятор требует, чтобы `match` покрывал все варианты — забыть обработать `Rectangle` нельзя.

### Методы у enum

```rust
impl Shape {
    fn area(&self) -> f64 {
        match self {
            Shape::Circle(r) => std::f64::consts::PI * r * r,
            Shape::Rectangle { w, h } => w * h,
            Shape::None => 0.0,
        }
    }
    fn is_visible(&self) -> bool {
        !matches!(self, Shape::None)
    }
}
```

`matches!(value, pattern)` — макрос-предикат, удобен для коротких проверок.

### Размер enum и niche-оптимизация

Размер `enum` равен `размер тега + max(размер варианта)` с учётом выравнивания. Но компилятор делает **niche-оптимизацию**: если в варианте есть «недостижимые» битовые комбинации (например, `&T` не может быть нулевым, `bool` использует только 0 и 1), он использует их для тега.

```rust
use std::mem::size_of;
println!("{}", size_of::<Option<u8>>());        // 2 байта (1 для тега + 1 для данных)
println!("{}", size_of::<Option<&u8>>());       // 8 байт — None кодируется нулевым указателем
println!("{}", size_of::<Option<Box<u8>>>());   // 8 байт — то же самое
println!("{}", size_of::<Option<bool>>());      // 1 байт
```

Это объясняет, почему `Option<&T>` в Rust не толще обычного `&T`.

### New-type паттерн

```rust
struct Meters(f64);
struct Seconds(f64);

fn speed(m: Meters, s: Seconds) -> f64 {
    m.0 / s.0
}
```

Запутаться невозможно: `speed(Seconds(10.0), Meters(100.0))` — ошибка компиляции. То же часто делают для типобезопасных идентификаторов (`UserId`, `OrderId`).

### Builder-паттерн

Когда у структуры много опциональных полей:

```rust
#[derive(Default)]
struct Config { host: String, port: u16, tls: bool, retries: u32 }

#[derive(Default)]
struct ConfigBuilder { c: Config }

impl ConfigBuilder {
    fn new() -> Self { Self::default() }
    fn host(mut self, h: impl Into<String>) -> Self { self.c.host = h.into(); self }
    fn port(mut self, p: u16) -> Self { self.c.port = p; self }
    fn tls(mut self, t: bool) -> Self { self.c.tls = t; self }
    fn build(self) -> Config { self.c }
}

let cfg = ConfigBuilder::new()
    .host("localhost")
    .port(443)
    .tls(true)
    .build();
```

## Практика

### Задача 1. Прямоугольник

```rust
#[derive(Debug)]
struct Rectangle { w: u32, h: u32 }

impl Rectangle {
    fn square(side: u32) -> Self { Self { w: side, h: side } }
    fn area(&self) -> u32 { self.w * self.h }
    fn can_hold(&self, other: &Rectangle) -> bool {
        self.w >= other.w && self.h >= other.h
    }
}

fn main() {
    let big = Rectangle { w: 30, h: 50 };
    let small = Rectangle::square(10);
    println!("{} {}", big.area(), small.area());
    println!("вместит: {}", big.can_hold(&small));
}
```

### Задача 2. New-type для безопасности

```rust
struct Celsius(f64);
struct Fahrenheit(f64);

impl Celsius {
    fn to_fahrenheit(&self) -> Fahrenheit {
        Fahrenheit(self.0 * 9.0 / 5.0 + 32.0)
    }
}

fn main() {
    let t = Celsius(25.0);
    let f = t.to_fahrenheit();
    println!("{} F", f.0);
}
```

### Задача 3. Enum для фигур

```rust
#[derive(Debug)]
enum Shape {
    Circle(f64),
    Rect(f64, f64),
    Triangle { a: f64, b: f64, c: f64 },
}

impl Shape {
    fn area(&self) -> f64 {
        match self {
            Shape::Circle(r) => std::f64::consts::PI * r * r,
            Shape::Rect(w, h) => w * h,
            Shape::Triangle { a, b, c } => {
                let s = (a + b + c) / 2.0;
                (s * (s - a) * (s - b) * (s - c)).sqrt()  // формула Герона
            }
        }
    }
}

fn main() {
    let shapes = [
        Shape::Circle(1.0),
        Shape::Rect(2.0, 3.0),
        Shape::Triangle { a: 3.0, b: 4.0, c: 5.0 },
    ];
    for s in &shapes { println!("{s:?} -> {:.3}", s.area()); }
}
```

### Задача 4. Стейт-машина светофора

```rust
#[derive(Debug, Clone, Copy)]
enum Light { Red, Yellow, Green }

impl Light {
    fn next(self) -> Self {
        match self {
            Light::Red => Light::Green,
            Light::Green => Light::Yellow,
            Light::Yellow => Light::Red,
        }
    }
}

fn main() {
    let mut l = Light::Red;
    for _ in 0..6 {
        println!("{l:?}");
        l = l.next();
    }
}
```

### Задача 5. Опциональные поля

```rust
#[derive(Debug)]
struct User {
    name: String,
    email: Option<String>,
    phone: Option<String>,
}

impl User {
    fn contacts(&self) -> Vec<String> {
        let mut out = Vec::new();
        if let Some(e) = &self.email { out.push(format!("email: {e}")); }
        if let Some(p) = &self.phone { out.push(format!("phone: {p}")); }
        out
    }
}

fn main() {
    let u = User { name: "Аня".into(), email: Some("a@b".into()), phone: None };
    for c in u.contacts() { println!("{c}"); }
}
```

### Задача 6. Калькулятор как enum

```rust
enum Op { Add(f64, f64), Sub(f64, f64), Mul(f64, f64), Div(f64, f64) }

impl Op {
    fn eval(&self) -> Option<f64> {
        match self {
            Op::Add(a, b) => Some(a + b),
            Op::Sub(a, b) => Some(a - b),
            Op::Mul(a, b) => Some(a * b),
            Op::Div(_, b) if *b == 0.0 => None,
            Op::Div(a, b) => Some(a / b),
        }
    }
}

fn main() {
    for op in [Op::Add(2.0, 3.0), Op::Div(10.0, 0.0), Op::Mul(4.0, 5.0)] {
        println!("{:?}", op.eval());
    }
}
```

### Задача 7. Builder для запроса

```rust
#[derive(Debug, Default)]
struct Query {
    table: String,
    limit: Option<u32>,
    offset: Option<u32>,
    order: Option<String>,
}

#[derive(Default)]
struct QueryBuilder { q: Query }

impl QueryBuilder {
    fn from(table: &str) -> Self {
        Self { q: Query { table: table.into(), ..Default::default() } }
    }
    fn limit(mut self, n: u32) -> Self { self.q.limit = Some(n); self }
    fn offset(mut self, n: u32) -> Self { self.q.offset = Some(n); self }
    fn order_by(mut self, c: &str) -> Self { self.q.order = Some(c.into()); self }
    fn build(self) -> Query { self.q }
}

fn main() {
    let q = QueryBuilder::from("users")
        .limit(50)
        .offset(0)
        .order_by("created_at DESC")
        .build();
    println!("{q:?}");
}
```

## Тест

**1.** Сколько байт занимает `Option<&u8>` на 64-битной платформе и почему?

**2.** Что выведет?

```rust
struct P { x: i32 }
impl P { fn x(&self) -> i32 { self.x + 1 } }
fn main() {
    let p = P { x: 5 };
    println!("{}", p.x());
}
```

**3.** В чём разница между `fn new(name: String) -> Self` и `fn new(name: String) -> User` в impl-блоке?

**4.** Скомпилируется ли?

```rust
enum E { A, B(i32) }
fn main() {
    let e = E::A;
    match e {
        E::A => println!("a"),
        E::B(_) => println!("b"),
        E::A => println!("a2"),
    }
}
```

**5.** Зачем нужны unit-структуры?

**6.** Что напечатает программа?

```rust
#[derive(Debug)]
struct V(i32, i32);
fn main() {
    let v = V(1, 2);
    let V(a, b) = v;
    println!("{a} {b}");
}
```

**7.** В чём разница между `#[derive(Copy, Clone)]` и просто `#[derive(Clone)]`?

**8.** Что выведет код?

```rust
struct Counter { n: u32 }
impl Counter {
    fn new() -> Self { Self { n: 0 } }
    fn inc(&mut self) -> u32 { self.n += 1; self.n }
}
fn main() {
    let mut c = Counter::new();
    println!("{} {} {}", c.inc(), c.inc(), c.inc());
}
```

**9.** Что такое new-type паттерн и какую проблему он решает?

**10.** Скомпилируется ли?

```rust
struct Wrapper(String);
fn show(w: Wrapper) {
    println!("{}", w.0);
    println!("{}", w.0);
}
```

---

### Ответы

1. 8 байт — размер обычного указателя. Это niche-оптимизация: ссылка `&u8` никогда не бывает нулевой, поэтому `None` кодируется нулевым указателем без отдельного тега. Поэтому `Option<&T>` ничем не толще `&T`.

2. `6`. Метод `x()` возвращает `self.x + 1`. Поле и метод могут иметь одно имя — синтаксически они различаются скобками: `p.x` (поле), `p.x()` (метод).

3. Семантически ничего: `Self` — алиас текущего типа. Но `Self` устойчив к переименованию структуры и читается лучше; гайд стиля Rust рекомендует `Self`.

4. Не скомпилируется. Компилятор выдаст ошибку **unreachable pattern** на втором `E::A` (плечо недостижимо, потому что первая ветвь его уже поймала). Это лента предупреждений с включённым `deny` — ошибка.

5. Маркерные типы для типобезопасности (`struct Meters; struct Seconds;`), реализация трейтов на «пустой» тип, type-state pattern, а также как заглушки в дженериках, когда нужен тип, но не нужны данные.

6. `1 2`. Деструктуризация кортежной структуры разбирает `v` на `a = 1` и `b = 2`.

7. `Clone` — это «дорогая глубокая копия» (метод `.clone()`). `Copy` — это маркер, что копирование безопасно делать побитово при простом присваивании. `Copy` требует `Clone` (поэтому пишут оба). Тип может быть `Copy` только если все его поля `Copy` — например, `String` никогда не `Copy`.

8. `1 2 3`. `inc` берёт `&mut self`, увеличивает `n` и возвращает новое значение. Три вызова дают 1, 2, 3. Заметьте, что компилятор разрешает три `&mut` подряд, потому что они не пересекаются во времени — каждая ссылка живёт только на время вызова метода (reborrow).

9. New-type — это структура-обёртка вокруг существующего типа: `struct UserId(u64)`. Решаемые проблемы: типобезопасность (нельзя перепутать `UserId` и `OrderId`, хотя оба `u64`), возможность реализовать на обёртке свои трейты (orphan rule запрещает реализовывать чужой трейт для чужого типа), документирование смысла.

10. Не скомпилируется. `w.0` пытается переместить `String` из структуры дважды; но `String` не `Copy`. После первого `println!` поле `w.0` уже перемещено... На самом деле `println!("{}", w.0)` берёт `&w.0` через автореференцию для `Display`, поэтому компилятор как раз пропустит этот код. Двойной `println!("{}", w.0)` через `&` валиден. Если бы было `let s = w.0; let s = w.0;` — тогда был бы move-ошибка.
# Урок 7. Структуры и перечисления

## Теория

### Структуры

```rust
struct User {
    name: String,
    email: String,
    active: bool,
    sign_in_count: u32,
}

let u = User {
    name: String::from("Alice"),
    email: String::from("a@b"),
    active: true,
    sign_in_count: 1,
};
```

Поля иммутабельны вместе со структурой: чтобы менять, нужен `let mut u`.

Кортежные структуры: `struct Point(i32, i32);`, доступ `p.0`, `p.1`.

Unit-структуры: `struct Marker;` — без полей, для маркерных трейтов.

Шаблон обновления: `User { name: new, ..u }`.

### impl и методы

```rust
impl User {
    fn new(name: String, email: String) -> Self {
        Self { name, email, active: true, sign_in_count: 0 }
    }
    fn greet(&self) -> String {
        format!("Привет, {}!", self.name)
    }
    fn deactivate(&mut self) {
        self.active = false;
    }
}
```

`&self` — заимствование, `&mut self` — изменяющее, `self` — забирает владение.

### Перечисления (enum)

```rust
enum Shape {
    Circle(f64),
    Rectangle { w: f64, h: f64 },
    None,
}
```

`Option<T>` и `Result<T, E>` — самые важные стандартные enum'ы.

### Tagged unions vs структуры

Enum в Rust — sum-тип: одно из. Размер enum равен `размер тега + max(размер варианта)` с учётом выравнивания.

## Практика

```rust
#[derive(Debug)]
enum Shape { Circle(f64), Rect(f64, f64) }

impl Shape {
    fn area(&self) -> f64 {
        match self {
            Shape::Circle(r) => std::f64::consts::PI * r * r,
            Shape::Rect(w, h) => w * h,
        }
    }
}

fn main() {
    let shapes = [Shape::Circle(1.0), Shape::Rect(2.0, 3.0)];
    for s in &shapes { println!("{s:?} -> {}", s.area()); }
}
```

## Тест

**1.** Сколько байт занимает `Option<&u8>` на 64-битной платформе и почему?

**2.** Что выведет?
```rust
struct P { x: i32 }
impl P { fn x(&self) -> i32 { self.x + 1 } }
fn main() {
    let p = P { x: 5 };
    println!("{}", p.x());
}
```

**3.** В чём разница между `fn new(name: String) -> Self` и `fn new(name: String) -> User` в impl-блоке?

**4.** Скомпилируется ли?
```rust
enum E { A, B(i32) }
fn main() {
    let e = E::A;
    match e {
        E::A => println!("a"),
        E::B(_) => println!("b"),
        E::A => println!("a2"),
    }
}
```

**5.** Зачем нужны unit-структуры?

---

### Ответы

1. 8 байт. Это niche-оптимизация: `&u8` никогда не бывает нулевой ссылкой, поэтому состояние `None` кодируется нулём указателя без дополнительного тега.

2. `6`. Метод `x()` возвращает `self.x + 1`. Поле и метод могут иметь одно имя — синтаксически различаются скобками.

3. Технически ничего, `Self` — алиас типа реализации. Но `Self` устойчив к переименованию типа и читается лучше; стандарт стиля рекомендует `Self`.

4. Не скомпилируется: компилятор выдаст ошибку **unreachable pattern** на втором `E::A`, плюс предупреждение о повторе. Технически плечо unreachable.

5. Маркерные типы для типобезопасности: `struct Meters; struct Seconds;` — невозможно случайно сложить. Также для реализации трейтов на «пустой» тип, например при design pattern type-state.
