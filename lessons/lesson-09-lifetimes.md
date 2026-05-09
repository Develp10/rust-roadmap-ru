# Урок 9. Времена жизни (lifetimes)

Времена жизни — самая «загадочная» часть Rust для новичков. На самом деле идея простая: lifetime — это **метка**, которая описывает, как долго остаётся валидной ссылка. Компилятор использует эти метки, чтобы убедиться, что ссылка не переживёт данные, на которые указывает.

## Теория

### Что такое lifetime и зачем он нужен

Каждая ссылка в Rust имеет время жизни. Чаще всего его выводит компилятор автоматически, но иногда — особенно когда ссылок несколько — нужно указать связи между ними явно.

Рассмотрим классический пример:

```rust
fn longest(a: &str, b: &str) -> &str {
    if a.len() > b.len() { a } else { b }
}
```

Этот код **не компилируется**: компилятор не знает, какое из двух входных времён жизни должно стать временем жизни возврата. Если `a` живёт 10 строк, а `b` — 3 строки, то ссылка на результат после третьей строки может оказаться висячей.

Чинится явным lifetime-параметром:

```rust
fn longest<'a>(a: &'a str, b: &'a str) -> &'a str {
    if a.len() > b.len() { a } else { b }
}
```

`'a` — это **параметр времени жизни**. Запись говорит: «`a` и `b` живут как минимум `'a`, и возвращаемая ссылка тоже живёт `'a`». Компилятор подбирает `'a` как пересечение времён жизни `a` и `b`.

### Lifetime — это не «сколько живут данные»

Распространённое заблуждение. Lifetime ничего не **меняет** в реальном времени жизни. Он только **описывает** инвариант, который компилятор проверяет. Если вы напишете `'a`, данные не станут жить дольше — но если они умрут раньше, чем обещано, компиляция упадёт.

Аналогия: типы не меняют значения, они описывают их природу. Lifetimes так же — описывают «природу» ссылок.

### Синтаксис

```rust
&i32        // ссылка
&'a i32     // ссылка с явным lifetime 'a
&'a mut i32 // мутабельная ссылка с lifetime 'a
```

`'a` — это идентификатор с апострофом, как и метки циклов. По соглашению используют короткие имена: `'a`, `'b`, иногда смысловые — `'src`, `'ctx`.

### Правила elision (вывода)

Чтобы не писать `'a` в каждой функции, компилятор сам расставляет lifetimes по трём правилам:

1. **Каждый входной параметр-ссылка получает свой собственный lifetime.**
2. **Если входной lifetime ровно один, он автоматически становится lifetime результата.**
3. **Если есть `&self` или `&mut self`, его lifetime присваивается всем выходным ссылкам.**

Если после применения этих правил lifetime результата всё ещё неопределён, компилятор выдаёт ошибку и просит указать `'a` явно.

Примеры:

```rust
fn first(s: &str) -> &str { s.split_whitespace().next().unwrap_or("") }
// эквивалентно: fn first<'a>(s: &'a str) -> &'a str

fn pair<'a>(a: &'a str, b: &'a str) -> (&'a str, &'a str) { (a, b) }
// elision не работает — два входа, два выхода с неоднозначностью

fn first_word(&self) -> &str { /* ... */ }
// lifetime берётся от &self
```

### Структуры со ссылками

Если структура хранит ссылку, ей нужен параметр времени жизни:

```rust
struct Excerpt<'a> {
    part: &'a str,
}

impl<'a> Excerpt<'a> {
    fn new(text: &'a str, start: usize, end: usize) -> Self {
        Self { part: &text[start..end] }
    }
    fn part(&self) -> &str { self.part }
}

fn main() {
    let novel = String::from("Я ходил по улице вчера...");
    let e = Excerpt::new(&novel, 0, 12);
    println!("{}", e.part());
}
```

Запись `Excerpt<'a>` означает: «значение этой структуры не может пережить `'a`». Это естественно: если у вас структура хранит `&'a str`, и оригинальная строка дропнется, доступ к `part` стал бы UB.

### Связь нескольких lifetimes

Иногда нужно сказать «`'b` живёт хотя бы столько, сколько `'a`». Это записывается через `'b: 'a` (читается «`'b` outlives `'a`»):

```rust
fn longest_with<'a, 'b: 'a>(x: &'a str, y: &'b str) -> &'a str {
    if x.len() > y.len() { x } else { y }
}
```

Здесь возвращаем `&'a str`, и `y` («b») должен жить как минимум столько же, чтобы его можно было вернуть как `&'a str`.

### Lifetime `'static`

`'static` — особое время жизни, означающее «всё время работы программы». Самый частый пример — строковые литералы:

```rust
let s: &'static str = "hello";
```

Они лежат в секции данных бинаря и доступны всегда. Также `'static` имеют:

- Глобальные `static`-переменные.
- `Box::leak(Box::new(value))` — намеренная «утечка» в пользу `'static`.

### `T: 'static` — это другое

Очень частая путаница. `&'static T` и `T: 'static` — разные вещи.

- `&'static T` — ссылка, валидная всё время работы программы.
- `T: 'static` — bound, означающий «`T` не содержит ссылок с временем жизни короче `'static`». Любой владеющий тип удовлетворяет: `String`, `Vec<i32>`, `i32`, `Box<dyn Error + 'static>` — все они `'static`.

Поэтому когда вы видите ограничение `T: 'static` (например, в `thread::spawn` или `Box<dyn Trait + 'static>`), оно **не** требует `&'static`-ссылок — лишь чтобы тип не содержал коротко-живущих ссылок.

### Lifetimes и методы

```rust
struct Container<'a> { data: &'a str }

impl<'a> Container<'a> {
    fn new(data: &'a str) -> Self { Self { data } }
    fn get(&self) -> &str { self.data }                    // elision
    fn get_explicit(&self) -> &'a str { self.data }        // явно: lifetime данных, не self
    fn announce<'b>(&self, msg: &'b str) -> &str {
        println!("{msg}");
        self.data
    }
}
```

Заметьте разницу между `get` и `get_explicit`: первая возвращает ссылку, ограниченную `self` (правило elision №3); вторая — ссылку с lifetime данных, что более общо и иногда полезно.

### Subtyping и variance

Lifetimes имеют отношение «короче-длиннее»: `'static` — самое длинное. Если функция принимает `&'a str`, ей можно передать и `&'static str` — это безопасно сужать lifetime.

Это называется **subtyping**: `'static <: 'a` для любого `'a`. На практике это работает «само» и не требует от вас осознанных действий — но объясняет, почему передача литерала в функцию с любым `'a` всегда успешна.

### Higher-Ranked Trait Bounds (HRTB)

Иногда нужно сказать «функция работает с **любым** lifetime»:

```rust
fn apply<F>(f: F) where F: for<'a> Fn(&'a str) -> &'a str {
    let s = String::from("hello");
    let r = f(&s);
    println!("{r}");
}
```

`for<'a>` — это HRTB. Он нужен, когда замыкание/функция вызывается внутри с лозунгом «я заранее не знаю, какой lifetime будет у аргумента, но обещаю работать с любым». Часто встречается в библиотеках.

### Lifetimes в trait objects

```rust
fn make_iter<'a>(slice: &'a [i32]) -> Box<dyn Iterator<Item = &'a i32> + 'a> {
    Box::new(slice.iter())
}
```

`Box<dyn Trait + 'a>` — указатель на динамический тип, время жизни которого ограничено `'a`. По умолчанию для `dyn Trait` подразумевается `'static`, поэтому если внутри есть ссылки — нужно указать lifetime явно.

### Распространённые ошибки

1. **`borrowed value does not live long enough`** — данные дропаются раньше, чем заканчивается lifetime ссылки. Решение: переменная должна жить дольше, либо вернуть значение по владению.
2. **`missing lifetime specifier`** — компилятор не смог вывести lifetime по правилам elision. Решение: добавить `'a` явно.
3. **`cannot return reference to local variable`** — ссылка на локальную переменную после возврата. Решение: вернуть `String`/`Vec` по владению, либо принять `&mut`-буфер от вызывающего.
4. **`lifetime mismatch`** — выводимые lifetimes не сходятся. Решение: явные `'a` и связи `'b: 'a`.

## Практика

### Задача 1. Самая длинная строка

```rust
fn longest<'a>(a: &'a str, b: &'a str) -> &'a str {
    if a.len() > b.len() { a } else { b }
}

fn main() {
    let s1 = String::from("короткая");
    let s2 = "очень длинная статическая строка";
    let r = longest(&s1, s2);
    println!("{r}");
}
```

### Задача 2. Структура с ссылкой

```rust
struct Excerpt<'a> { part: &'a str }

impl<'a> Excerpt<'a> {
    fn new(text: &'a str) -> Self {
        let first_sentence = text.split('.').next().unwrap_or(text);
        Excerpt { part: first_sentence }
    }
}

fn main() {
    let novel = String::from("Это первое предложение. А это второе.");
    let e = Excerpt::new(&novel);
    println!("первое: {}", e.part);
}
```

### Задача 3. Метод с разными lifetime

```rust
struct Parser<'a> { source: &'a str, pos: usize }

impl<'a> Parser<'a> {
    fn new(s: &'a str) -> Self { Self { source: s, pos: 0 } }
    fn next_word(&mut self) -> Option<&'a str> {
        let rest = &self.source[self.pos..];
        let trimmed = rest.trim_start();
        self.pos += rest.len() - trimmed.len();
        let end = trimmed.find(char::is_whitespace).unwrap_or(trimmed.len());
        if end == 0 { return None; }
        let word = &trimmed[..end];
        self.pos += end;
        Some(word)
    }
}

fn main() {
    let text = String::from("раз два три четыре");
    let mut p = Parser::new(&text);
    while let Some(w) = p.next_word() {
        println!("{w}");
    }
}
```

Обратите внимание: `next_word` возвращает `Option<&'a str>`, а не `Option<&str>` (ссылку с lifetime от `&self`). Это важно: возвращаемая ссылка живёт столько же, сколько исходный текст, а не столько, сколько `&mut self`.

### Задача 4. Lifetime у итератора

```rust
fn first_word(s: &str) -> &str {
    s.split_whitespace().next().unwrap_or("")
}

fn last_char(s: &str) -> Option<char> {
    s.chars().last()
}

fn main() {
    let s = String::from("hello world");
    println!("{} {:?}", first_word(&s), last_char(&s));
}
```

Здесь работает elision: один входной lifetime → он же выходной.

### Задача 5. Несколько lifetime

```rust
fn pick<'a, 'b>(short: &'a str, long: &'b str, want_short: bool) -> &'a str
where 'b: 'a
{
    if want_short { short } else { long }
}

fn main() {
    let a = String::from("a");
    let b = String::from("длинная-длинная");
    println!("{}", pick(&a, &b, false));
}
```

`'b: 'a` гарантирует, что `b` живёт хотя бы столько, чтобы её можно было вернуть как `&'a str`.

### Задача 6. Box::leak для 'static

```rust
fn make_static_str(s: String) -> &'static str {
    Box::leak(s.into_boxed_str())
}

fn main() {
    let owned = String::from("dynamically built");
    let leaked: &'static str = make_static_str(owned);
    println!("{}", leaked);
}
```

`Box::leak` намеренно «забывает» освободить память, превращая владение в `&'static`. Используется редко: например, для конфигов, читаемых один раз и живущих всю программу.

### Задача 7. Тип-парсер с lifetime

```rust
#[derive(Debug)]
struct KeyVal<'a> { key: &'a str, value: &'a str }

fn parse_kv(s: &str) -> Option<KeyVal<'_>> {
    let (k, v) = s.split_once('=')?;
    Some(KeyVal { key: k.trim(), value: v.trim() })
}

fn main() {
    for line in ["host = localhost", "port=8080", "broken"] {
        println!("{line:?} -> {:?}", parse_kv(line));
    }
}
```

`KeyVal<'_>` — анонимный lifetime: компилятор подставит подходящий. Альтернативно: `KeyVal<'a>` с явным параметром у функции.

## Тест

**1.** Почему это не компилируется?

```rust
fn first<'a>(s: &'a str) -> &'a str {
    let local = String::from(s);
    &local
}
```

**2.** Как исправить, чтобы компилировалось?

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

**4.** Что значит ограничение `T: 'static` и как оно отличается от `&'static T`?

**5.** Можно ли в `struct` хранить ссылку без указания lifetime?

**6.** Что выведет программа?

```rust
fn main() {
    let s = String::from("hello");
    let r;
    {
        let t = String::from("world");
        r = if s.len() > t.len() { &s } else { &t };
        println!("{r}");
    }
}
```

**7.** Скомпилируется ли?

```rust
struct Wrap<'a> { r: &'a i32 }

fn main() {
    let w;
    let n = 5;
    w = Wrap { r: &n };
    println!("{}", w.r);
}
```

**8.** Что означает `fn f<'a>(x: &'a str) -> &'a str` по сравнению с `fn f(x: &str) -> &str` после применения elision?

**9.** Почему этот код требует явных lifetime-параметров?

```rust
fn split_and_first(s: &str, sep: &str) -> &str {
    s.split(sep).next().unwrap_or(s)
}
```

**10.** Что означает `'b: 'a` в where-клаузе и зачем это нужно?

---

### Ответы

1. `local` — локальная переменная функции, она дропается при выходе. Возвращаемая ссылка `&local` была бы висячей. Компилятор это видит и отклоняет код. Чинится возвратом `String` по владению или приёмом `&mut`-буфера снаружи.

2. Добавить явный lifetime: `fn longest<'a>(a: &'a str, b: &'a str) -> &'a str`. Компилятор не может вывести lifetime результата по elision — два входных, неоднозначность.

3. Напечатает `x`. `println!` находится внутри блока, где `s` ещё жива и ссылка `r` валидна. Если бы `println!` стоял после закрывающей скобки — была бы ошибка компиляции.

4. `&'static T` — это ссылка, которая валидна всё время работы программы (например, литерал `"hi"`). `T: 'static` — это bound, гарантирующий, что тип `T` не содержит ссылок короче `'static`. Любой владеющий тип (`String`, `Vec<i32>`, `i32`) — `'static`. Поэтому `thread::spawn` принимает любые владеющие данные.

5. Нет. Любая ссылка в полях требует параметра lifetime: `struct R<'a> { x: &'a str }`. Без параметра компилятор не сможет проверить, что инстанс `R` не переживёт данные, на которые указывает `x`.

6. Не скомпилируется. `r` живёт дольше блока, а ссылается на `t`, которая внутри блока. После закрывающей скобки `t` дропается, а `r` всё ещё пытается ссылаться. Borrow checker отклонит код.

7. Не скомпилируется по тому же принципу: `w` объявлена раньше `n`, поэтому имеет более длинный lifetime, а пытается ссылаться на `n`, у которой lifetime короче. Компилятор не пропустит.

8. Они эквивалентны. По правилу elision №2: один входной lifetime автоматически становится lifetime результата. Поэтому `fn f(x: &str) -> &str` развёртывается ровно в `fn f<'a>(x: &'a str) -> &'a str`.

9. Здесь два входных lifetime — `s` и `sep`. Правило elision №2 не применяется, и компилятор не знает, к какому из них привязать выходную ссылку. Чинится явным lifetime: `fn split_and_first<'a>(s: &'a str, sep: &str) -> &'a str`.

10. `'b: 'a` означает «`'b` outlives `'a`», то есть `'b` живёт как минимум столько же, сколько `'a`. Нужно, когда мы хотим вернуть ссылку с lifetime `'a`, но в качестве источника может выступать значение с другим lifetime `'b`: чтобы возврат был валидным, `'b` обязан перекрывать `'a`.
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
