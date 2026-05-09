# Урок 11. Коллекции и итераторы

Коллекции и итераторы — это рабочие лошадки повседневного Rust-кода. Освоить их идиомы важнее, чем выучить любой синтаксический трюк: 80% задач решаются комбинацией `Vec`/`HashMap` и цепочки итераторных адаптеров.

## Теория

### Зачем нужны разные коллекции

Коллекции в `std::collections` живут в куче и **владеют** своими данными. Когда коллекция дропается — дропаются и элементы. Это детерминированно, без сборщика мусора.

Главные коллекции и когда их брать:

| Коллекция | Доступ | Вставка | Особенности |
|-----------|--------|---------|-------------|
| `Vec<T>` | O(1) по индексу | O(1) push в конец | непрерывная память, лучший кэш |
| `VecDeque<T>` | O(1) по индексу | O(1) с обоих концов | кольцевой буфер |
| `LinkedList<T>` | O(n) | O(1) после курсора | редко нужен |
| `HashMap<K, V>` | O(1) среднее | O(1) среднее | порядок не гарантирован |
| `BTreeMap<K, V>` | O(log n) | O(log n) | отсортирован по ключу |
| `HashSet<T>` | O(1) среднее | O(1) среднее | без дубликатов |
| `BTreeSet<T>` | O(log n) | O(log n) | отсортирован |
| `BinaryHeap<T>` | O(1) max | O(log n) | приоритетная очередь |

Правила выбора:

- Нужен **порядок** или диапазонные запросы — `BTreeMap`/`BTreeSet`.
- Нужна **очередь** (FIFO) или дек — `VecDeque`.
- Нужна **приоритетная очередь** — `BinaryHeap`.
- По умолчанию — `Vec`/`HashMap`/`HashSet`.

### Vec<T>: создание и базовые операции

```rust
let mut v: Vec<i32> = Vec::new();
v.push(1);
v.push(2);
v.push(3);

let v2 = vec![1, 2, 3];                     // макрос
let zeros = vec![0u8; 1024];                // 1024 нуля
let with_cap: Vec<i32> = Vec::with_capacity(100);
```

Самые ходовые методы:

```rust
let mut v = vec![3, 1, 4, 1, 5, 9, 2, 6];
v.push(5);              v.pop();
v.insert(0, 99);        v.remove(0);
v.sort();               v.dedup();
v.reverse();            v.retain(|x| *x > 2);
v.swap(0, 1);           v.contains(&5);
let n = v.len();         let empty = v.is_empty();
v.clear();
```

`extend` добавляет в конец из любого `IntoIterator`:

```rust
let mut v = vec![1, 2];
v.extend([3, 4, 5]);
v.extend(10..15);
```

### HashMap<K, V>

```rust
use std::collections::HashMap;

let mut scores: HashMap<String, i32> = HashMap::new();
scores.insert("Alice".into(), 90);
scores.insert("Bob".into(), 85);

if let Some(s) = scores.get("Alice") { println!("Alice: {s}"); }
let s = scores.get("Charlie").copied().unwrap_or(0);

scores.remove("Bob");
let n = scores.len();
let has = scores.contains_key("Alice");

for (k, v) in &scores { println!("{k}: {v}"); }
```

Особое внимание — **entry API**, идиоматичный способ «вставить или обновить»:

```rust
*scores.entry("Charlie".into()).or_insert(0) += 5;
scores.entry("Dave".into()).or_insert_with(|| compute_default());
```

`entry` делает один поиск по таблице вместо двух (`get` + `insert`) — это и быстрее, и безопасно с точки зрения borrow checker.

### Создание HashMap из итератора

```rust
let m: HashMap<&str, i32> = vec![("a", 1), ("b", 2), ("c", 3)]
    .into_iter()
    .collect();

let words = ["one", "two", "three"];
let lengths: HashMap<&str, usize> = words.iter()
    .map(|w| (*w, w.len()))
    .collect();
```

### HashSet и BTreeSet

```rust
use std::collections::HashSet;

let a: HashSet<i32> = [1, 2, 3, 4].into_iter().collect();
let b: HashSet<i32> = [3, 4, 5, 6].into_iter().collect();

let inter: HashSet<i32> = a.intersection(&b).copied().collect();   // {3, 4}
let uni:   HashSet<i32> = a.union(&b).copied().collect();           // {1..=6}
let diff:  HashSet<i32> = a.difference(&b).copied().collect();      // {1, 2}
```

`BTreeSet` поддерживает диапазонные запросы:

```rust
use std::collections::BTreeSet;
let s: BTreeSet<i32> = (1..=20).collect();
for n in s.range(5..10) { print!("{n} "); }   // 5 6 7 8 9
```

### VecDeque и BinaryHeap

```rust
use std::collections::{VecDeque, BinaryHeap};

let mut q: VecDeque<i32> = VecDeque::new();
q.push_back(1); q.push_back(2);
q.push_front(0);
println!("{:?}", q.pop_front());

let mut heap = BinaryHeap::new();    // max-heap по умолчанию
heap.push(3); heap.push(1); heap.push(4); heap.push(1);
while let Some(x) = heap.pop() { print!("{x} "); }   // 4 3 1 1
```

Для min-heap используют `std::cmp::Reverse`: `heap.push(Reverse(3))`.

### Итераторы — сердце Rust

Итератор — это любой тип, реализующий трейт:

```rust
trait Iterator {
    type Item;
    fn next(&mut self) -> Option<Self::Item>;
    // плюс десятки методов с реализациями по умолчанию
}
```

Только один обязательный метод — `next`. На нём построены десятки адаптеров.

### Три способа получить итератор из коллекции

```rust
let v = vec![1, 2, 3];

for x in v.iter()     { /* x: &i32 */ }       // заимствует
for x in v.iter_mut() { /* x: &mut i32 */ }   // мутабельно
for x in v.into_iter() { /* x: i32 */ }       // забирает владение
```

`for x in v` неявно вызывает `v.into_iter()` и **перемещает** `v`. После цикла `v` использовать нельзя — это самая частая путаница новичков.

### Ленивость и terminal operations

Адаптеры **ленивы**: `map`, `filter`, `take`, `skip`, `zip`, `chain` сами по себе ничего не делают. Они строят описание вычисления. Чтобы получить результат, нужен **terminal operation**: `collect`, `sum`, `product`, `for_each`, `count`, `fold`, `reduce`, `any`, `all`, цикл `for`, `min`, `max`.

```rust
let v = vec![1, 2, 3, 4, 5];
let evens_squared: Vec<i32> = v.iter()
    .filter(|n| **n % 2 == 0)
    .map(|n| n * n)
    .collect();
// [4, 16]
```

### Самые полезные адаптеры

```rust
let v = vec![1, 2, 3, 4, 5];

let doubled: Vec<i32> = v.iter().map(|x| x * 2).collect();
let evens: Vec<i32>   = v.iter().copied().filter(|x| x % 2 == 0).collect();
let first3: Vec<i32>  = v.iter().take(3).copied().collect();
let after2: Vec<i32>  = v.iter().skip(2).copied().collect();
let chained: Vec<i32> = v.iter().chain([100, 200].iter()).copied().collect();
let zipped: Vec<(i32, char)> = v.iter().zip(['a','b','c'].iter()).map(|(n, c)| (*n, *c)).collect();
let with_idx: Vec<(usize, i32)> = v.iter().enumerate().map(|(i, x)| (i, *x)).collect();
let unique: Vec<i32>  = v.iter().copied().collect::<std::collections::HashSet<_>>().into_iter().collect();
```

### Reductions: sum, product, fold, reduce

```rust
let v = vec![1, 2, 3, 4];
let sum: i32 = v.iter().sum();
let prod: i32 = v.iter().product();
let count = v.iter().count();
let max = v.iter().max();              // Option<&i32>
let min = v.iter().min();
let any_neg = v.iter().any(|x| *x < 0);
let all_pos = v.iter().all(|x| *x > 0);

// fold: общий случай свёртки с начальным значением
let s = v.iter().fold(0, |acc, x| acc + x);

// reduce: свёртка без начального значения, возвращает Option
let max = v.iter().copied().reduce(i32::max);
```

`fold` — самый общий: с его помощью можно реализовать любую свёртку, включая `sum` и `max`.

### Группировки и более сложное

```rust
use std::collections::HashMap;

let words = ["apple", "ant", "banana", "berry", "cherry"];

// сгруппировать по первой букве
let groups: HashMap<char, Vec<&str>> = words.iter()
    .fold(HashMap::new(), |mut acc, w| {
        let key = w.chars().next().unwrap();
        acc.entry(key).or_insert_with(Vec::new).push(*w);
        acc
    });

for (k, v) in &groups {
    println!("{k}: {v:?}");
}
```

`itertools`-крейт даёт `group_by`, `chunks`, `tuples` и десятки других удобных адаптеров — стоит подключить в любой реальный проект.

### collect и turbofish

`collect` — самый перегруженный метод. Он умеет собирать в любой тип, реализующий `FromIterator`. Иногда компилятору нужно подсказать тип:

```rust
let xs = (0..5).collect::<Vec<_>>();                              // turbofish ::<>
let s: String = vec!['h','i'].into_iter().collect();
let m: HashMap<&str, i32> = vec![("a", 1), ("b", 2)].into_iter().collect();
let result: Result<Vec<i32>, _> = ["1", "2", "x"].iter().map(|s| s.parse()).collect();
```

Последняя строка — особенно полезная фишка: `Iterator<Item = Result<T, E>>` собирается в `Result<Vec<T>, E>`. На первой ошибке итерация прекращается.

### Ленивые цепочки и производительность

Итераторы компилируются в тот же машинный код, что и ручные циклы — это **zero-cost abstraction**. Не бойтесь длинных цепочек: `rustc` + LLVM их инлайнят и часто векторизуют.

Что **не** zero-cost: `collect` в `Vec` (выделяет память), `HashMap` (хеширование), создание `Box<dyn Iterator>` (динамика).

### Реализация своего итератора

```rust
struct Fib { curr: u64, next: u64 }

impl Iterator for Fib {
    type Item = u64;
    fn next(&mut self) -> Option<u64> {
        let cur = self.curr;
        self.curr = self.next;
        self.next = cur + self.curr;
        Some(cur)
    }
}

fn main() {
    let f = Fib { curr: 0, next: 1 };
    let first_ten: Vec<u64> = f.take(10).collect();
    println!("{:?}", first_ten);
}
```

Реализовав `next`, вы автоматически получаете все адаптеры (`map`, `filter`, `take` и т.д.) бесплатно.

### Iterator vs IntoIterator

- `Iterator` — то, что уже умеет `next`.
- `IntoIterator` — то, что умеет превратиться в итератор. Реализован для `Vec`, массивов, `HashMap`, диапазонов и т.д.

`for x in collection` под капотом — это `for x in collection.into_iter()`. Поэтому `for x in vec` забирает `vec`, а `for x in &vec` — нет (`&Vec` тоже реализует `IntoIterator`, и его `into_iter` даёт `iter()`).

## Практика

### Задача 1. Сумма квадратов чётных

```rust
fn sum_even_sq(xs: &[i32]) -> i32 {
    xs.iter().filter(|x| **x % 2 == 0).map(|x| x * x).sum()
}

fn main() {
    println!("{}", sum_even_sq(&[1, 2, 3, 4, 5, 6]));   // 56
}
```

### Задача 2. Частота слов

```rust
use std::collections::HashMap;

fn word_frequency(text: &str) -> HashMap<String, u32> {
    text.split_whitespace().fold(HashMap::new(), |mut acc, w| {
        *acc.entry(w.to_lowercase()).or_insert(0) += 1;
        acc
    })
}

fn top_n(freq: &HashMap<String, u32>, n: usize) -> Vec<(String, u32)> {
    let mut pairs: Vec<_> = freq.iter().map(|(k, v)| (k.clone(), *v)).collect();
    pairs.sort_by(|a, b| b.1.cmp(&a.1).then(a.0.cmp(&b.0)));
    pairs.into_iter().take(n).collect()
}

fn main() {
    let text = "the quick brown fox jumps over the lazy dog the fox is quick";
    let freq = word_frequency(text);
    for (word, count) in top_n(&freq, 3) {
        println!("{word}: {count}");
    }
}
```

### Задача 3. Безопасный парсинг с collect Result

```rust
fn parse_all(strs: &[&str]) -> Result<Vec<i32>, std::num::ParseIntError> {
    strs.iter().map(|s| s.parse::<i32>()).collect()
}

fn main() {
    println!("{:?}", parse_all(&["1", "2", "3"]));      // Ok([1, 2, 3])
    println!("{:?}", parse_all(&["1", "x", "3"]));      // Err
}
```

### Задача 4. Группировка по первой букве

```rust
use std::collections::HashMap;

fn group_by_first(words: &[&str]) -> HashMap<char, Vec<String>> {
    words.iter().fold(HashMap::new(), |mut acc, w| {
        if let Some(c) = w.chars().next() {
            acc.entry(c.to_ascii_lowercase()).or_insert_with(Vec::new).push(w.to_string());
        }
        acc
    })
}

fn main() {
    let words = ["apple", "ant", "banana", "berry", "cherry", "blueberry"];
    let g = group_by_first(&words);
    let mut keys: Vec<&char> = g.keys().collect();
    keys.sort();
    for k in keys { println!("{k}: {:?}", g[k]); }
}
```

### Задача 5. VecDeque как очередь

```rust
use std::collections::VecDeque;

fn process_jobs(jobs: Vec<&str>) {
    let mut q: VecDeque<&str> = VecDeque::from(jobs);
    while let Some(job) = q.pop_front() {
        println!("обрабатываю: {job}");
        if job == "fail" {
            q.push_back("retry");
        }
    }
}

fn main() {
    process_jobs(vec!["a", "b", "fail", "c"]);
}
```

### Задача 6. BinaryHeap для топ-K

```rust
use std::collections::BinaryHeap;
use std::cmp::Reverse;

fn top_k(values: &[i32], k: usize) -> Vec<i32> {
    let mut heap: BinaryHeap<Reverse<i32>> = BinaryHeap::with_capacity(k + 1);
    for &v in values {
        heap.push(Reverse(v));
        if heap.len() > k { heap.pop(); }
    }
    let mut result: Vec<i32> = heap.into_iter().map(|Reverse(v)| v).collect();
    result.sort_by(|a, b| b.cmp(a));
    result
}

fn main() {
    println!("{:?}", top_k(&[5, 1, 9, 3, 7, 4, 2, 8, 6], 3));   // [9, 8, 7]
}
```

### Задача 7. Свой итератор

```rust
struct Counter { count: u32, max: u32 }

impl Iterator for Counter {
    type Item = u32;
    fn next(&mut self) -> Option<u32> {
        if self.count < self.max {
            self.count += 1;
            Some(self.count)
        } else { None }
    }
}

fn main() {
    let c = Counter { count: 0, max: 5 };
    let s: u32 = c.zip(0u32..).map(|(a, b)| a * b).sum();
    println!("{s}");          // 1*0 + 2*1 + 3*2 + 4*3 + 5*4 = 40
}
```

### Задача 8. Пересечение HashSet

```rust
use std::collections::HashSet;

fn common_words(a: &str, b: &str) -> Vec<String> {
    let sa: HashSet<String> = a.split_whitespace().map(|s| s.to_lowercase()).collect();
    let sb: HashSet<String> = b.split_whitespace().map(|s| s.to_lowercase()).collect();
    let mut common: Vec<String> = sa.intersection(&sb).cloned().collect();
    common.sort();
    common
}

fn main() {
    let r = common_words("the rust programming language", "the python language is great");
    println!("{r:?}");
}
```

## Тест

**1.** Что напечатает код?

```rust
let v = vec![1, 2, 3];
let r: Vec<i32> = v.iter().map(|x| x * 2).collect();
println!("{:?} {:?}", v, r);
```

**2.** А этот?

```rust
let v = vec![1, 2, 3];
for x in v {}
println!("{:?}", v);
```

**3.** В чём разница между `HashMap` и `BTreeMap`? Когда выбрать `BTreeMap`?

**4.** Что вернёт?

```rust
let v = vec![1, 2, 3, 4, 5];
let r = v.iter().filter(|&&x| x > 2).count();
```

**5.** Как идиоматично «вставить, если ключа нет, иначе увеличить»?

**6.** Что выведет программа?

```rust
let v = vec![1, 2, 3, 4, 5];
let r: Result<Vec<i32>, _> = v.iter().map(|x| {
    if *x == 3 { Err("плохо") } else { Ok(x * 10) }
}).collect();
println!("{:?}", r);
```

**7.** Чем отличаются `fold` и `reduce`?

**8.** Что напечатает код?

```rust
let s: i32 = (1..=10).filter(|x| x % 2 == 0).sum();
println!("{s}");
```

**9.** Зачем нужен `std::cmp::Reverse` и приведите пример его использования.

**10.** Что произойдёт при таком коде?

```rust
let mut v = vec![1, 2, 3];
let it = v.iter();
v.push(4);
for x in it { println!("{x}"); }
```

---

### Ответы

1. `[1, 2, 3] [2, 4, 6]`. `v.iter()` заимствует `v` иммутабельно, поэтому после `collect` оригинал доступен. `r` — новый вектор удвоенных значений.

2. Не скомпилируется. `for x in v` неявно вызывает `v.into_iter()` и забирает владение. После цикла `v` мёртв. Нужно `for x in &v` или `for x in v.iter()`.

3. `HashMap` — O(1) среднее, **порядок не гарантирован**, требует `Hash + Eq`. `BTreeMap` — O(log n), **отсортирован по ключу**, требует `Ord`. `BTreeMap` нужен, когда: важен детерминированный обход; нужны диапазонные запросы (`map.range(a..b)`); ключ плохо хешируется или сравнивается дешевле.

4. `3`. Элементы `3, 4, 5` проходят фильтр. Двойная ссылка `&&x` потому, что `v.iter()` даёт `&i32`, а замыкание в `filter` принимает ещё одну ссылку — итого `&&i32`, который мы деструктурируем в `x: i32`.

5. `*map.entry(key).or_insert(0) += 1;`. `entry` — идиоматичный API: одно обращение к таблице вместо двух. Если ключа нет, `or_insert(0)` вставляет `0` и возвращает `&mut V`; затем `*... += 1` инкрементирует.

6. `Err("плохо")`. При сборе `Iterator<Item = Result<...>>` в `Result<Vec<...>, _>` итерация прекращается на первой ошибке, и весь результат становится `Err` с этой ошибкой.

7. `fold(init, f)` принимает начальное значение и всегда возвращает результат типа `B` (даже если итератор пуст — вернётся `init`). `reduce(f)` использует первый элемент как начальное значение и возвращает `Option<T>` — `None` если итератор пуст, `Some(value)` иначе. `reduce` подходит, когда тип элементов и тип результата совпадают.

8. `30`. Чётные от 1 до 10: `2, 4, 6, 8, 10`, их сумма `30`.

9. `Reverse(T)` инвертирует порядок сравнения для типа, реализующего `Ord`. Главный сценарий — превратить `BinaryHeap` (max-heap по умолчанию) в min-heap: `heap.push(Reverse(value))`. Также удобен в `sort_by_key` для сортировки по убыванию: `v.sort_by_key(|x| Reverse(x.priority))`.

10. Не скомпилируется. `v.iter()` создаёт иммутабельную ссылку на `v`. `v.push(4)` требует `&mut self` и может реаллоцировать буфер, инвалидируя итератор. Borrow-checker отклонит код на строке `v.push`, потому что итератор `it` ещё используется в цикле ниже.
# Урок 11. Коллекции и итераторы

## Теория

Коллекции в std живут в куче и владеют своими данными. Когда коллекция дропается — дропаются и элементы. Никакого GC, всё детерминировано.

### Главные коллекции

- `Vec<T>` — динамический массив, подряд в памяти. Доступ O(1), push/pop с конца O(1) амортизированно.
- `HashMap<K, V>` — хеш-таблица. Доступ O(1) в среднем. Порядок не гарантирован.
- `HashSet<T>` — множество без дубликатов.
- `BTreeMap<K, V>` / `BTreeSet<T>` — отсортированные по ключу. Операции O(log n).
- `VecDeque<T>` — двусторонняя очередь, push/pop с обеих сторон O(1).

Выбор коллекции важнее, чем кажется. Нужен порядок? `BTreeMap`. Нужна очередь? `VecDeque`. По умолчанию — `Vec`/`HashMap`.

### Создание и базовые операции

```rust
let mut v: Vec<i32> = Vec::new();
v.push(1); v.push(2); v.push(3);

let v2 = vec![1, 2, 3]; // макрос

use std::collections::HashMap;
let mut scores = HashMap::new();
scores.insert("Alice", 90);
scores.insert("Bob", 85);

// Безопасный доступ: возвращает Option
if let Some(s) = scores.get("Alice") {
    println!("Alice: {s}");
}

// entry API — идиоматичный способ "вставить или обновить"
*scores.entry("Charlie").or_insert(0) += 5;
```

### Итераторы — сердце Rust

Итератор — это любой тип, реализующий `Iterator`:

```rust
trait Iterator {
    type Item;
    fn next(&mut self) -> Option<Self::Item>;
}
```

Всего один метод. На нём построены десятки адаптеров: `map`, `filter`, `take`, `skip`, `zip`, `enumerate`, `chain`, `flat_map`, `peekable`, `fold`, `reduce`, `collect`, `sum`, `count`, `any`, `all` и т. д.

### Три способа получить итератор

```rust
let v = vec![1, 2, 3];

for x in v.iter()    { /* x: &i32   — заимствует */ }
for x in v.iter_mut(){ /* x: &mut i32 — изменяет */ }
for x in v           { /* x: i32     — забирает владение */ }
```

`for x in v` неявно вызывает `v.into_iter()` и **перемещает** `v`. После цикла `v` использовать нельзя. Это самая частая путаница новичков.

### Ленивость

Итераторы ленивые. Сами по себе `map`/`filter` ничего не делают — они строят описание вычисления. Чтобы получить результат, нужен **terminal operation**: `collect`, `sum`, `for_each`, цикл `for` и т. д.

```rust
let nums = vec![1, 2, 3, 4, 5];
let evens_sq: Vec<i32> = nums.iter()
    .filter(|n| *n % 2 == 0)
    .map(|n| n * n)
    .collect();
// [4, 16]
```

### `collect` и turbofish

`collect` — самый перегруженный метод. Он умеет собирать в любой тип, реализующий `FromIterator`. Иногда компилятору нужно подсказать тип:

```rust
let xs = (0..5).collect::<Vec<_>>(); // turbofish ::<>
let s: String = vec!['h','i'].into_iter().collect();
let m: HashMap<&str, i32> = vec![("a", 1), ("b", 2)].into_iter().collect();
```

### Производительность

Итераторы компилируются в тот же машинный код, что и ручные циклы — это **zero-cost abstraction**. Не бойтесь длинных цепочек: `rustc` + LLVM их инлайнят и векторизуют.

## Практика

Анализатор слов: читаем строку, считаем частоту слов, выводим топ-N.

```rust
use std::collections::HashMap;

fn word_frequency(text: &str) -> HashMap<String, u32> {
    let mut freq = HashMap::new();
    for word in text.split_whitespace() {
        let key = word.to_lowercase();
        *freq.entry(key).or_insert(0) += 1;
    }
    freq
}

fn top_n(freq: &HashMap<String, u32>, n: usize) -> Vec<(String, u32)> {
    let mut pairs: Vec<_> = freq.iter()
        .map(|(k, v)| (k.clone(), *v))
        .collect();
    pairs.sort_by(|a, b| b.1.cmp(&a.1).then(a.0.cmp(&b.0)));
    pairs.into_iter().take(n).collect()
}

fn main() {
    let text = "the quick brown fox jumps over the lazy dog the fox is quick";
    let freq = word_frequency(text);
    for (word, count) in top_n(&freq, 3) {
        println!("{word}: {count}");
    }
}
```

Бонус: перепишите `word_frequency` через `fold`:

```rust
fn word_frequency_fold(text: &str) -> HashMap<String, u32> {
    text.split_whitespace().fold(HashMap::new(), |mut acc, w| {
        *acc.entry(w.to_lowercase()).or_insert(0) += 1;
        acc
    })
}
```

## Тест

**1.** Что напечатает код?
```rust
let v = vec![1, 2, 3];
let r: Vec<i32> = v.iter().map(|x| x * 2).collect();
println!("{:?} {:?}", v, r);
```

**2.** А этот?
```rust
let v = vec![1, 2, 3];
for x in v {}
println!("{:?}", v);
```

**3.** В чём разница между `HashMap` и `BTreeMap`? Когда нужен `BTreeMap`?

**4.** Что вернёт?
```rust
let v = vec![1, 2, 3, 4, 5];
let r = v.iter().filter(|&&x| x > 2).count();
```

**5.** Как идиоматично «вставить, если ключа нет, иначе увеличить»?

---

### Ответы

**1.** `[1, 2, 3] [2, 4, 6]`. `v.iter()` заимствует, `v` остаётся доступным.

**2.** Не скомпилируется. `for x in v` вызывает `v.into_iter()` и забирает владение. `v` после цикла мёртв. Нужно `for x in &v` или `v.iter()`.

**3.** `HashMap` — O(1) в среднем, **порядок не гарантирован**. `BTreeMap` — O(log n), элементы упорядочены по ключу. `BTreeMap` нужен когда: нужен детерминированный обход, диапазонные запросы (`range(..)`), или ключ — что-то, что не хешируется хорошо.

**4.** `3`. Элементы 3, 4, 5. Двойная ссылка `&&x` — потому что `v.iter()` даёт `&i32`, а `filter` отдаёт ещё одну ссылку.

**5.** `*map.entry(key).or_insert(0) += 1;`. `entry` — идиоматичный API: одно обращение к таблице вместо двух.
