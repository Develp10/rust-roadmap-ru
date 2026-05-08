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
