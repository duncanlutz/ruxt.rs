# Ruxt
Ruxt is a Rust web framework written on top of Actix-web. It was written to make it easier to write plain HTML web applications in Rust, specifically utilizing the HTMX library!

This project is still in its early stages and is not recommended for production use.

## Installation
Run the following command to add Ruxt to your project:
```bash
cargo add ruxt
```

Or add the following to your `Cargo.toml` file:
```toml
[dependencies]
ruxt = "0.1.2"
```

## Getting Started
### CLI Tool
To create a new Ruxt project, install the `create-ruxt-app` CLI tool with the following command:
```bash
cargo install create-ruxt-app
```

Then, create a new project with the following command:
```bash
create-ruxt-app {project_name}
```

This will create a new project with the following structure:
```
my_project
├── Cargo.toml
├── src
│   ├── main.rs
│   └── pages
│       ├── index.rs
│       └── mod.rs
```

To run your project, navigate to the root of your project and run the following command:
```bash
cargo run
```

## Basic Routing
The Ruxt `main` macro will automatically generate routes for files in the `routes` directory.
The routes are generated based on the file name, so a file named `index.rs` will be available at the root of the server.

The macro determines which HTTP verb to use based on the function name. For example, a function named `get` will be a `GET` route.

So for example:

```rust
// routes/index.rs
use actix_web::{web, HttpResponse, Responder};

pub async fn get() -> impl Responder {
    HttpResponse::Ok().body("Hello, World!")
}
```

Will be available as a GET request at `http://localhost:8080/`.

The following verbs are available for routing:
- `get`
- `post`
- `put`
- `patch`
- `delete`

## Dynamic Paths
Dynamic routes can be created by naming a folder or file with two leading underscores. For example, a folder named `__user` will create a dynamic route at `/user/{id}`.

```rust
// routes/__user.rs
use actix_web::{web, HttpResponse, Responder};

pub async fn post(id: web::Path<String>) -> impl Responder {
    HttpResponse::Ok().body(format!("Hello, {}!", id))
}
```

Will be available as a POST request at `http://localhost:8080/user/{id}`.

## Current Limitations
- As of now it is not possible to have a route with the name `mod` or `index` because of the way the macro generates routes. I'm looking into a solution for this.

## Current Features
- [x] Basic file-based routing
- [x] Dynamic routing

## Planned Features
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

## How it works
Ruxt uses a file-based routing system.

This is accomplished using a custom proc-macro that reads the contents of the `pages` directory and generates a new `main` function that creates a new Actix-web server with routes for each file in the `pages` directory.

## License
This project is licensed under the MIT license.
