# Урок 23. WebAssembly (WASM) на Rust

## Теория

### Что такое WebAssembly

WebAssembly — переносимый бинарный формат для выполнения кода в браузере и вне его (WASI). Особенности:

- быстрый и компактный
- безопасный (sandbox)
- запускается в браузерах, Node.js, Deno, Cloudflare Workers, Wasmtime/Wasmer
- мультиязычный (Rust, C/C++, Go, AssemblyScript, ...)

Rust — один из самых популярных языков для WASM благодаря отсутствию runtime и предсказуемой производительности.

### Цели сборки

| Target | Назначение |
|--------|-----------|
| `wasm32-unknown-unknown` | браузер, обычно через wasm-bindgen |
| `wasm32-wasi` | WASI-приложения вне браузера |
| `wasm32-unknown-emscripten` | Emscripten (для C/C++ совместимости) |

```bash
rustup target add wasm32-unknown-unknown
cargo build --target wasm32-unknown-unknown --release
```

### wasm-bindgen

`wasm-bindgen` — мост между Rust и JavaScript, генерирует JS-обёртки для функций и типов.

```toml
[lib]
crate-type = ["cdylib"]

[dependencies]
wasm-bindgen = "0.2"
```

```rust
use wasm_bindgen::prelude::*;

#[wasm_bindgen]
pub fn greet(name: &str) -> String {
    format!("Hello, {}!", name)
}

#[wasm_bindgen]
extern "C" {
    #[wasm_bindgen(js_namespace = console)]
    fn log(s: &str);
}
```

```bash
wasm-pack build --target web
```

В JS:

```js
import init, { greet } from './pkg/myapp.js';
await init();
console.log(greet('World'));
```

### Yew, Leptos, Dioxus

Frontend-фреймворки на Rust для WASM:

- Yew — Elm/React-подобный, виртуальный DOM
- Leptos — fine-grained reactivity, SSR, гидратация
- Dioxus — кроссплатформенный (web/desktop/mobile)

```rust
use leptos::*;

#[component]
fn Counter() -> impl IntoView {
    let (count, set_count) = create_signal(0);
    view! {
        <button on:click=move |_| set_count.update(|n| *n += 1)>
            "Count: " {count}
        </button>
    }
}
```

### WASI

WASI — стандартный системный API (файлы, env, время) для WASM вне браузера.

```bash
rustup target add wasm32-wasi
cargo build --target wasm32-wasi
wasmtime target/wasm32-wasi/debug/app.wasm
```

### Размер бинарника

Стратегии уменьшения:

```toml
[profile.release]
opt-level = "z"
lto = true
codegen-units = 1
panic = "abort"
strip = true
```

Дополнительно: `wasm-opt` (binaryen), `twiggy` для анализа.

### web-sys и js-sys

`js-sys` — биндинги к ECMAScript-API (`Object`, `Array`, `Promise`).
`web-sys` — биндинги к Web API (`Document`, `fetch`, `HtmlElement`, ...).

```rust
use web_sys::{window, console};

console::log_1(&"hello".into());
let doc = window().unwrap().document().unwrap();
let body = doc.body().unwrap();
body.set_inner_html("<h1>From Rust</h1>");
```

## Практика

### 1. Hello world (wasm-bindgen)

```rust
use wasm_bindgen::prelude::*;

#[wasm_bindgen]
pub fn add(a: i32, b: i32) -> i32 { a + b }

#[wasm_bindgen]
pub fn fib(n: u32) -> u64 {
    let (mut a, mut b) = (0u64, 1u64);
    for _ in 0..n { let c = a + b; a = b; b = c; }
    a
}
```

### 2. DOM-манипуляция через web-sys

```rust
#[wasm_bindgen(start)]
pub fn run() -> Result<(), JsValue> {
    let window = web_sys::window().ok_or(JsValue::from("no window"))?;
    let document = window.document().ok_or(JsValue::from("no doc"))?;
    let body = document.body().ok_or(JsValue::from("no body"))?;
    let h = document.create_element("h1")?;
    h.set_text_content(Some("Hello from Rust"));
    body.append_child(&h)?;
    Ok(())
}
```

### 3. Fetch API

```rust
use wasm_bindgen_futures::JsFuture;
use web_sys::{Request, RequestInit, RequestMode, Response};

#[wasm_bindgen]
pub async fn fetch_json(url: String) -> Result<JsValue, JsValue> {
    let mut opts = RequestInit::new();
    opts.method("GET").mode(RequestMode::Cors);
    let req = Request::new_with_str_and_init(&url, &opts)?;
    let win = web_sys::window().unwrap();
    let resp_value = JsFuture::from(win.fetch_with_request(&req)).await?;
    let resp: Response = resp_value.dyn_into()?;
    JsFuture::from(resp.json()?).await
}
```

### 4. Серверный WASI-инструмент

```rust
fn main() {
    let args: Vec<String> = std::env::args().collect();
    println!("got {} args", args.len());
    let now = std::time::SystemTime::now();
    println!("now = {:?}", now);
}
```

```bash
cargo build --target wasm32-wasi --release
wasmtime run target/wasm32-wasi/release/app.wasm a b c
```

### 5. Yew-компонент

```rust
use yew::prelude::*;

#[function_component]
fn App() -> Html {
    let counter = use_state(|| 0);
    let onclick = {
        let counter = counter.clone();
        move |_| counter.set(*counter + 1)
    };
    html! {
        <button {onclick}>{ format!("clicked {} times", *counter) }</button>
    }
}

fn main() { yew::Renderer::<App>::new().render(); }
```

### 6. Cloudflare Worker

```rust
use worker::*;

#[event(fetch)]
async fn main(req: Request, env: Env, _ctx: Context) -> Result<Response> {
    Response::ok("Hello from Rust on the edge!")
}
```

## Тест

1. Какой target собирает WASM для браузера через wasm-bindgen?
2. Зачем нужен `crate-type = ["cdylib"]`?
3. Что делает `wasm-pack build --target web`?
4. В чём разница `js-sys` и `web-sys`?
5. Что такое WASI?
6. Какой `opt-level` минимизирует размер бинарника?
7. Как импортировать `console.log` в Rust?
8. Как асинхронно вызвать JS Promise?
9. Какой фреймворк предлагает реактивность fine-grained?
10. Зачем нужен `wasm-opt`?

---

### Ответы

1. `wasm32-unknown-unknown`.
2. Чтобы Rust сгенерировал dynamic library, пригодную для линковки в WASM-модуль.
3. Сборка пакета и генерация JS-обёрток в `pkg/` для прямого импорта в браузере.
4. `js-sys` — биндинги к стандартным JS-объектам; `web-sys` — к Web API (DOM, fetch и т. д.).
5. WebAssembly System Interface — стандартный системный API для WASM-приложений вне браузера.
6. `opt-level = "z"` (или `"s"`).
7. `#[wasm_bindgen] extern "C" { #[wasm_bindgen(js_namespace = console)] fn log(s: &str); }`.
8. Через `wasm_bindgen_futures::JsFuture::from(promise).await`.
9. Leptos.
10. Дополнительно оптимизировать и сжать готовый `.wasm` через binaryen.
