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
