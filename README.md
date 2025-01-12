# jsonbox.rs

[![crates.io](https://img.shields.io/crates/v/jsonbox.svg)](https://crates.io/crates/jsonbox)
[![docs.rs](https://docs.rs/jsonbox/badge.svg)](https://docs.rs/jsonbox)
[![build](https://github.com/kuy/jsonbox-rs/workflows/build/badge.svg)](https://github.com/kuy/jsonbox-rs/actions)

⚙️ Rust wrapper for 📦 [jsonbox.io](https://jsonbox.io/).

## Usage

```rust
// Declaration
use jsonbox::{Client, Error};
use serde::{Deserialize, Serialize};

// Define struct
#[derive(Serialize, Deserialize)]
pub struct Data {
    pub name: String,
    pub message: String,
}

fn main() -> Result<(), Error> {
    // Create client with <BOX_ID>
    let client = Client::new("enjoy_your_first_jsonbox_rs");

    // Insert data
    let data = Data {
        name: "kuy".into(),
        message: "Hello, Jsonbox!".into(),
    };
    let (record, meta) = client.create(&data)?;
    println!("CREATE: data={:?}, id={} @{}", record, meta.id, meta.created_on);

    Ok(())
}
```

### CREATE

```rust
let data = Data {
    name: "kuy".into(),
    message: "Hello, Jsonbox!".into(),
};
let (record, meta) = client.create(&data)?;
println!("CREATE: data={:?}, meta={:?}", record, meta);
```

### READ

#### all (default parameters)

```rust
let all = client.read().all::<Data>()?;
println!("READ: len={}, all={:?}", all.len(), all);
```

#### with specific id

```rust
let (record, meta) = client.read().id("5d876d852a780700177c0557")?;
println!("READ: data={:?}, meta={:?}", record, meta);
```

#### with limit

```rust
let few = client.read().limit(10).run::<Data>()?;
println!("READ: len={}, few={:?}", few.len(), few);
```

#### with skip

```rust
let rest = client.read().skip(5).run::<Data>()?;
println!("READ: len={}, rest={:?}", rest.len(), rest);
```

#### with order (asc/desc)

```rust
let asc = client.read().order_by("name").run::<Data>()?;
println!("READ: len={}, asc={:?}", asc.len(), asc);

let desc = client.read().order_by("count").desc().run::<Data>()?;
println!("READ: len={}, desc={:?}", desc.len(), desc);
```

#### with filter

```rust
let filtered = client
    .read()
    .filter_by("name:{}", "Json Box")
    .run::<Data>()?;
println!("READ: len={}, filtered={:?}", filtered.len(), filtered);
```

See [baisc example](https://github.com/kuy/jsonbox-rs/blob/master/examples/basic.rs) or [official documentation](https://github.com/vasanthv/jsonbox#filtering) for more about filters.

### UPDATE

```rust
let data = Data::new("kuy", "Hello, Jsonbox!");
client.update("5d876d852a780700177c0557", &data)?;
println!("UPDATE: OK");
```

### DELETE

```rust
client.delete("5d876d852a780700177c0557")?;
println!("DELETE: OK");
```

## Examples

- [jsonbox-todo-example](https://github.com/kuy/jsonbox-todo-example)
- [hello](https://github.com/kuy/jsonbox-rs/blob/master/examples/hello.rs)
  - `cargo run --example hello`
- [basic](https://github.com/kuy/jsonbox-rs/blob/master/examples/basic.rs)
  - `cargo run --example basic`
- [errors](https://github.com/kuy/jsonbox-rs/blob/master/examples/errors.rs)
  - `cargo run --example errors`

## License

MIT

## Author

Yuki Kodama / [@kuy](https://twitter.com/kuy)
