# Ruxt
Ruxt is a Rust web framework written on top of Actix-web. It was written to make it easier to write plain HTML web applications in Rust, specifically utilizing the HTMX library.

This project is still in its early stages and is not recommended for production use.

## Installation
Add the following to your `Cargo.toml`:
```toml
[dependencies]
ruxt = "0.1.0"
```

## Current Features
- [x] Basic file-based routing

## Planned Features
- [ ] Dynamic routing
- [ ] JSX-like templating
- [ ] Middleware
- [ ] Static file serving
- [ ] Next.js-like layout system
- [ ] 404 handling
- [ ] Tailwind tooling integration
- [ ] CLI tooling for creating new projects
- [ ] Better error messaging

More to come!

Feel free to open an issue if you have any feature requests or suggestions.

## Current Limitations
The way the proc-macro is programmed does not account for a block inside the `HttpServer::new` function.

Meaning that this will work:
```rust
HttpServer::new(move || App::new().app_data(test_data.to_string()))
```

But this will not:
```rust
let server = HttpServer::new(move || {
    App::new().app_data(test_data.to_string())
});
```

This is a dumb limitation and I'm working on fixing it.

The routing system is also very basic and does not support any kind of dynamic routing. This is something I'm working on as well.

Basically, you can play with this, but shouldn't deploy anything with it yet.

## How it works
Ruxt uses a file-based routing system.

This is accomplished using a custom proc-macro that reads the contents of the `pages` directory and generates a new `main` function that creates a new Actix-web server with routes for each file in the `pages` directory.

## Usage
1. Create a new project with the following command:
```bash
cargo new --bin my_project
```
2. Create a new directory called `pages` in the root of your project.
3. Create a new file called `index.rs` in the `pages` directory.
4. Add the following code to `index.rs`:
```rust
use actix_web::{HttpResponse, Responder};
pub async fn index() -> impl Responder {
    HttpResponse::Ok().body("Hello, World!")
}
```
5. Add the following code to your `main.rs` file:
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
6. Run your project with the following command:
```bash
cargo run
```
7. Visit `http://localhost:8080` in your browser to see the output.

## License
This project is licensed under the MIT license.
