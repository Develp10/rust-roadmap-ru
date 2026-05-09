# Урок 3. Управление потоком и pattern matching

Управление потоком в Rust строится вокруг идеи: `if`, `match`, `loop` — это всё **выражения**, которые возвращают значения. Это даёт более лаконичный и предсказуемый код, чем привычный imperative-стиль.

## Теория

### if/else как выражение

```rust
let n = 7;
let parity = if n % 2 == 0 { "even" } else { "odd" };
```

Особенности:

- Условие должно иметь тип `bool`. `if 1 { ... }` — ошибка.
- Все ветви должны возвращать один и тот же тип. Иначе выводы вывода типов ломаются.
- Если `else` пропущен, тип ветви — `()`.
- Цепочки `if ... else if ... else` работают, но при большом количестве вариантов лучше `match`.

```rust
let temp = 22;
let category = if temp < 0 { "холодно" }
               else if temp < 15 { "прохладно" }
               else if temp < 25 { "комфортно" }
               else { "жарко" };
```

### Циклы: loop, while, for

В Rust три вида циклов:

```rust
// Бесконечный
loop {
    println!("раз");
    break;
}

// С условием
let mut n = 0;
while n < 5 {
    println!("{n}");
    n += 1;
}

// По итератору
for x in 0..5 { println!("{x}"); }
for x in (0..5).rev() { println!("{x}"); }
for x in (0..10).step_by(2) { println!("{x}"); }
for (i, name) in ["Аня", "Боря"].iter().enumerate() { println!("{i}: {name}"); }
```

`for x in (...)` — самый идиоматичный. Под капотом он использует `IntoIterator` и не требует ручной работы с индексами.

### loop как выражение

`loop` — единственный цикл, из которого можно вернуть значение через `break value`:

```rust
let mut counter = 0;
let result = loop {
    counter += 1;
    if counter == 10 { break counter * 2; }
};
println!("{}", result);  // 20
```

`while` и `for` всегда имеют тип `()` — у них нет аналогичной формы.

### Метки циклов

При вложенных циклах `break` и `continue` относятся к самому внутреннему. Чтобы выйти выше, ставят метку:

```rust
'outer: for i in 0..10 {
    for j in 0..10 {
        if i * j > 50 {
            break 'outer;
        }
        if j == i {
            continue 'outer;
        }
    }
}
```

Метка — это идентификатор с апострофом, как у времён жизни.

### match — ключевая конструкция

`match` — это «паттерн-матчинг на стероидах». Главное свойство: компилятор требует **исчерпывающего** покрытия всех возможных значений (exhaustive). Это критически важно для перечислений и значительно сокращает класс багов.

```rust
let n = 3;
let s = match n {
    1 => "один",
    2 | 3 => "пара",            // несколько вариантов через |
    4..=10 => "немного",         // диапазон inclusive
    n if n < 0 => "отрицательное", // guard
    _ => "много",                 // wildcard
};
```

Возможности паттернов:

- **Литералы**: `1`, `'a'`, `"hello"`.
- **Диапазоны**: `1..=5` (inclusive), `'a'..='z'`.
- **OR-паттерны**: `1 | 2 | 3`.
- **Биндинги `@`**: `x @ 1..=5` — захватывает значение в `x` и одновременно проверяет диапазон.
- **Деструктуризация** кортежей, структур, перечислений: `(a, b)`, `Point { x, y }`, `Some(v)`.
- **Wildcard**: `_` (одно значение), `..` (остальные поля или элементы).
- **Guards**: дополнительное условие `if ...` после паттерна.
- **Ссылочные паттерны**: `&x` или `ref x`.

Пример с разными возможностями:

```rust
fn describe(point: (i32, i32)) -> &'static str {
    match point {
        (0, 0) => "начало координат",
        (x, 0) if x > 0 => "положительная ось X",
        (x, 0) if x < 0 => "отрицательная ось X",
        (0, _) => "ось Y",
        (x, y) if x == y => "диагональ y=x",
        _ => "точка",
    }
}
```

### if let и while let

Часто `match` нужен только ради одной ветки. Тогда есть короткая форма:

```rust
let maybe = Some(7);

if let Some(v) = maybe {
    println!("есть значение: {v}");
} else {
    println!("ничего");
}

let mut stack = vec![1, 2, 3];
while let Some(top) = stack.pop() {
    println!("снят: {top}");
}
```

Начиная с Rust 1.65 можно использовать `let ... else`:

```rust
fn parse_or_zero(s: &str) -> i32 {
    let Ok(n) = s.parse::<i32>() else {
        return 0;
    };
    n * 2
}
```

Эта конструкция — отличная альтернатива «лестницам» из `match`/`if let`, когда успешный путь основной, а неуспешный — ранний выход.

### Деструктуризация в let

Паттерны можно использовать не только в `match`:

```rust
let (a, b, c) = (1, 2, 3);
let [first, .., last] = [10, 20, 30, 40];
let Point { x, y } = Point { x: 1, y: 2 };
```

### Exhaustiveness и непокрытые варианты

Когда компилятор видит `match`, который не покрывает все случаи, он выдаёт ошибку и подсказывает, какие варианты пропущены. Это особенно мощно для пользовательских `enum`:

```rust
enum Color { Red, Green, Blue }

fn name(c: Color) -> &'static str {
    match c {
        Color::Red => "red",
        Color::Green => "green",
        // error[E0004]: non-exhaustive patterns: `Color::Blue` not covered
    }
}
```

Это огромное преимущество: добавили вариант `Yellow` в `Color` — компилятор сразу покажет все `match`, которые надо обновить.

## Практика

### Задача 1. FizzBuzz через match

```rust
fn fizzbuzz(n: u32) -> String {
    match (n % 3, n % 5) {
        (0, 0) => "FizzBuzz".into(),
        (0, _) => "Fizz".into(),
        (_, 0) => "Buzz".into(),
        (_, _) => n.to_string(),
    }
}

fn main() {
    for i in 1..=20 { println!("{:>2}: {}", i, fizzbuzz(i)); }
}
```

Перепишите этот же FizzBuzz через один `match n` с гвардами.

### Задача 2. Калькулятор с операциями

```rust
fn calc(op: char, a: f64, b: f64) -> Option<f64> {
    match op {
        '+' => Some(a + b),
        '-' => Some(a - b),
        '*' => Some(a * b),
        '/' if b != 0.0 => Some(a / b),
        '/' => None,            // деление на ноль
        _   => None,
    }
}

fn main() {
    let ops = [('+', 2.0, 3.0), ('/', 10.0, 0.0), ('*', 4.0, 5.0)];
    for (op, a, b) in ops {
        println!("{a} {op} {b} = {:?}", calc(op, a, b));
    }
}
```

### Задача 3. Классификация треугольника

```rust
#[derive(Debug)]
enum Triangle { Equilateral, Isosceles, Scalene, Invalid }

fn classify(a: u32, b: u32, c: u32) -> Triangle {
    if a + b <= c || a + c <= b || b + c <= a {
        return Triangle::Invalid;
    }
    match (a == b, b == c, a == c) {
        (true, true, _) => Triangle::Equilateral,
        (true, _, _) | (_, true, _) | (_, _, true) => Triangle::Isosceles,
        _ => Triangle::Scalene,
    }
}

fn main() {
    let tests = [(3, 3, 3), (5, 5, 8), (3, 4, 5), (1, 2, 5)];
    for (a, b, c) in tests {
        println!("{a}-{b}-{c} -> {:?}", classify(a, b, c));
    }
}
```

### Задача 4. Поиск в коллекции

```rust
fn find_first_even(nums: &[i32]) -> Option<usize> {
    for (i, &n) in nums.iter().enumerate() {
        if n % 2 == 0 {
            return Some(i);
        }
    }
    None
}

fn main() {
    println!("{:?}", find_first_even(&[1, 3, 5, 8, 9]));   // Some(3)
    println!("{:?}", find_first_even(&[1, 3, 5]));         // None
}
```

Перепишите тело функции через один `.iter().position(...)`.

### Задача 5. Простая стейт-машина

```rust
#[derive(Debug, Clone, Copy)]
enum State { Idle, Working, Done, Error }

fn next(state: State, ok: bool) -> State {
    use State::*;
    match (state, ok) {
        (Idle,    true)  => Working,
        (Working, true)  => Done,
        (Done,    _)     => Done,
        (_,       false) => Error,
        (Error,   _)     => Error,
    }
}

fn main() {
    let mut s = State::Idle;
    for ok in [true, true, true, false, true] {
        s = next(s, ok);
        println!("{ok} -> {s:?}");
    }
}
```

### Задача 6. Парсинг даты с let-else

```rust
fn parse_date(s: &str) -> Option<(u16, u8, u8)> {
    let parts: Vec<&str> = s.split('-').collect();
    let [y, m, d] = parts.as_slice() else { return None; };
    let y: u16 = y.parse().ok()?;
    let m: u8 = m.parse().ok()?;
    let d: u8 = d.parse().ok()?;
    if !(1..=12).contains(&m) || !(1..=31).contains(&d) { return None; }
    Some((y, m, d))
}

fn main() {
    for s in ["2026-05-09", "2026/05/09", "2026-13-01", "2026-12-31"] {
        println!("{s} -> {:?}", parse_date(s));
    }
}
```

## Тест

**1.** Что вернёт этот код?

```rust
fn main() {
    let x = loop {
        break 42;
    };
    println!("{x}");
}
```

**2.** Скомпилируется ли?

```rust
enum Color { Red, Green, Blue }
fn name(c: Color) -> &'static str {
    match c {
        Color::Red => "red",
        Color::Green => "green",
    }
}
```

**3.** В чём разница между `1..5` и `1..=5` в паттерне и в выражении?

**4.** Что напечатается?

```rust
fn main() {
    let n = 7;
    let s = match n {
        x if x < 0 => "neg",
        0 => "zero",
        1..=10 => "small",
        _ => "big",
    };
    println!("{s}");
}
```

**5.** Для чего нужны метки циклов (`'outer: for ...`)?

**6.** Что напечатает программа?

```rust
fn main() {
    let p = (3, 0);
    let s = match p {
        (0, 0) => "origin",
        (_, 0) => "x-axis",
        (0, _) => "y-axis",
        _ => "elsewhere",
    };
    println!("{s}");
}
```

**7.** Чем `if let Some(x) = opt { ... }` отличается от `match opt { Some(x) => ..., None => () }`?

**8.** Что напечатает код?

```rust
fn main() {
    let v = vec![1, 2, 3];
    let s = if let [a, b, c] = v.as_slice() { a + b + c } else { 0 };
    println!("{s}");
}
```

**9.** Зачем нужен `let ... else` и чем он лучше комбинации `match` + ранний `return`?

**10.** Что выведет код?

```rust
fn main() {
    let mut sum = 0;
    'outer: for i in 1..=5 {
        for j in 1..=5 {
            if j > i { continue 'outer; }
            sum += j;
        }
    }
    println!("{sum}");
}
```

---

### Ответы

1. `42`. `loop` — выражение, `break 42` возвращает значение из него. Тип выводится как `i32`.

2. Не скомпилируется. `match` обязан быть исчерпывающим: вариант `Color::Blue` не покрыт. Компилятор предложит добавить плечо `Color::Blue => ...` или `_ => ...`.

3. В **выражениях** оба работают давно: `1..5` — это `Range` (полуоткрытый, без 5), `1..=5` — `RangeInclusive` (с 5). В **паттернах** долгое время был только `..=`. Полуоткрытые `a..b` в паттернах стабилизированы в Rust 1.80; в более старых версиях надо было использовать диапазон с `..=` или гарду.

4. `small`. Гард `x < 0` не сработал, литерал `0` не подошёл, диапазон `1..=10` подошёл.

5. Чтобы из вложенного цикла выйти во внешний (`break 'outer`) или продолжить внешний (`continue 'outer`). Альтернатива — флаги-костыли — менее читабельна и дороже.

6. `x-axis`. Точка `(3, 0)` не подходит под `(0, 0)`, подходит под `(_, 0)` и попадает в эту ветвь.

7. Семантически они эквивалентны. `if let` короче и читается «выполни блок, если паттерн совпал». Если есть и `else`, обе формы делают одно и то же; `match` предпочтительнее, когда вариантов больше двух.

8. `6`. Слайс длины 3 разобран в `a, b, c`, сумма `1 + 2 + 3 = 6`.

9. `let ... else` хорошо подходит, когда дальше по функции нужен «развязанный» биндинг успешного варианта, а неуспех — ранний выход. Альтернатива через `match` + `return` работает, но требует больше кода и часто вынужденного `{ ... }`. `let-else` делает основной путь линейным.

10. `15`. Для `i = 1`: добавится `1`. Для `i = 2`: добавятся `1 + 2 = 3`. И так далее: `1 + 3 + 6 + 10 + 15 = ?` Нет: на каждой итерации внешнего цикла внутренний прибавляет `1+2+...+i`. Сумма = `1 + 3 + 6 + 10 + 15 = 35`. Точный ответ `35`.
# Урок 3. Управление потоком и pattern matching

## Теория

### if/else как выражение

```rust
let n = 7;
let parity = if n % 2 == 0 { "even" } else { "odd" };
```

Ветви должны возвращать одинаковый тип.

### Циклы

- `loop` — бесконечный, `break value` возвращает из него значение.
- `while cond` — пока условие истинно.
- `for x in iter` — по любому итератору.

Метки циклов:

```rust
'outer: for i in 0..10 {
    for j in 0..10 {
        if i * j > 50 { break 'outer; }
    }
}
```

### match

`match` — exhaustive: компилятор требует покрыть все варианты. Паттерны:

- литералы: `1`, `'a'`;
- диапазоны: `1..=5` (inclusive), `1..5` (exclusive);
- биндинги: `x @ 1..=5`;
- деструктуризация: `(a, b)`, `Point { x, y }`, `Some(v)`;
- охват остального: `_`, `..`;
- гварды: `x if x > 0`.

### if let / while let

```rust
if let Some(v) = maybe { println!("есть: {v}"); }
while let Some(top) = stack.pop() { /* ... */ }
```

## Практика

FizzBuzz через `match` без `if`:

```rust
fn fizzbuzz(n: u32) -> String {
    match (n % 3, n % 5) {
        (0, 0) => "FizzBuzz".into(),
        (0, _) => "Fizz".into(),
        (_, 0) => "Buzz".into(),
        (_, _) => n.to_string(),
    }
}

fn main() {
    for i in 1..=20 { println!("{}", fizzbuzz(i)); }
}
```

Дополните: перепишите через один вход в `match n` с гвардами.

## Тест

**1.** Что вернёт этот код?

```rust
fn main() {
    let x = loop {
        break 42;
    };
    println!("{x}");
}
```

**2.** Скомпилируется ли?

```rust
enum Color { Red, Green, Blue }
fn name(c: Color) -> &'static str {
    match c {
        Color::Red => "red",
        Color::Green => "green",
    }
}
```

**3.** Разница между `1..5` и `1..=5` в паттерне и в выражении?

**4.** Что напечатается?

```rust
fn main() {
    let n = 7;
    let s = match n {
        x if x < 0 => "neg",
        0 => "zero",
        1..=10 => "small",
        _ => "big",
    };
    println!("{s}");
}
```

**5.** Для чего нужны метки циклов (`'outer: for ...`)?

---

### Ответы

1. `42`. `loop` — выражение; `break value` возвращает значение из него. Тип выводится как `i32`.

2. Нет. `match` должен быть exhaustive: не покрыт вариант `Color::Blue`. Нужно добавить плечо или `_ => ...`.

3. В выражениях оба работают давно: `1..5` — `Range` (1..4 без 5), `1..=5` — `RangeInclusive`. В паттернах исторически был только `..=`; exclusive `..` в паттернах стабилизирован позже и в старых версиях работает только через фичу.

4. `small`: проверяется гвард (ложь), литерал 0 (нет), диапазон 1..=10 (да).

5. Чтобы из вложенного цикла выйти во внешний или продолжить внешний, избегая флагов-костылей.
