# Урок 21. gRPC с tonic

## Теория

### Что такое gRPC

gRPC — RPC-фреймворк от Google поверх HTTP/2. Использует Protocol Buffers (protobuf) для описания сервисов и типов, поддерживает четыре вида вызовов:

| Тип | Клиент | Сервер |
|-----|--------|--------|
| Unary | один запрос | один ответ |
| Server streaming | один запрос | поток ответов |
| Client streaming | поток запросов | один ответ |
| Bidi streaming | поток | поток |

### tonic

`tonic` — основная gRPC-библиотека Rust поверх `tokio`/`hyper`/`prost`. Кодогенерация выполняется через `tonic-build` в `build.rs`.

```toml
[dependencies]
tonic = "0.11"
prost = "0.12"
tokio = { version = "1", features = ["full"] }

[build-dependencies]
tonic-build = "0.11"
```

### .proto файл

```proto
syntax = "proto3";
package hello;

service Greeter {
  rpc SayHello (HelloRequest) returns (HelloReply);
}

message HelloRequest { string name = 1; }
message HelloReply { string message = 1; }
```

### build.rs

```rust
fn main() -> Result<(), Box<dyn std::error::Error>> {
    tonic_build::compile_protos("proto/hello.proto")?;
    Ok(())
}
```

### Сервер

```rust
use tonic::{transport::Server, Request, Response, Status};

pub mod hello {
    tonic::include_proto!("hello");
}
use hello::greeter_server::{Greeter, GreeterServer};
use hello::{HelloRequest, HelloReply};

#[derive(Default)]
pub struct MyGreeter;

#[tonic::async_trait]
impl Greeter for MyGreeter {
    async fn say_hello(&self, req: Request<HelloRequest>) -> Result<Response<HelloReply>, Status> {
        let name = req.into_inner().name;
        Ok(Response::new(HelloReply { message: format!("Hello, {}!", name) }))
    }
}

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    Server::builder()
        .add_service(GreeterServer::new(MyGreeter::default()))
        .serve("0.0.0.0:50051".parse()?)
        .await?;
    Ok(())
}
```

### Клиент

```rust
use hello::greeter_client::GreeterClient;

let mut client = GreeterClient::connect("http://localhost:50051").await?;
let resp = client.say_hello(HelloRequest { name: "World".into() }).await?;
println!("{}", resp.into_inner().message);
```

### Streaming

```proto
service Numbers {
  rpc Range (RangeRequest) returns (stream Number);
}
```

```rust
type ResponseStream = Pin<Box<dyn Stream<Item = Result<Number, Status>> + Send>>;
async fn range(&self, req: Request<RangeRequest>) -> Result<Response<Self::RangeStream>, Status> {
    let RangeRequest { from, to } = req.into_inner();
    let s = futures::stream::iter((from..to).map(|n| Ok(Number { value: n })));
    Ok(Response::new(Box::pin(s)))
}
```

### Interceptors и метаданные

```rust
fn auth(req: Request<()>) -> Result<Request<()>, Status> {
    match req.metadata().get("authorization") {
        Some(t) if t == "Bearer secret" => Ok(req),
        _ => Err(Status::unauthenticated("no token")),
    }
}
let svc = GreeterServer::with_interceptor(MyGreeter::default(), auth);
```

### TLS

```rust
let cert = std::fs::read("server.pem")?;
let key = std::fs::read("server.key")?;
let identity = Identity::from_pem(cert, key);
Server::builder()
    .tls_config(ServerTlsConfig::new().identity(identity))?
    .add_service(svc).serve(addr).await?;
```

## Практика

### 1. Echo-сервис

```proto
service Echo { rpc Ping(PingMsg) returns (PingMsg); }
message PingMsg { string text = 1; int64 ts = 2; }
```

```rust
async fn ping(&self, req: Request<PingMsg>) -> Result<Response<PingMsg>, Status> {
    Ok(Response::new(req.into_inner()))
}
```

### 2. Server-streaming таймер

```rust
async fn ticks(&self, req: Request<TickReq>) -> Result<Response<Self::TicksStream>, Status> {
    let n = req.into_inner().count;
    let (tx, rx) = tokio::sync::mpsc::channel(8);
    tokio::spawn(async move {
        for i in 0..n {
            tx.send(Ok(Tick { value: i })).await.unwrap();
            tokio::time::sleep(std::time::Duration::from_millis(500)).await;
        }
    });
    Ok(Response::new(Box::pin(tokio_stream::wrappers::ReceiverStream::new(rx))))
}
```

### 3. Bidi-чат

```rust
async fn chat(&self, req: Request<tonic::Streaming<Msg>>) -> Result<Response<Self::ChatStream>, Status> {
    let mut input = req.into_inner();
    let (tx, rx) = tokio::sync::mpsc::channel(16);
    tokio::spawn(async move {
        while let Some(Ok(m)) = input.next().await {
            let _ = tx.send(Ok(Msg { text: format!("echo: {}", m.text) })).await;
        }
    });
    Ok(Response::new(Box::pin(ReceiverStream::new(rx))))
}
```

### 4. Health-check

```rust
let (mut health_reporter, health_service) = tonic_health::server::health_reporter();
health_reporter.set_serving::<GreeterServer<MyGreeter>>().await;
Server::builder().add_service(health_service).add_service(svc).serve(addr).await?;
```

### 5. Конвертация ошибок

```rust
impl From<sqlx::Error> for Status {
    fn from(e: sqlx::Error) -> Self {
        match e {
            sqlx::Error::RowNotFound => Status::not_found("not found"),
            other => Status::internal(other.to_string()),
        }
    }
}
```

### 6. Reflection (для grpcurl)

```rust
let reflection = tonic_reflection::server::Builder::configure()
    .register_encoded_file_descriptor_set(hello::FILE_DESCRIPTOR_SET)
    .build()?;
```

## Тест

1. Поверх какого протокола работает gRPC?
2. Какой формат сериализации использует gRPC?
3. Какие 4 типа вызовов поддерживает gRPC?
4. Зачем нужен `build.rs` с `tonic_build`?
5. Как пометить async-метод трейта `Greeter`?
6. Что возвращает `Request::into_inner()`?
7. Как реализовать server-streaming в tonic?
8. Что такое interceptor?
9. Как добавить TLS на сервере?
10. Какой код ошибки использовать, если запись не найдена?

---

### Ответы

1. HTTP/2.
2. Protocol Buffers (protobuf).
3. Unary, server streaming, client streaming, bidi streaming.
4. Сгенерировать Rust-код из `.proto` файлов во время сборки.
5. Атрибутом `#[tonic::async_trait]`.
6. Тело сообщения protobuf (распаковка `Request<T>` в `T`).
7. Возвращать `Pin<Box<dyn Stream<Item=Result<T, Status>> + Send>>` — например, через `mpsc::channel` + `ReceiverStream`.
8. Функция/closure, проверяющая/модифицирующая `Request` до диспетчеризации (например, авторизация).
9. `Server::builder().tls_config(ServerTlsConfig::new().identity(identity))?`.
10. `Status::not_found(...)`.
