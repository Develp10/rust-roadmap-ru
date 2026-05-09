# Урок 19. Веб-разработка с axum

`axum` — это веб-фреймворк от команды tokio, построенный поверх `hyper` и `tower`. Он легковесный, идиоматичный и активно развивается. Если вы знаете `tokio` (урок 14) — освоить axum дело пары часов.

## Теория

### Зависимости

`Cargo.toml`:

```toml
[dependencies]
axum = "0.7"
tokio = { version = "1", features = ["full"] }
serde = { version = "1", features = ["derive"] }
serde_json = "1"
tower = "0.5"
tower-http = { version = "0.5", features = ["trace", "cors"] }
tracing = "0.1"
tracing-subscriber = "0.3"
```

### Минимальный сервер

```rust
use axum::{routing::get, Router};

#[tokio::main]
async fn main() {
    let app = Router::new().route("/", get(|| async { "Привет, axum!" }));

    let listener = tokio::net::TcpListener::bind("127.0.0.1:3000").await.unwrap();
    println!("listening on {}", listener.local_addr().unwrap());
    axum::serve(listener, app).await.unwrap();
}
```

`get(handler)` — фабрика, которая регистрирует обработчик HTTP GET. Хэндлером может быть любая `async fn` (или sync), реализующая трейт `Handler`.

### Что такое extractor

axum использует *extractors* — типы, которые «извлекают» что-то из запроса. Они автоматически вызываются перед хэндлером:

```rust
use axum::{extract::{Path, Query, Json}, http::StatusCode};
use serde::Deserialize;

#[derive(Deserialize)]
struct Filter { limit: Option<u32> }

async fn user_by_id(Path(id): Path<u64>) -> String {
    format!("user {id}")
}

async fn search(Query(f): Query<Filter>) -> String {
    format!("limit = {:?}", f.limit)
}

#[derive(Deserialize)]
struct CreateUser { name: String }

async fn create(Json(payload): Json<CreateUser>) -> (StatusCode, String) {
    (StatusCode::CREATED, format!("создан {}", payload.name))
}
```

Готовые экстракторы:

- `Path` — параметры пути.
- `Query` — query string.
- `Json` — JSON-тело.
- `Form` — `application/x-www-form-urlencoded`.
- `Headers` / `HeaderMap` — заголовки.
- `State` — общее состояние приложения.
- `Extension` — данные, добавленные middleware.

### Что такое response

Хэндлер возвращает любой тип, реализующий `IntoResponse`. Это:

- `String`, `&'static str` — text/plain.
- `(StatusCode, Body)` — статус + тело.
- `Json(value)` — JSON.
- `Html(value)` — HTML.
- `Result<T, E>` — где `E: IntoResponse` (для централизованной обработки ошибок).

### Состояние приложения

```rust
use std::sync::Arc;
use axum::{extract::State, routing::get, Router};

#[derive(Clone)]
struct AppState {
    db_url: Arc<String>,
}

async fn handler(State(state): State<AppState>) -> String {
    format!("DB: {}", state.db_url)
}

#[tokio::main]
async fn main() {
    let state = AppState { db_url: Arc::new("postgres://...".into()) };
    let app = Router::new()
        .route("/", get(handler))
        .with_state(state);
    // ...
}
```

`State` шарит данные по всем хэндлерам и должен быть `Clone`. Лучшая практика — обернуть тяжёлые ресурсы в `Arc`.

### Маршрутизация

```rust
let users = Router::new()
    .route("/", get(list_users).post(create_user))
    .route("/:id", get(get_user).delete(delete_user));

let app = Router::new()
    .nest("/api/users", users)
    .route("/health", get(|| async { "ok" }));
```

`nest` подключает подмаршрутизатор по префиксу. `merge` — объединяет маршрутизаторы без префикса.

### Обработка ошибок

Удобный паттерн: свой тип ошибки с реализацией `IntoResponse`:

```rust
use axum::{response::{IntoResponse, Response}, http::StatusCode, Json};
use serde_json::json;

pub enum AppError {
    NotFound(String),
    BadRequest(String),
    Internal(anyhow::Error),
}

impl IntoResponse for AppError {
    fn into_response(self) -> Response {
        let (status, msg) = match self {
            AppError::NotFound(m)   => (StatusCode::NOT_FOUND, m),
            AppError::BadRequest(m) => (StatusCode::BAD_REQUEST, m),
            AppError::Internal(e)   => (StatusCode::INTERNAL_SERVER_ERROR, e.to_string()),
        };
        (status, Json(json!({ "error": msg }))).into_response()
    }
}
```

Теперь хэндлеры могут использовать `Result<T, AppError>` и оператор `?`.

### Middleware и tower

axum использует абстракции `tower::Service` и `tower::Layer`. Готовые middleware:

```rust
use tower_http::{trace::TraceLayer, cors::CorsLayer};

let app = Router::new()
    .route("/", get(root))
    .layer(TraceLayer::new_for_http())
    .layer(CorsLayer::permissive());
```

Это даст логи каждого запроса и поддержку CORS.

### Тестирование axum-приложений

Есть удобный паттерн с `tower::ServiceExt`:

```rust
use axum::body::Body;
use axum::http::Request;
use tower::ServiceExt;

#[tokio::test]
async fn root_returns_hello() {
    let app = Router::new().route("/", get(|| async { "hi" }));
    let response = app
        .oneshot(Request::builder().uri("/").body(Body::empty()).unwrap())
        .await
        .unwrap();
    assert_eq!(response.status(), 200);
}
```

## Практика

Минимальный CRUD по in-memory списку задач:

```rust
use axum::{
    extract::{Path, State},
    http::StatusCode,
    response::IntoResponse,
    routing::{get, post},
    Json, Router,
};
use serde::{Deserialize, Serialize};
use std::{collections::HashMap, sync::{Arc, RwLock}};

#[derive(Clone, Default)]
struct AppState {
    tasks: Arc<RwLock<HashMap<u64, Task>>>,
    next_id: Arc<RwLock<u64>>,
}

#[derive(Clone, Serialize, Deserialize)]
struct Task {
    id: u64,
    title: String,
    done: bool,
}

#[derive(Deserialize)]
struct NewTask { title: String }

async fn list(State(s): State<AppState>) -> Json<Vec<Task>> {
    let tasks = s.tasks.read().unwrap();
    Json(tasks.values().cloned().collect())
}

async fn create(State(s): State<AppState>, Json(body): Json<NewTask>)
    -> (StatusCode, Json<Task>)
{
    let mut id_lock = s.next_id.write().unwrap();
    *id_lock += 1;
    let id = *id_lock;
    drop(id_lock);

    let task = Task { id, title: body.title, done: false };
    s.tasks.write().unwrap().insert(id, task.clone());
    (StatusCode::CREATED, Json(task))
}

async fn done(State(s): State<AppState>, Path(id): Path<u64>) -> impl IntoResponse {
    let mut tasks = s.tasks.write().unwrap();
    match tasks.get_mut(&id) {
        Some(t) => { t.done = true; (StatusCode::OK, Json(t.clone())).into_response() }
        None => (StatusCode::NOT_FOUND, "нет такой").into_response(),
    }
}

#[tokio::main]
async fn main() {
    let state = AppState::default();
    let app = Router::new()
        .route("/tasks", get(list).post(create))
        .route("/tasks/:id/done", post(done))
        .with_state(state);

    let listener = tokio::net::TcpListener::bind("127.0.0.1:3000").await.unwrap();
    println!("listening on http://{}", listener.local_addr().unwrap());
    axum::serve(listener, app).await.unwrap();
}
```

Запускаем сервер: `cargo run`. Проверим curl:

```bash
curl -s localhost:3000/tasks
curl -s -X POST localhost:3000/tasks \\
     -H 'Content-Type: application/json' \\
     -d '{"title":"купить хлеб"}'
curl -s -X POST localhost:3000/tasks/1/done
```

В реальном приложении `HashMap + RwLock` заменяется на БД (`sqlx`, `sea-orm`, `diesel`), а `anyhow::Error` — на свой тип ошибок.

## Тест

1. Что такое extractor в axum и приведите три примера встроенных экстракторов.
2. Какой трейт должен реализовать тип, чтобы быть возвращён из хэндлера?
3. Как поделиться общим состоянием (например, пулом БД) между всеми хэндлерами?
4. Чем `nest` отличается от `merge` при построении маршрутов?
5. Зачем заводить собственный тип ошибки и реализовывать для него `IntoResponse`?

---

### Ответы

1. Extractor — это тип-параметр в сигнатуре хэндлера, который axum использует, чтобы «вытащить» данные из запроса и передать в функцию. Примеры: `Path<T>` (параметры пути), `Query<T>` (query string), `Json<T>` (JSON-тело), `State<T>` (общее состояние), `HeaderMap` (заголовки).
2. Тип должен реализовывать трейт `IntoResponse`. Стандартная библиотека и сам axum предоставляют реализации для `String`, `&'static str`, `(StatusCode, Body)`, `Json<T>`, `Html<T>` и `Result<T, E>` где `E: IntoResponse`.
3. Через `State`: создаём структуру приложения (обычно `Clone` и с `Arc` внутри), регистрируем её через `Router::with_state(state)`, в хэндлерах принимаем `State(s): State<AppState>`. `Clone` дешёвый из-за `Arc`.
4. `nest("/api", router)` подключает поддерево маршрутов под префиксом `/api`. `merge(router)` объединяет маршруты на одном уровне, без префикса; полезно, когда отдельные модули регистрируют свои пути без общей приставки.
5. Свой тип ошибки централизует логику: один `impl IntoResponse` превращает любой вариант ошибки в корректный HTTP-ответ. Это даёт консистентный JSON-формат ошибок, единый код статуса для каждой категории и позволяет хэндлерам спокойно использовать `?` — все ошибки сами превратятся в HTTP-ответы.
