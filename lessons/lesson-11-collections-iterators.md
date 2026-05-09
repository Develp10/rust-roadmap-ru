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
