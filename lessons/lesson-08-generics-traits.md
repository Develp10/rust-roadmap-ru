# Урок 8. Дженерики и трейты

Дженерики и трейты — это два краеугольных механизма Rust для абстракции. Дженерики дают полиморфизм по типу с **нулевой стоимостью в рантайме** через мономорфизацию, а трейты описывают «поведение», которое можно требовать от типов.

## Теория

### Дженерики (generic types)

Дженерик — это функция, структура, enum или метод, параметризованные **типом**.

```rust
fn largest<T: PartialOrd + Copy>(list: &[T]) -> T {
    let mut max = list[0];
    for &x in list {
        if x > max { max = x; }
    }
    max
}

fn main() {
    println!("{}", largest(&[10, 25, 3, 17]));      // 25
    println!("{}", largest(&[3.1, 2.7, 1.6]));       // 3.1
    println!("{}", largest(&['a', 'z', 'b']));       // z
}
```

`<T: PartialOrd + Copy>` называется **trait bound**: «`T` — любой тип, реализующий `PartialOrd` и `Copy`».

### Мономорфизация

В отличие от дженериков в Java или C#, в Rust на этапе компиляции для каждого использованного типа создаётся **отдельная копия функции**. Это называется мономорфизация. Поэтому:

- Нет накладных расходов в рантайме.
- Нет boxing'а маленьких типов.
- Размер бинарника может расти, если использовать дженерик с десятками разных типов.

```rust
fn id<T>(x: T) -> T { x }

let a = id(5);          // компилируется как fn id_i32(x: i32) -> i32
let b = id("hi");       // компилируется как fn id_str(x: &str) -> &str
```

### Дженерики в структурах

```rust
struct Point<T> { x: T, y: T }

impl<T: std::ops::Add<Output = T> + Copy> Point<T> {
    fn sum(&self) -> T { self.x + self.y }
}

impl Point<f64> {
    fn distance_from_origin(&self) -> f64 {
        (self.x * self.x + self.y * self.y).sqrt()
    }
}

let p = Point { x: 3.0, y: 4.0 };
println!("{}", p.distance_from_origin());     // 5
```

Заметьте: `distance_from_origin` определён только для `Point<f64>` — это пример *условной реализации*.

### Трейты — описания поведения

Трейт похож на интерфейс из других языков, но с большими возможностями:

```rust
trait Greet {
    fn hello(&self) -> String;
    fn shout(&self) -> String {
        // метод по умолчанию
        self.hello().to_uppercase()
    }
}

struct Cat;
impl Greet for Cat {
    fn hello(&self) -> String { "meow".into() }
}

struct Robot;
impl Greet for Robot {
    fn hello(&self) -> String { "01000001".into() }
    fn shout(&self) -> String { format!("BEEP {}", self.hello()) }
}
```

У трейта могут быть:

- **Обязательные методы** (без тела) — реализатор должен предоставить.
- **Методы по умолчанию** (с телом) — реализатор может переопределить или оставить.
- **Ассоциированные типы** — `type Item;`.
- **Ассоциированные константы** — `const PI: f64;`.

### Trait bounds: разные формы

```rust
// Через дженерик-параметр
fn print_all<T: std::fmt::Debug>(xs: &[T]) {
    for x in xs { println!("{x:?}"); }
}

// Через where (читабельнее при сложных ограничениях)
fn process<T, U>(t: T, u: U) -> T
where
    T: Clone + std::fmt::Debug,
    U: Into<String>,
{
    let _ = u.into();
    t.clone()
}

// impl Trait в аргументе (синтаксический сахар над дженериком)
fn shout(g: impl Greet) -> String { g.shout() }

// impl Trait в возврате (вернуть один конкретный анонимный тип)
fn make_adder(x: i32) -> impl Fn(i32) -> i32 {
    move |y| x + y
}
```

### Статическая vs динамическая диспетчеризация

```rust
// Статическая (мономорфизация): один конкретный тип на вызов
fn static_call<T: Greet>(g: T) { println!("{}", g.hello()); }

// Динамическая (vtable): один код, разные типы за указателем
fn dynamic_call(g: &dyn Greet) { println!("{}", g.hello()); }

let cat = Cat;
static_call(cat);              // компилятор сгенерирует static_call_Cat
let robot = Robot;
dynamic_call(&robot);          // через vtable
```

Различия:

| Свойство | `impl Trait` (static) | `dyn Trait` (dynamic) |
|----------|-----------------------|------------------------|
| Где выбирается тип | компиляция | рантайм |
| Стоимость вызова | прямой call (можно инлайнить) | косвенный через vtable |
| Размер | известен | unsized — нужен указатель |
| Гомогенные коллекции | один тип | разные типы за `Box<dyn Trait>` |
| Размер бинарника | может расти (мономорфизация) | константный |

```rust
// Гетерогенный список: разные типы под одним dyn-трейтом
let zoo: Vec<Box<dyn Greet>> = vec![Box::new(Cat), Box::new(Robot)];
for animal in &zoo {
    println!("{}", animal.hello());
}
```

### Ассоциированные типы vs дженерик-параметры трейта

```rust
// Ассоциированный тип: одна реализация на тип
trait Iterator2 {
    type Item;
    fn next2(&mut self) -> Option<Self::Item>;
}

// Дженерик-параметр: можно реализовывать многократно с разными T
trait Convert<T> {
    fn convert(&self) -> T;
}

impl Convert<String> for i32 {
    fn convert(&self) -> String { self.to_string() }
}
impl Convert<f64> for i32 {
    fn convert(&self) -> f64 { *self as f64 }
}
```

Стандартный `Iterator` использует ассоциированный тип — у одного типа итератор может выдавать только один тип `Item`. Если бы это был параметр, реализовать `Iterator<u8>` и `Iterator<u16>` для одного и того же типа было бы неудобно и порождало неоднозначности.

### Object safety

Не каждый трейт можно использовать как `dyn Trait`. Трейт **object-safe**, если все его методы:

- не принимают `Self` по значению,
- не имеют дженерик-параметров на уровне метода,
- помечены `where Self: Sized` (если без них нельзя — то метод недоступен через `dyn`).

Иначе компилятор не может построить vtable.

```rust
trait NotObjectSafe {
    fn clone_me(&self) -> Self;     // Self по значению — нельзя dyn
}
```

Если хотите трейт-объект, проектируйте трейт object-safe-ным.

### Operator overloading через трейты

```rust
use std::ops::Add;

#[derive(Debug, Clone, Copy)]
struct V2 { x: f64, y: f64 }

impl Add for V2 {
    type Output = V2;
    fn add(self, rhs: V2) -> V2 {
        V2 { x: self.x + rhs.x, y: self.y + rhs.y }
    }
}

let a = V2 { x: 1.0, y: 2.0 };
let b = V2 { x: 3.0, y: 4.0 };
println!("{:?}", a + b);
```

Операторы `+ - * / == < > !` всё это — синтаксический сахар над трейтами `Add`, `Sub`, `Mul`, `Div`, `PartialEq`, `PartialOrd`, `Not` и т.д.

### Display и Debug

```rust
use std::fmt;

struct Money { amount: u64, currency: String }

impl fmt::Display for Money {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        write!(f, "{} {}", self.amount, self.currency)
    }
}

impl fmt::Debug for Money {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        write!(f, "Money({:?}, {:?})", self.amount, self.currency)
    }
}
```

`Display` (`{}`) — для пользователя, `Debug` (`{:?}`) — для разработчика.

### Orphan rule

Реализовать трейт `T` для типа `U` можно, только если хотя бы один из них определён в текущем крейте. Это нужно, чтобы избежать конфликтующих реализаций между крейтами. Обходится через new-type:

```rust
struct MyVec(Vec<i32>);
impl SomeExternalTrait for MyVec { /* ... */ }
```

## Практика

### Задача 1. Свой трейт Summary

```rust
trait Summary {
    fn title(&self) -> &str;
    fn summarize(&self) -> String {
        format!("(Read more from {}...)", self.title())
    }
}

struct Article { headline: String, content: String }
struct Tweet { user: String, text: String }

impl Summary for Article {
    fn title(&self) -> &str { &self.headline }
    fn summarize(&self) -> String {
        let preview: String = self.content.chars().take(20).collect();
        format!("{}: {}", self.headline, preview)
    }
}
impl Summary for Tweet {
    fn title(&self) -> &str { &self.user }
}

fn print<T: Summary>(item: &T) { println!("{}", item.summarize()); }

fn main() {
    let a = Article { headline: "Rust 2026".into(), content: "Что нового в языке программирования...".into() };
    let t = Tweet { user: "@ferris".into(), text: "I love Rust".into() };
    print(&a);
    print(&t);
}
```

### Задача 2. impl Trait в возврате

```rust
fn make_counter(start: i32, step: i32) -> impl FnMut() -> i32 {
    let mut n = start;
    move || { let cur = n; n += step; cur }
}

fn main() {
    let mut c1 = make_counter(0, 1);
    println!("{} {} {}", c1(), c1(), c1());     // 0 1 2

    let mut c2 = make_counter(10, -2);
    println!("{} {} {}", c2(), c2(), c2());     // 10 8 6
}
```

### Задача 3. Гетерогенный зоопарк

```rust
trait Animal {
    fn name(&self) -> &str;
    fn sound(&self) -> &str;
    fn describe(&self) -> String {
        format!("{} говорит: {}", self.name(), self.sound())
    }
}

struct Dog { name: String }
struct Cow { name: String }
struct Duck { name: String }

impl Animal for Dog { fn name(&self) -> &str { &self.name } fn sound(&self) -> &str { "гав" } }
impl Animal for Cow { fn name(&self) -> &str { &self.name } fn sound(&self) -> &str { "муу" } }
impl Animal for Duck { fn name(&self) -> &str { &self.name } fn sound(&self) -> &str { "кря" } }

fn main() {
    let zoo: Vec<Box<dyn Animal>> = vec![
        Box::new(Dog { name: "Рекс".into() }),
        Box::new(Cow { name: "Бурёнка".into() }),
        Box::new(Duck { name: "Кряква".into() }),
    ];
    for a in &zoo { println!("{}", a.describe()); }
}
```

### Задача 4. Display для своего типа

```rust
use std::fmt;

struct Duration { hours: u32, minutes: u32 }

impl fmt::Display for Duration {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        write!(f, "{}ч {:02}м", self.hours, self.minutes)
    }
}

fn main() {
    let d = Duration { hours: 2, minutes: 5 };
    println!("длительность: {d}");
}
```

### Задача 5. Перегрузка операторов

```rust
use std::ops::{Add, Mul};

#[derive(Debug, Clone, Copy)]
struct V3 { x: f64, y: f64, z: f64 }

impl Add for V3 {
    type Output = V3;
    fn add(self, r: V3) -> V3 { V3 { x: self.x + r.x, y: self.y + r.y, z: self.z + r.z } }
}
impl Mul<f64> for V3 {
    type Output = V3;
    fn mul(self, s: f64) -> V3 { V3 { x: self.x * s, y: self.y * s, z: self.z * s } }
}

fn main() {
    let a = V3 { x: 1.0, y: 2.0, z: 3.0 };
    let b = V3 { x: 4.0, y: 5.0, z: 6.0 };
    println!("{:?}", (a + b) * 2.0);
}
```

### Задача 6. Trait с ассоциированным типом

```rust
trait Container {
    type Item;
    fn first(&self) -> Option<&Self::Item>;
    fn count(&self) -> usize;
}

struct Stack<T> { items: Vec<T> }

impl<T> Container for Stack<T> {
    type Item = T;
    fn first(&self) -> Option<&T> { self.items.first() }
    fn count(&self) -> usize { self.items.len() }
}

fn main() {
    let s = Stack { items: vec![10, 20, 30] };
    println!("{:?} {}", s.first(), s.count());
}
```

### Задача 7. Generic-функция с двумя bound'ами

```rust
use std::fmt::Debug;
use std::ops::Add;

fn sum<T: Add<Output = T> + Copy + Default + Debug>(xs: &[T]) -> T {
    let mut acc = T::default();
    for &x in xs { acc = acc + x; }
    acc
}

fn main() {
    println!("{:?}", sum(&[1, 2, 3, 4]));
    println!("{:?}", sum(&[1.5_f64, 2.5, 3.0]));
}
```

## Тест

**1.** В чём принципиальная разница между `impl Trait` в позиции аргумента и в позиции возврата?

**2.** Почему `fn f() -> dyn Trait` не компилируется, а `fn f() -> Box<dyn Trait>` — да?

**3.** Что такое object safety? Какие конструкции делают трейт **не** object-safe?

**4.** Чем `where T: Clone` отличается от `<T: Clone>` — есть ли семантические различия?

**5.** Что выведет код?

```rust
trait A { fn x(&self) -> i32 { 1 } }
trait B: A { fn x(&self) -> i32 { 2 } }
struct S;
impl A for S {}
impl B for S {}
fn main() { let s = S; println!("{}", s.x()); }
```

**6.** Что такое мономорфизация и какие её плюсы и минусы?

**7.** Почему стандартный `Iterator` использует ассоциированный тип `Item`, а не дженерик-параметр?

**8.** Что напечатает программа?

```rust
trait Speak { fn speak(&self) -> String { "...".into() } }
struct A;
struct B;
impl Speak for A {}
impl Speak for B { fn speak(&self) -> String { "B!".into() } }
fn main() {
    let xs: Vec<Box<dyn Speak>> = vec![Box::new(A), Box::new(B)];
    for x in &xs { println!("{}", x.speak()); }
}
```

**9.** Скомпилируется ли?

```rust
fn pick(b: bool) -> impl Iterator<Item = i32> {
    if b { (0..3).into_iter() } else { vec![10, 20].into_iter() }
}
```

**10.** Что такое orphan rule и зачем он нужен?

---

### Ответы

1. В аргументе `fn f(x: impl Trait)` — это синтаксический сахар над дженериком `fn f<T: Trait>(x: T)`: каждый вызов мономорфизируется под конкретный тип. В возврате `-> impl Trait` — это **один конкретный анонимный тип**, выбранный самой функцией; вызывающий не может «попросить» другой тип, и все ветви функции должны возвращать именно его.

2. `dyn Trait` — unsized (`!Sized`); функция должна возвращать значение известного на стеке размера. `Box<dyn Trait>` — это указатель на куче, у него фиксированный размер, поэтому такое возвращаемое значение валидно.

3. Object safety — свойство трейта, позволяющее использовать его как `dyn Trait` (через vtable). Не object-safe методы: с `Self` в сигнатуре по значению (как `fn clone(&self) -> Self`), с дженерик-параметрами на уровне метода, требующие `Self: Sized` (тогда метод не доступен через `dyn`, но трейт может быть object-safe в целом).

4. Семантически они эквивалентны для простых случаев. `where` читабельнее при сложных ограничениях (HRTB `for<'a> &'a T: Foo`, ограничения на ассоциированные типы) и удобнее размещается на нескольких строках. На современных проектах в основном пишут через `where`.

5. Ошибка компиляции: неоднозначный вызов `s.x()` — есть две реализации с одинаковым именем (`A::x` и `B::x`). Нужно явно: `<S as A>::x(&s)` или `<S as B>::x(&s)`.

6. Мономорфизация — это создание отдельной копии кода для каждой инстанциации дженерика на этапе компиляции. Плюсы: zero-cost (прямые вызовы, инлайнинг, лучшие оптимизации), нет boxing'а маленьких типов. Минусы: рост размера бинарника при использовании одной функции с десятками типов и более долгое время компиляции.

7. Один тип не может разумно реализовывать «итератор» сразу для нескольких `Item` — обычно ровно один тип элементов привязан к итератору. Ассоциированный тип фиксирует одну привязку и делает API однозначным: `v.iter().sum::<i32>()` без двусмысленностей. Если бы `Item` был дженерик-параметром, при вызовах надо было бы постоянно указывать тип явно.

8. Напечатает `...` (для `A` — реализация по умолчанию) и `B!` (для `B` — переопределённая).

9. Не скомпилируется. Возвращаемый тип `impl Iterator<Item = i32>` — один конкретный анонимный тип. Ветви возвращают **разные** типы (`Range<i32>` и `std::vec::IntoIter<i32>`). Чинится через `Box<dyn Iterator<Item = i32>>` или через явное приведение к одному типу (например, `itertools::Either`).

10. Orphan rule запрещает реализовывать трейт `T` для типа `U`, если ни `T`, ни `U` не определены в текущем крейте. Цель — предотвратить конфликтующие реализации между крейтами: иначе крейт A мог бы реализовать `Display for Vec<i32>`, крейт B — другую `Display for Vec<i32>`, и при сборке программы было бы непонятно, какую брать. Обходится через new-type: оборачиваете чужой тип в свой и реализуете трейт на обёртке.
# Урок 8. Дженерики и трейты

## Теория

**Дженерики** позволяют писать код, абстрагированный по типу, который мономорфизируется на этапе компиляции (zero-cost). **Трейты** описывают поведение и используются как ограничения на типы.

### Дженерики

```rust
fn largest<T: PartialOrd + Copy>(list: &[T]) -> T {
    let mut max = list[0];
    for &x in list { if x > max { max = x; } }
    max
}
```

### Трейты

```rust
trait Greet {
    fn hello(&self) -> String;
    fn shout(&self) -> String { self.hello().to_uppercase() } // default
}

struct Cat;
impl Greet for Cat {
    fn hello(&self) -> String { "meow".into() }
}
```

### Trait bounds, where, dyn vs impl Trait

```rust
fn print_all<T>(xs: &[T]) where T: std::fmt::Debug { for x in xs { println!("{:?}", x); } }

fn make_adder(x: i32) -> impl Fn(i32) -> i32 { move |y| x + y } // статически
fn pick(b: bool) -> Box<dyn Greet> { if b { Box::new(Cat) } else { Box::new(Cat) } } // динамически
```

- `impl Trait` — статическая диспетчеризация, один конкретный тип.
- `dyn Trait` — динамическая, через vtable, размер неизвестен ⇒ нужен указатель (`Box`/`&`).

### Associated types vs generics

```rust
trait Iterator2 { type Item; fn next2(&mut self) -> Option<Self::Item>; }
```

Associated type фиксирует один тип на реализацию; дженерик в трейте позволяет много реализаций для разных параметров.

## Практика

Реализуйте трейт `Summary` с методом по умолчанию `summarize` и переопределите его для двух структур.

```rust
trait Summary {
    fn title(&self) -> &str;
    fn summarize(&self) -> String { format!("(Read more from {}...)", self.title()) }
}

struct Article { headline: String, content: String }
struct Tweet { user: String, text: String }

impl Summary for Article {
    fn title(&self) -> &str { &self.headline }
    fn summarize(&self) -> String { format!("{}: {}", self.headline, &self.content[..self.content.len().min(20)]) }
}
impl Summary for Tweet {
    fn title(&self) -> &str { &self.user }
}

fn main() {
    let a = Article { headline: "Rust 2026".into(), content: "Что нового в языке...".into() };
    let t = Tweet { user: "@ferris".into(), text: "I love Rust".into() };
    println!("{}", a.summarize());
    println!("{}", t.summarize());
}
```

## Тест

1. В чём принципиальная разница между `impl Trait` в позиции аргумента и в позиции возврата?
2. Почему `fn f() -> dyn Trait` не компилируется, а `fn f() -> Box<dyn Trait>` — да?
3. Что такое object safety? Какие методы делают трейт не object-safe?
4. Чем `where T: Clone` отличается от `<T: Clone>` — есть ли семантические различия?
5. Что выведет код?
```rust
trait A { fn x(&self) -> i32 { 1 } }
trait B: A { fn x(&self) -> i32 { 2 } }
struct S;
impl A for S {}
impl B for S {}
fn main() { let s = S; println!("{}", s.x()); }
```

---

### Ответы

1. В аргументе `impl Trait` — это сахар над дженериком (каждый вызов мономорфизируется). В возврате — это **один конкретный анонимный тип**, выбранный функцией; вызывающий не может выбрать другой тип.
2. `dyn Trait` — unsized (`!Sized`); функция должна возвращать значение известного размера. `Box<dyn Trait>` — указатель, имеет фиксированный размер.
3. Object safety — свойство трейта, позволяющее использовать его как `dyn Trait`. Не object-safe: методы с `Self` в сигнатуре по значению, дженерик-методы, `Sized`-методы (без `where Self: Sized`).
4. Семантически они эквивалентны для простых случаев, но `where` читабельнее и поддерживает более сложные ограничения (например, `where for<'a> &'a T: Foo`).
5. Ошибка компиляции: неоднозначный вызов `s.x()` — нужно `<S as A>::x(&s)` или `<S as B>::x(&s)`.
