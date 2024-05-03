# Ruxt
Ruxt is a Rust web framework written on top of Actix-web. It was written to make it easier to write plain HTML web applications in Rust, specifically utilizing the HTMX library.

## Installation
Add the following to your `Cargo.toml`:
```toml
[dependencies]
ruxt = "0.1.0"
```

## Usage
```rust
#[ruxt::main]
async fn main() -> std::io::Result<()> {
   let test_data = "Hello, World!";
   HttpServer::new(move || App::new().app_data(test_data.to_string()))
   .bind(("0.0.0.0", 8080))?
   .run()
   .await
}
```

## License
This project is licensed under the MIT license.
