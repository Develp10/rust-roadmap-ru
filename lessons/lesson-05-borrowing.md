# Урок 5. Ссылки и заимствования (References & Borrowing)

## Теория

Ссылка `&T` даёт доступ к значению без передачи владения. Ссылки **всегда валидны** — это инвариант borrow-checker'а.

### Правила заимствования

В любой момент времени можно иметь или:

- любое число `&T` (неизменяемых ссылок),
- **либо** ровно одну `&mut T` (изменяемую),
- но не одновременно.

```rust
fn main() {
    let mut s = String::from("hi");
    let r1 = &s;
    let r2 = &s;
    println!("{r1} {r2}");
    let m = &mut s;   // ок: r1, r2 больше не используются
    m.push_str("!");
}
```

### NLL (Non-Lexical Lifetimes)

Время заимствования определяется не блоком, а фактическим использованием. Компилятор видит, что после `println!` ссылки рабочей больше не нужны.

### Dangling references

```rust
fn dangle() -> &String {     // ошибка
    let s = String::from("x");
    &s
}
```

Ссылка бы указывала на дропнувшую строку. Компилятор отловит это.

## Практика

```rust
fn longest<'a>(a: &'a str, b: &'a str) -> &'a str {
    if a.len() > b.len() { a } else { b }
}

fn add_exclamation(s: &mut String) {
    s.push('!');
}

fn main() {
    let mut greeting = String::from("hello");
    add_exclamation(&mut greeting);
    let other = String::from("hi");
    println!("{}", longest(&greeting, &other));
}
```

## Тест

**1.** Почему это не компилируется?
```rust
fn main() {
    let mut v = vec![1,2,3];
    let r = &v[0];
    v.push(4);
    println!("{r}");
}
```

**2.** А это?
```rust
fn main() {
    let mut s = String::from("x");
    let r1 = &s;
    let r2 = &mut s;
    println!("{r2}");
    println!("{r1}");
}
```

**3.** Что вернёт функция?
```rust
fn first_word(s: &str) -> &str {
    s.split_whitespace().next().unwrap_or("")
}
```
Почему здесь не нужно явно указывать lifetime?

**4.** `&str` всегда реализует `Copy`? Почему?

**5.** Чем reborrow отличается от move для `&mut T`?

---

### Ответы

1. `r` — `&i32` внутрь буфера. `push` может реаллоцировать, и `r` будет указывать в освобождённую память. Borrow-checker запрещает `&mut`-вызов при живой `&`.

2. Не компилируется. `r1` используется после создания `&mut`, значит время жизни обеих пересекается. Переставьте вывод `r1` до `&mut` — и будет ок (NLL).

3. Вернёт подсрез входной строки. Lifetime выводится по правилу elision: единственный input lifetime автоматически привязывается к output.

4. Да. `&str` — фактически жирный указатель (ptr+len), без владения данными. Копирование безопасно.

5. `&mut T` не реализует `Copy`, поэтому передача в функцию похожа на move. Но компилятор автоматически делает reborrow `&mut *r`: оригинал приостанавливается на время вызова и возвращается после.
