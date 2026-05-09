# Урок 13. Многопоточность: потоки, Send/Sync, каналы

## Теория

Rust даёт **fearless concurrency** — компилятор гарантирует отсутствие data races на этапе компиляции. Это не магия: всё построено на типах `Send` и `Sync`.

### Запуск потока

```rust
use std::thread;

let h = thread::spawn(|| {
    for i in 0..5 {
        println!("from thread: {i}");
    }
});

for i in 0..5 {
    println!("from main: {i}");
}

h.join().unwrap(); // ждём завершения
```

`spawn` принимает замыкание, возвращает `JoinHandle<T>`. Без `join` поток может прерваться на середине, когда main завершится.

### Перенос данных в поток: `move`

```rust
let data = vec![1, 2, 3];
thread::spawn(move || {
    println!("{:?}", data);
}).join().unwrap();
```

Без `move` замыкание попыталось бы заимствовать `data`, но компилятор не знает, переживёт ли `data` поток. `move` явно перемещает владение.

### `Send` и `Sync` — ключ ко всему

- **`Send`** — тип можно **переместить** между потоками. Реализуется автоматически почти для всех типов. Не реализуют: `Rc<T>`, сырые указатели.
- **`Sync`** — на тип можно **разделять ссылку** между потоками. `T: Sync` ⇔ `&T: Send`. Не реализуют: `Cell`, `RefCell` (внутренняя мутабельность без синхронизации).

Эти трейты — **auto-traits**: компилятор сам их выводит на основе полей структуры. Достаточно, чтобы хотя бы одно поле было `!Send`, и вся структура не `Send`.

### Каналы — `mpsc`

Multi-producer, single-consumer. Идиоматичный способ передавать данные между потоками без `Mutex`.

```rust
use std::sync::mpsc;
use std::thread;

let (tx, rx) = mpsc::channel();

for i in 0..3 {
    let tx = tx.clone();
    thread::spawn(move || {
        tx.send(i * 10).unwrap();
    });
}
drop(tx); // закрываем последний sender

for value in rx {
    println!("got {value}");
}
```

`for value in rx` блокируется, пока не пришлёт сообщение, и завершается, когда **все** `tx` дропнуты.

Для двусторонних каналов и broadcast используйте `crossbeam-channel` или `flume`.

### Разделяемое состояние: `Arc<Mutex<T>>`

Когда несколько потоков должны изменять общие данные:

```rust
use std::sync::{Arc, Mutex};
use std::thread;

let counter = Arc::new(Mutex::new(0));
let mut handles = vec![];

for _ in 0..10 {
    let c = Arc::clone(&counter);
    handles.push(thread::spawn(move || {
        let mut n = c.lock().unwrap();
        *n += 1;
    }));
}
for h in handles { h.join().unwrap(); }
println!("итог: {}", *counter.lock().unwrap()); // 10
```

`lock()` возвращает `MutexGuard` — RAII-страж. Когда выйдет из scope, mutex автоматически освободится.

`RwLock` — для сценариев «много чтения, редкая запись».

### Атомики — для счётчиков и флагов

Без блокировок:

```rust
use std::sync::atomic::{AtomicUsize, Ordering};
use std::sync::Arc;
use std::thread;

let counter = Arc::new(AtomicUsize::new(0));
let mut hs = vec![];
for _ in 0..10 {
    let c = Arc::clone(&counter);
    hs.push(thread::spawn(move || c.fetch_add(1, Ordering::Relaxed)));
}
for h in hs { h.join().unwrap(); }
println!("{}", counter.load(Ordering::Relaxed));
```

`Ordering` управляет memory ordering — это глубокая тема (см. книгу «Rust Atomics and Locks» от Mara Bos). `Relaxed` для счётчиков, `Acquire`/`Release` для флагов готовности, `SeqCst` — самый строгий, дороже всех.

### Параллелизм данных: `rayon`

```rust
use rayon::prelude::*;

let v: Vec<i32> = (0..1_000_000).collect();
let sum: i64 = v.par_iter().map(|&x| x as i64 * x as i64).sum();
```

Меняем `iter()` на `par_iter()` — и получаем многопоточную обработку.

### Tipический выбор инструмента

- **Каналы** — независимые задачи, передающие сообщения.
- **`Mutex`** — общее состояние, которое читается и пишется случайно.
- **`RwLock`** — много читателей, редкие писатели.
- **Atomic** — простые счётчики/флаги.
- **`rayon`** — обработать большой набор данных.

## Практика

Параллельный word-counter: считаем слова из нескольких строк в одной `HashMap`.

```rust
use std::collections::HashMap;
use std::sync::{Arc, Mutex};
use std::thread;

fn main() {
    let texts = vec![
        "rust is fast and safe",
        "rust is fast",
        "safe systems programming",
    ];

    let result = Arc::new(Mutex::new(HashMap::<String, u32>::new()));
    let mut handles = vec![];

    for text in texts {
        let result = Arc::clone(&result);
        let text = text.to_string();
        handles.push(thread::spawn(move || {
            let mut local = HashMap::<String, u32>::new();
            for w in text.split_whitespace() {
                *local.entry(w.to_string()).or_insert(0) += 1;
            }
            // объединяем в общую карту
            let mut shared = result.lock().unwrap();
            for (k, v) in local {
                *shared.entry(k).or_insert(0) += v;
            }
        }));
    }
    for h in handles { h.join().unwrap(); }

    let final_map = result.lock().unwrap();
    let mut pairs: Vec<_> = final_map.iter().collect();
    pairs.sort_by(|a, b| b.1.cmp(a.1));
    for (w, c) in pairs.iter().take(5) {
        println!("{w}: {c}");
    }
}
```

Обратите внимание: каждый поток **сначала считает локально**, потом мержит результат под mutex. Это паттерн «map-reduce» — минимизируем contention.

## Тест

**1.** Скомпилируется ли?
```rust
use std::rc::Rc;
use std::thread;
let data = Rc::new(vec![1, 2, 3]);
let d = Rc::clone(&data);
thread::spawn(move || println!("{:?}", d));
```

**2.** Что делает `MutexGuard`, когда выходит из scope?

**3.** Зачем нужен `move` перед замыканием в `thread::spawn`?

**4.** Как закрыть mpsc-канал так, чтобы `for v in rx` завершился?

**5.** Когда выбрать `AtomicUsize`, а когда `Mutex<usize>`?

---

### Ответы

**1.** Не скомпилируется. `Rc` не реализует `Send`, потому что его счётчик не атомарный — две копии в разных потоках могли бы повредить счётчик. Решение: `Arc`.

**2.** Освобождает mutex (`unlock`). Это RAII: lock'и не теряются, даже если случилась паника.

**3.** Без `move` замыкание попытается захватить переменные по ссылке. Компилятор не может доказать, что переменные переживут поток (они на стеке текущей функции, поток мог бы пережить функцию). `move` перемещает владение в замыкание, делая поток самодостаточным.

**4.** Дропнуть **все** `Sender`'ы (`tx`). Когда последний sender дропается, канал закрывается, и `rx` итератор завершается.

**5.** `AtomicUsize` — для одиночных операций (`fetch_add`, `load`, `store`), без блокировок, очень быстро. `Mutex<usize>` — если нужна сложная логика «прочитать → проверить → записать» атомарно. Атомики не композируются: «увеличить, если меньше N» через atomic — это уже compare-and-swap loop.
