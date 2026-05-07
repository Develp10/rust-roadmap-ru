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
