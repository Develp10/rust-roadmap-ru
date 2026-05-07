# Урок 9. Времена жизни (lifetimes)

## Теория

Lifetimes — это статическое описание того, сколько живёт ссылка. Они ничего не меняют во времени жизни данных — это средство выражать связи между ссылками для компилятора.

### Правила elision

1. Каждый входной параметр-ссылка получает свой lifetime.
2. Если input lifetime ровно один, он присваивается всем output lifetimes.
3. Если есть `&self`/`&mut self`, его lifetime присваивается всем output.

### Явные lifetimes

```rust
fn longest<'a>(a: &'a str, b: &'a str) -> &'a str {
    if a.len() > b.len() { a } else { b }
}
```

`'a` здесь — пересечение времён жизни `a` и `b`.

### Структуры со ссылками

```rust
struct Excerpt<'a> { part: &'a str }
```

### 'static

Строковые литералы имеют тип `&'static str`. `T: 'static` — это ограничение trait bound, а не время жизни ссылки.

## Практика

```rust
fn longest_with<'a, 'b>(x: &'a str, y: &'b str) -> &'a str
where 'b: 'a {
    if x.len() > y.len() { x } else { y }
}
```

## Тест

**1.** Почему это не компилируется?
```rust
fn first<'a>(s: &'a str) -> &'a str {
    let local = String::from(s);
    &local
}
```

**2.** Как исправить?
```rust
fn longest(a: &str, b: &str) -> &str {
    if a.len() > b.len() { a } else { b }
}
```

**3.** Что выведет?
```rust
fn main() {
    let r;
    {
        let s = String::from("x");
        r = &s;
        println!("{r}");
    }
}
```

**4.** Что значит `T: 'static`?

**5.** Можно ли в `struct` хранить ссылку без lifetime?

---

### Ответы

1. `local` дропается на выходе из функции. Ссылка была бы dangling.

2. Добавить времена жизни: `fn longest<'a>(a: &'a str, b: &'a str) -> &'a str`.

3. `x`. Печать внутри блока, где `s` жива.

4. Тип не содержит не-`'static` ссылок. Владеющие типы (`String`, `Vec`) удовлетворяют этому.

5. Нет. Нужен lifetime-параметр: `struct R<'a> { x: &'a str }`.
