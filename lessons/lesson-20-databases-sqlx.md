# Урок 20. Работа с базами данных: sqlx и diesel

## Теория

### Экосистема Rust для баз данных

В Rust несколько подходов к работе с БД:

| Библиотека | Тип | Особенности |
|-----------|-----|-------------|
| sqlx | Async, SQL-first | Проверка SQL во время компиляции, поддержка Postgres/MySQL/SQLite |
| diesel | Sync/Async, ORM | Сильная типизация, миграции, schema.rs |
| sea-orm | Async ORM | Активная разработка, удобный API |
| tokio-postgres | Async, низкий уровень | Чистый клиент Postgres |
| rusqlite | Sync | Тонкая обёртка над SQLite |

### sqlx — основы

```toml
[dependencies]
sqlx = { version = "0.7", features = ["runtime-tokio", "postgres", "macros", "chrono", "uuid"] }
tokio = { version = "1", features = ["full"] }
```

```rust
use sqlx::{PgPool, Row};

#[tokio::main]
async fn main() -> Result<(), sqlx::Error> {
    let pool = PgPool::connect("postgres://user:pass@localhost/db").await?;

    let row = sqlx::query("SELECT 1 + 1 AS sum")
        .fetch_one(&pool)
        .await?;
    let sum: i32 = row.get("sum");
    println!("{}", sum);
    Ok(())
}
```

### Compile-time проверка запросов

```rust
#[derive(sqlx::FromRow)]
struct User { id: i64, name: String }

let users = sqlx::query_as!(User, "SELECT id, name FROM users WHERE id = $1", 1i64)
    .fetch_all(&pool).await?;
```

Макросы `query!` и `query_as!` подключаются к БД во время компиляции и проверяют SQL. Для CI используется `cargo sqlx prepare`, генерирующий offline-кэш в `.sqlx/`.

### Миграции

```bash
sqlx migrate add create_users
sqlx migrate run
```

В коде:

```rust
sqlx::migrate!("./migrations").run(&pool).await?;
```

### Транзакции

```rust
let mut tx = pool.begin().await?;
sqlx::query("INSERT INTO accounts(id, balance) VALUES($1, $2)")
    .bind(1).bind(100).execute(&mut *tx).await?;
sqlx::query("UPDATE accounts SET balance = balance - 10 WHERE id = $1")
    .bind(1).execute(&mut *tx).await?;
tx.commit().await?;
```

### Diesel — синхронный ORM

```rust
use diesel::prelude::*;

#[derive(Queryable, Selectable)]
#[diesel(table_name = users)]
struct User { id: i32, name: String }

let conn = &mut PgConnection::establish("postgres://...")?;
let result = users::table.filter(users::id.eq(1)).load::<User>(conn)?;
```

### Connection pool

`PgPool` (sqlx) или `r2d2` (diesel) — пул переиспользуемых соединений. Создавайте один pool на приложение, клонируйте Arc-обёртку (внутри уже Arc).

## Практика

### 1. CRUD users в Postgres (sqlx)

```rust
use sqlx::PgPool;

#[derive(sqlx::FromRow, Debug)]
struct User { id: i64, name: String, email: String }

async fn create_user(pool: &PgPool, name: &str, email: &str) -> sqlx::Result<i64> {
    let row = sqlx::query("INSERT INTO users(name,email) VALUES($1,$2) RETURNING id")
        .bind(name).bind(email)
        .fetch_one(pool).await?;
    Ok(row.get("id"))
}

async fn get_user(pool: &PgPool, id: i64) -> sqlx::Result<Option<User>> {
    sqlx::query_as::<_, User>("SELECT id,name,email FROM users WHERE id=$1")
        .bind(id).fetch_optional(pool).await
}

async fn list_users(pool: &PgPool) -> sqlx::Result<Vec<User>> {
    sqlx::query_as::<_, User>("SELECT id,name,email FROM users ORDER BY id")
        .fetch_all(pool).await
}

async fn update_email(pool: &PgPool, id: i64, email: &str) -> sqlx::Result<u64> {
    let r = sqlx::query("UPDATE users SET email=$1 WHERE id=$2")
        .bind(email).bind(id).execute(pool).await?;
    Ok(r.rows_affected())
}

async fn delete_user(pool: &PgPool, id: i64) -> sqlx::Result<u64> {
    let r = sqlx::query("DELETE FROM users WHERE id=$1")
        .bind(id).execute(pool).await?;
    Ok(r.rows_affected())
}
```

### 2. SQLite — встраиваемая БД

```rust
use sqlx::sqlite::SqlitePoolOptions;

let pool = SqlitePoolOptions::new()
    .max_connections(5)
    .connect("sqlite:app.db").await?;

sqlx::query("CREATE TABLE IF NOT EXISTS notes(id INTEGER PRIMARY KEY, text TEXT)")
    .execute(&pool).await?;
```

### 3. Stream больших результатов

```rust
use futures::StreamExt;

let mut stream = sqlx::query_as::<_, User>("SELECT id,name,email FROM users")
    .fetch(&pool);
while let Some(user) = stream.next().await {
    let u = user?;
    println!("{:?}", u);
}
```

### 4. Перевод средств в транзакции

```rust
async fn transfer(pool: &PgPool, from: i64, to: i64, amount: i64) -> sqlx::Result<()> {
    let mut tx = pool.begin().await?;
    sqlx::query("UPDATE accounts SET balance = balance - $1 WHERE id = $2")
        .bind(amount).bind(from).execute(&mut *tx).await?;
    sqlx::query("UPDATE accounts SET balance = balance + $1 WHERE id = $2")
        .bind(amount).bind(to).execute(&mut *tx).await?;
    tx.commit().await?;
    Ok(())
}
```

### 5. Repository-pattern

```rust
#[async_trait::async_trait]
trait UserRepo {
    async fn find(&self, id: i64) -> sqlx::Result<Option<User>>;
    async fn save(&self, name: &str, email: &str) -> sqlx::Result<i64>;
}

struct PgUserRepo { pool: PgPool }

#[async_trait::async_trait]
impl UserRepo for PgUserRepo {
    async fn find(&self, id: i64) -> sqlx::Result<Option<User>> {
        sqlx::query_as::<_, User>("SELECT id,name,email FROM users WHERE id=$1")
            .bind(id).fetch_optional(&self.pool).await
    }
    async fn save(&self, name: &str, email: &str) -> sqlx::Result<i64> {
        let r = sqlx::query("INSERT INTO users(name,email) VALUES($1,$2) RETURNING id")
            .bind(name).bind(email).fetch_one(&self.pool).await?;
        Ok(r.get("id"))
    }
}
```

### 6. Diesel пример

```rust
diesel::insert_into(users::table)
    .values((users::name.eq("Alice"), users::email.eq("a@x.io")))
    .execute(conn)?;
```

## Тест

1. Чем `query!` отличается от `query()`?
2. Зачем нужен `sqlx prepare`?
3. Как создать пул соединений к Postgres?
4. Как откатить транзакцию в sqlx?
5. Что делает `fetch_optional`?
6. Что такое `Queryable` в diesel?
7. Какой драйвер использовать для SQLite в sqlx?
8. Зачем нужен `#[derive(FromRow)]`?
9. В чём разница sqlx vs diesel?
10. Как запустить миграции в коде sqlx?

---

### Ответы

1. `query!` — макрос с проверкой SQL во время компиляции через подключение к БД; `query()` — функция времени выполнения.
2. Сохранить метаданные запросов в `.sqlx/` для оффлайн-сборки в CI.
3. `PgPool::connect("...").await?` или `PgPoolOptions::new().max_connections(5).connect(...)`.
4. Не вызывать `commit()`; на drop `Transaction` откатывается автоматически, либо явно `tx.rollback().await?`.
5. Возвращает `Option<T>` — `None`, если строка не найдена, без ошибки.
6. Трейт diesel для типов, которые можно загружать из результатов запроса.
7. `sqlite` (feature `sqlite`), URL вида `sqlite:file.db`.
8. Чтобы автоматически отображать строки результата в структуру по именам полей.
9. sqlx — async, SQL-first, compile-time проверка запросов; diesel — традиционный ORM с DSL и schema.rs.
10. `sqlx::migrate!("./migrations").run(&pool).await?;`.
