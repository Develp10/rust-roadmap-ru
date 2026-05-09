# Урок 14. Асинхронное программирование: `async`/`await` и `tokio`

## Теория

### Зачем нужен async

В синхронном коде поток блокируется на I/O — ждёт ответ от сети или диска, ничего не делая. Чтобы обслуживать тысячи соединений, пришлось бы создавать тысячи потоков (дорого: 8 МБ стека на поток + переключения контекста).

Async позволяет одному потоку обслуживать тысячи задач, переключаясь между ними когда они ждут I/O.

### Что такое `async fn`

`async fn` — это **сахар** для функции, возвращающей `impl Future<Output = T>`.

```rust
async fn fetch() -> String {
    "data".to_string()
}
// эквивалентно
fn fetch() -> impl Future<Output = String> { /* ... */ }
```

`Future` — описание ленивого вычисления. **Само по себе ничего не выполняется.** Чтобы запустить, нужен runtime.

### `.await` — точка переключения

`.await` говорит: «приостанови меня, пока этот future не готов; runtime может в это время выполнять другие задачи».

```rust
async fn handler() {
    let data = fetch().await;        // приостановились, ждём
    let parsed = parse(data).await;  // снова приостановились
    println!("{parsed}");
}
```

Между `.await` runtime может переключиться на другую задачу.

### Runtime — `tokio`

Стандартная библиотека не предоставляет executor. Самый популярный — `tokio`.

`Cargo.toml`:
```toml
[dependencies]
tokio = { version = "1", features = ["full"] }
```

```rust
#[tokio::main]
async fn main() {
    println!("hello, async!");
}
```

`#[tokio::main]` — макрос, разворачивающийся в синхронный `main`, который создаёт runtime и запускает в нём ваш future.

### `spawn` — параллельные задачи

```rust
#[tokio::main]
async fn main() {
    let h1 = tokio::spawn(async {
        tokio::time::sleep(std::time::Duration::from_millis(100)).await;
        "first"
    });
    let h2 = tokio::spawn(async {
        tokio::time::sleep(std::time::Duration::from_millis(50)).await;
        "second"
    });
    let (a, b) = (h1.await.unwrap(), h2.await.unwrap());
    println!("{a}, {b}");
}
```

Обе задачи выполняются параллельно. Общее время ≈ 100 мс, не 150.

### `join!` и `select!`

- **`tokio::join!`** — ждать завершения нескольких future параллельно.
- **`tokio::select!`** — реагировать на тот, что завершится первым (timeout, отмена).

```rust
use tokio::time::{sleep, Duration};

async fn slow() -> i32 { sleep(Duration::from_secs(2)).await; 42 }

async fn with_timeout() {
    tokio::select! {
        v = slow() => println!("got {v}"),
        _ = sleep(Duration::from_secs(1)) => println!("timeout!"),
    }
}
```

⚠️ **Cancellation safety**: при `select!` отменённая ветка может оставить состояние в незавершённом виде. Не вызывайте в `select!` future, у которых это критично (например, цепочки чтения из сокета).

### Блокирующий код в async

Если в `async fn` вызвать `std::thread::sleep` или тяжёлый CPU-bound код — вы заблокируете весь runtime-поток. Используйте:

- **`tokio::time::sleep`** — async-версия `sleep`.
- **`tokio::task::spawn_blocking`** — для синхронных вызовов (файловые системные API, тяжёлые вычисления).

```rust
let result = tokio::task::spawn_blocking(|| {
    expensive_cpu_computation()
}).await.unwrap();
```

### Каналы в tokio

`tokio::sync::mpsc` — асинхронный mpsc (`send().await` приостанавливает, если канал переполнен).
`tokio::sync::oneshot` — для одного значения (request → response).
`tokio::sync::broadcast` — несколько подписчиков, каждый получает свою копию.
`tokio::sync::watch` — состояние, которое меняется и наблюдается.

### Send + 'static

`tokio::spawn` требует, чтобы future был `Send + 'static`. Это значит: всё, что захвачено замыканием, должно быть передано по владению (`move`) и не содержать non-`Send` (например, `Rc`, `RefCell`).

### Не путайте: async ≠ многопоточность

Async — это про **конкурентность** (много задач на одном/нескольких потоках). Многопоточность — про **параллелизм** (одновременное выполнение). `tokio` по умолчанию мультитредовый, но вы можете запустить и в одном потоке: `#[tokio::main(flavor = "current_thread")]`.

## Практика

Конкурентный загрузчик: качаем 10 URL параллельно, ограничивая параллелизм до 3.

`Cargo.toml`:
```toml
[dependencies]
tokio = { version = "1", features = ["full"] }
reqwest = { version = "0.12", features = ["rustls-tls"], default-features = false }
futures = "0.3"
```

```rust
use futures::stream::{self, StreamExt};
use std::time::Instant;

async fn fetch(client: &reqwest::Client, url: &str) -> Result<usize, reqwest::Error> {
    let bytes = client.get(url).send().await?.bytes().await?;
    Ok(bytes.len())
}

#[tokio::main]
async fn main() {
    let urls = vec![
        "https://www.rust-lang.org",
        "https://crates.io",
        "https://docs.rs",
        "https://blog.rust-lang.org",
        "https://this-week-in-rust.org",
    ];

    let client = reqwest::Client::new();
    let start = Instant::now();

    let results: Vec<_> = stream::iter(urls)
        .map(|url| {
            let client = &client;
            async move { (url, fetch(client, url).await) }
        })
        .buffer_unordered(3) // максимум 3 параллельно
        .collect()
        .await;

    for (url, res) in results {
        match res {
            Ok(size) => println!("{url}: {size} bytes"),
            Err(e)   => println!("{url}: error {e}"),
        }
    }
    println!("done in {:?}", start.elapsed());
}
```

Запустите. Сравните время с последовательным вариантом (просто `for url in urls { ... }`).

## Тест

**1.** Что произойдёт?
```rust
async fn say() { println!("hi"); }
fn main() { say(); }
```

**2.** Скомпилируется ли?
```rust
use std::rc::Rc;
#[tokio::main]
async fn main() {
    let r = Rc::new(0);
    tokio::spawn(async move { println!("{}", r); });
}
```

**3.** В чём разница между `tokio::time::sleep` и `std::thread::sleep` внутри async?

**4.** Что выведет?
```rust
#[tokio::main]
async fn main() {
    let (tx, mut rx) = tokio::sync::mpsc::channel(8);
    tokio::spawn(async move {
        for i in 0..3 { tx.send(i).await.unwrap(); }
    });
    while let Some(v) = rx.recv().await {
        println!("{v}");
    }
}
```

**5.** Когда выбрать `tokio::spawn`, а когда обычный `tokio::join!`?

---

### Ответы

**1.** Программа напечатает... ничего. Она скомпилируется (с warning), но `say()` возвращает `Future`, который никогда не запускается. Future ленивый. Нужен runtime: `#[tokio::main] async fn main() { say().await; }`.

**2.** Не скомпилируется. `Rc` не реализует `Send`, а `tokio::spawn` требует `Send + 'static`. Решение: `Arc`.

**3.** `std::thread::sleep` блокирует **весь поток runtime**, замораживая другие async-задачи. `tokio::time::sleep` приостанавливает только текущий future, отдавая управление runtime для выполнения других задач.

**4.** `0`, `1`, `2`. Канал закрывается, когда `tx` дропается (после завершения spawn-задачи), и `recv()` начинает возвращать `None`, выходя из цикла.

**5.** `tokio::spawn` — задача выполняется **независимо** на runtime, можно `.await` потом или забыть. Подходит для долгоживущих задач (worker'ы, фоновые обработчики). `tokio::join!` — параллельно ждём несколько future **в текущей задаче**, не требуя `Send + 'static`. Дешевле `spawn` и работает с не-`Send` типами.
