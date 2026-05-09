# Урок 6. Слайсы, String и Vec

В этом уроке разбираем три самые часто используемые работающих с памятью сущности: `Vec<T>`, `String` и слайсы `&[T]`/`&str`. Вы постоянно будете встречать их в любом Rust-коде.

## Теория

### Слайс — окно в чужие данные

Слайс `&[T]` — это **жирный указатель**: пара (указатель, длина). Он не владеет данными, а лишь даёт временный доступ к непрерывному участку памяти.

```rust
let v = vec![1, 2, 3, 4];
let s: &[i32] = &v[1..3];   // [2, 3], не копия — просто окно
println!("{s:?}");
```

То же для строк: `&str` — это слайс байтов, гарантированно валидный UTF-8.

```rust
let text = "hello world";
let w: &str = &text[0..5];
println!("{w}");
```

Слайс — это аналог «view» в других языках. Передача в функцию таких ссылок дешёвая (два машинных слова) и не аллоцирует.

### Диапазоны для слайсов

```rust
let v = vec![10, 20, 30, 40, 50];
let a = &v[..];          // весь вектор как слайс
let b = &v[1..];         // от индекса 1 до конца
let c = &v[..3];         // до индекса 3
let d = &v[1..=3];       // включительно
let e = &v[1..4];        // полуоткрытый — стандартная форма
```

Безопасный вариант — `v.get(1..3)` возвращает `Option<&[T]>` и не паникует при выходе за границы.

### String vs &str

| Свойство | `String` | `&str` |
|----------|-----------|---------|
| Где данные | в куче | где-то (стек, куча, секция данных) |
| Владеет ли | да | нет |
| Можно ли менять | да (с `mut`) | нельзя |
| Размер на стеке | 24 байта (ptr+len+cap) | 16 байт (ptr+len) |
| Когда использовать | поле структуры, расти/менять | параметр функции, литерал, подсрез |

Строковые литералы (`"hello"`) имеют тип `&'static str` и лежат в секции данных бинаря — на них всегда можно ссылаться.

```rust
let s1: &str = "hello";              // литерал
let s2: String = String::from("hi"); // владение
let s3: String = "world".to_string();
let s4: String = format!("привет, {}", "мир");

let v: &str = &s2;        // автоматически превращается в &str
```

### Индексация строк

В Rust **запрещено** индексировать строку по байту:

```rust
let s = String::from("привет");
// let c = s[0];   // error: `String` cannot be indexed by `{integer}`
```

Причина: один Unicode-символ занимает от 1 до 4 байт в UTF-8, поэтому неясно, что должна вернуть `s[0]` — байт, кодовую точку или графему. Вместо индексации:

```rust
let s = "привет";
println!("{}", s.len());                       // 12 (в байтах UTF-8)
println!("{}", s.chars().count());             // 6
for b in s.bytes() { print!("{b:#x} "); }      // байты
println!();
for c in s.chars() { print!("{c} "); }         // символы
println!();
for (i, c) in s.char_indices() { println!("{i}: {c}"); }  // байтовый индекс + символ
```

Срезы `&s[a..b]` работают, но **только по валидным границам кодовых точек** — иначе паника. Используйте `s.char_indices()`, если нужно резать с гарантией.

### Vec<T> — основной контейнер

`Vec<T>` — это рост-массив на куче. Внутри лежит тройка (ptr, len, cap):

- `len` — текущее число элементов;
- `cap` — выделенная ёмкость;
- при превышении `cap` происходит реаллокация (обычно с удвоением).

Создание:

```rust
let v1: Vec<i32> = Vec::new();
let v2 = vec![1, 2, 3];
let v3 = vec![0u8; 1024];          // 1024 нуля
let v4 = Vec::with_capacity(100);  // ёмкость заранее
```

Основные операции:

```rust
let mut v = vec![3, 1, 4];
v.push(1);              // [3, 1, 4, 1]
v.insert(0, 99);        // [99, 3, 1, 4, 1]
v.remove(0);            // [3, 1, 4, 1]
let last = v.pop();     // Some(1)
v.sort();               // [1, 3, 4]
v.dedup();              // удалить подряд идущие дубли
v.retain(|x| *x > 1);   // оставить только > 1
v.reverse();
let n = v.len();
let empty = v.is_empty();
v.clear();              // оставить аллокацию, обнулить len
```

Итерация даёт три варианта:

```rust
let v = vec![1, 2, 3];
for x in v.iter() { /* &i32 */ }
let mut v = vec![1, 2, 3];
for x in v.iter_mut() { *x *= 2; }     // [2, 4, 6]
let v = vec![1, 2, 3];
for x in v.into_iter() { /* i32 */ }   // забирает вектор
```

`for x in &v` эквивалентно `v.iter()`, `for x in &mut v` — `v.iter_mut()`, `for x in v` — `v.into_iter()`.

### Реаллокация и инвалидация ссылок

```rust
let mut v = vec![1, 2, 3];
let r = &v[0];    // &i32 в буфер
v.push(99);       // может переаллоцировать!
// println!("{r}"); // ошибка компиляции
```

Borrow-checker ловит это статически: пока живы `&`-ссылки на содержимое, нельзя звать методы, требующие `&mut`.

### Capacity и производительность

```rust
let mut v: Vec<i32> = Vec::new();
println!("{} {}", v.len(), v.capacity());     // 0 0
v.push(1);
println!("{} {}", v.len(), v.capacity());     // 1 4
for i in 0..16 { v.push(i); }
println!("{} {}", v.len(), v.capacity());     // 17 16 или 32
```

Если вы знаете число элементов заранее — используйте `Vec::with_capacity(n)`, чтобы избежать перевыделений.

### Распространённые операции с Vec и итераторами

```rust
let v = vec![1, 2, 3, 4, 5];
let sum: i32 = v.iter().sum();
let product: i32 = v.iter().product();
let max = v.iter().max();             // Option<&i32>
let evens: Vec<_> = v.iter().filter(|x| **x % 2 == 0).collect();
let squares: Vec<_> = v.iter().map(|x| x * x).collect();
let any_neg = v.iter().any(|x| *x < 0);
let all_pos = v.iter().all(|x| *x > 0);
```

Подробно итераторы — урок 11.

### String как Vec<u8> с гарантиями

Внутри `String` хранится `Vec<u8>` с инвариантом «байты — валидный UTF-8». Поэтому:

- Расти/обрезать по символам нельзя так же дёшево, как по байтам.
- Конвертация `String -> Vec<u8>` бесплатна: `s.into_bytes()`.
- Обратная конвертация `Vec<u8> -> String` требует проверки UTF-8: `String::from_utf8(bytes)`.

```rust
let s = String::from("ru");
let bytes: Vec<u8> = s.into_bytes();
let back = String::from_utf8(bytes).unwrap();
println!("{back}");
```

## Практика

### Задача 1. Первое слово

```rust
fn first_word(s: &str) -> &str {
    match s.find(' ') {
        Some(i) => &s[..i],
        None => s,
    }
}

fn main() {
    println!("{}", first_word("hello world"));
    println!("{}", first_word("одно_слово"));
    println!("{}", first_word(""));
}
```

### Задача 2. Уникальные отсортированные

```rust
fn unique_sorted(mut v: Vec<i32>) -> Vec<i32> {
    v.sort();
    v.dedup();
    v
}

fn main() {
    println!("{:?}", unique_sorted(vec![3,1,2,2,3,1,4]));
}
```

### Задача 3. Реверс строки по символам

```rust
fn reverse(s: &str) -> String {
    s.chars().rev().collect()
}

fn main() {
    println!("{}", reverse("привет"));    // тевирп
    println!("{}", reverse("a 🦀 b"));     // b 🦀 a
}
```

### Задача 4. Таблица повторов

```rust
use std::collections::HashMap;

fn count_words(text: &str) -> HashMap<String, u32> {
    let mut m: HashMap<String, u32> = HashMap::new();
    for w in text.split_whitespace() {
        *m.entry(w.to_lowercase()).or_insert(0) += 1;
    }
    m
}

fn main() {
    let m = count_words("Rust rust RUST безопасный безопасный");
    let mut pairs: Vec<_> = m.into_iter().collect();
    pairs.sort_by(|a, b| b.1.cmp(&a.1));
    for (w, n) in pairs { println!("{w}: {n}"); }
}
```

### Задача 5. Объединение векторов без дубликатов

```rust
fn merge_unique(a: &[i32], b: &[i32]) -> Vec<i32> {
    let mut out: Vec<i32> = a.iter().chain(b.iter()).copied().collect();
    out.sort();
    out.dedup();
    out
}

fn main() {
    println!("{:?}", merge_unique(&[1,2,3,4], &[3,4,5,6]));
}
```

### Задача 6. Безопасное получение по индексу

```rust
fn nth(v: &[i32], i: usize) -> i32 {
    v.get(i).copied().unwrap_or(-1)
}

fn main() {
    let v = vec![10, 20, 30];
    for i in 0..5 {
        println!("{i}: {}", nth(&v, i));
    }
}
```

### Задача 7. Капитализация слов

```rust
fn capitalize(word: &str) -> String {
    let mut chars = word.chars();
    match chars.next() {
        Some(c) => c.to_uppercase().chain(chars).collect(),
        None => String::new(),
    }
}

fn capitalize_all(text: &str) -> String {
    text.split_whitespace().map(capitalize).collect::<Vec<_>>().join(" ")
}

fn main() {
    println!("{}", capitalize_all("привет мир от rust"));
}
```

### Задача 8. Ёмкость и производительность

```rust
fn build(n: usize) -> Vec<u32> {
    let mut v = Vec::with_capacity(n);
    for i in 0..n as u32 { v.push(i * i); }
    v
}

fn main() {
    let v = build(10);
    println!("{:?} cap={}", &v[..5], v.capacity());
}
```

Сравните, сколько раз растёт `capacity` при `Vec::new()` против `Vec::with_capacity(n)` — на больших `n` разница ощутима.

## Тест

**1.** Почему это не компилируется?

```rust
fn main() {
    let s = String::from("привет");
    let c = s[0];
    println!("{c}");
}
```

**2.** Что выведет?

```rust
fn main() {
    let s = "руст";
    println!("len={} chars={}", s.len(), s.chars().count());
}
```

**3.** Почему `&v[1..3]` паникует при `v.len() == 2`, а `v.get(1..3)` — нет?

**4.** Что с этим кодом?

```rust
fn main() {
    let mut v = vec![1, 2, 3];
    let r = &v[0];
    v.push(4);
    println!("{r}");
}
```

**5.** Разница между `iter()`, `iter_mut()` и `into_iter()` у `Vec<T>`?

**6.** Что напечатает программа?

```rust
fn main() {
    let v = vec![1, 2, 3];
    let s: i32 = v.iter().filter(|x| **x % 2 == 1).sum();
    println!("{s}");
}
```

**7.** Чем отличаются `String::from("hi")`, `"hi".to_string()`, `"hi".to_owned()` и `format!("hi")`?

**8.** Что происходит «за кулисами» при `v.push(x)`, если `v.len() == v.capacity()`?

**9.** Скомпилируется ли?

```rust
fn first(s: &str) -> char {
    s.chars().next().unwrap()
}
fn main() {
    let s = String::from("h");
    println!("{}", first(&s));
}
```

**10.** Сколько байт занимает `Vec<i32>` на стеке на 64-битной платформе и почему?

---

### Ответы

1. `String` не реализует `Index<usize>`: один UTF-8 символ может занимать от 1 до 4 байт, поэтому результат `s[0]` неоднозначен. Вместо индексации используют `.bytes()`, `.chars()`, `.char_indices()` или срез `&s[a..b]` по валидным границам.

2. `len=8 chars=4`. «руст» в UTF-8 — 4 кириллические буквы по 2 байта.

3. `&v[1..3]` — это операция `Index`, выход за границы вызывает `panic!`. `.get(range)` возвращает `Option<&[T]>` — при невалидном диапазоне выдаст `None`. Используйте `get`, когда не уверены в границах.

4. Не компилируется. `r` — иммутабельная ссылка в буфер. `push` требует `&mut self` и может реаллоцировать, инвалидируя `r`. Borrow-checker ловит это.

5. `iter()` даёт `Iterator<Item = &T>` (иммутабельные ссылки), `iter_mut()` — `&mut T` (мутабельные), `into_iter()` — `T` (забирает вектор во владение, элементы перемещаются).

6. `4`. Фильтр оставляет нечётные `1, 3`, сумма `4`.

7. По смыслу все четыре дают `String`. `String::from(s)` и `s.to_owned()` — самый прямой путь: копируют байты в новый `Vec<u8>`. `s.to_string()` ходит через `Display`-форматирование, поэтому теоретически дороже, но для строковых литералов оптимизировано. `format!("hi")` — самый «тяжёлый» путь (через форматтер), но обязателен, когда нужно подставлять значения.

8. Происходит **реаллокация**: запрашивается новый буфер размером (обычно) `2 * old_cap` или `max(old_cap*2, 4)`, существующие элементы перемещаются в новый буфер, старый освобождается. Любые ссылки в старый буфер становятся невалидными, поэтому borrow-checker запрещает `push` при живых `&`-ссылках.

9. Скомпилируется. `&s` — это `&String`, и компилятор автоматически превращает его в `&str` через коэрцию `Deref`. Это причина, по которой принимают `&str` — функция работает и со `String`, и с литералами.

10. 24 байта: указатель на буфер (8) + длина (8 `usize`) + ёмкость (8 `usize`). На 32-битной платформе было бы 12 байт.
# Урок 6. Слайсы, String и Vec

## Теория

### Слайс

Слайс `&[T]` / `&str` — это жирный указатель (ptr + len), представляющий непрерывный участок памяти. Не владеет данными.

```rust
let v = vec![1, 2, 3, 4];
let s: &[i32] = &v[1..3]; // [2, 3]
let text = "hello world";
let w: &str = &text[0..5];
```

### String vs &str

`String` — владеющий, ростовой буфер UTF-8 на куче.  
`&str` — срез UTF-8, не владеет. Строковые литералы имеют тип `&'static str` и лежат в секции данных бинаря.

Индексация строки по байту запрещена: `s[0]` не компилируется. Используйте `.bytes()`, `.chars()`, `.char_indices()`, срезы `&s[a..b]` по валидным границам кодовых точек.

### Vec<T>

Ростовый массив на куче (ptr, len, cap). Основные вызовы: `push`, `pop`, `insert`, `remove`, `iter()`, `iter_mut()`, `into_iter()`, `retain`, `sort`.

```rust
let mut v = vec![3, 1, 2];
v.sort();
v.retain(|x| *x > 1);
let s: i32 = v.iter().sum();
```

## Практика

```rust
fn first_word(s: &str) -> &str {
    match s.find(' ') {
        Some(i) => &s[..i],
        None => s,
    }
}

fn unique_sorted(v: Vec<i32>) -> Vec<i32> {
    let mut v = v;
    v.sort();
    v.dedup();
    v
}

fn main() {
    println!("{}", first_word("hello world"));
    println!("{:?}", unique_sorted(vec![3,1,2,2,3,1]));
}
```

## Тест

**1.** Почему это не компилируется?
```rust
fn main() {
    let s = String::from("привет");
    let c = s[0];
    println!("{c}");
}
```

**2.** Что выведет?
```rust
fn main() {
    let s = "руст";
    println!("len={} chars={}", s.len(), s.chars().count());
}
```

**3.** Почему `&v[1..3]` паникует при `v.len() == 2`, а `v.get(1..3)` — нет?

**4.** Что с этим кодом?
```rust
fn main() {
    let mut v = vec![1, 2, 3];
    let r = &v[0];
    v.push(4);
    println!("{r}");
}
```

**5.** Разница между `iter()`, `iter_mut()` и `into_iter()` у `Vec<T>`?

---

### Ответы

1. `String` не реализует `Index<usize>`: один символ UTF-8 может занимать от 1 до 4 байт. Из-за этого результат был бы неоднозначен. Нужны `.bytes()`, `.chars()` или срез.

2. `len=8 chars=4`: "руст" в UTF-8 — 4 кириллические буквы по 2 байта.

3. `&v[1..3]` — операция Index, выход за границы → `panic!`. `.get(range)` возвращает `Option<&[T]>` — при невалидности даёт `None`.

4. Не компилируется. `r` — иммутабельная ссылка в буфер. `push` требует `&mut` и может реаллоцировать, инвалидируя `r`.

5. `iter()` даёт `Iterator<Item=&T>`, `iter_mut()` — `&mut T`, `into_iter()` — `T` (забирает вектор во владение).
