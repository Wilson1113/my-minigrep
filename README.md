# minigrep

A small `grep` clone in Rust, built while working through Chapter 12 of *The Rust Programming Language*.

## Usage

```sh
cargo run -- <query> <file_path>

# case-insensitive search
IGNORE_CASE=1 cargo run -- to poem.txt
```

Results go to stdout, errors to stderr — so you can redirect them separately:

```sh
cargo run -- to poem.txt > output.txt
```

## What I learned

- **Splitting a binary into `main.rs` + `lib.rs`** — `main` handles CLI wiring and exit codes; all the searching logic lives in the library, which makes it testable.
- **Lifetimes** — `search<'a>(query: &str, content: &'a str) -> Vec<&'a str>` tells the compiler the returned slices borrow from `content`, not `query`.
- **Error handling without panics** — returning `Result` and using `unwrap_or_else` + `process::exit(1)` instead of `unwrap`/`expect`, plus `Box<dyn Error>` to bubble up different error types with `?`.
- **Iterators over indexing** — `Config::build` takes `impl Iterator<Item = String>` and consumes `env::args()` directly, so no cloning; `search` is a single `lines().filter().collect()` chain.
- **Environment variables** — `env::var("IGNORE_CASE")` for config that doesn't belong in an argument.
- **`stdout` vs `stderr`** — `println!` for results, `eprintln!` for errors, so piping output stays clean.
- **Test-driven development** — writing the failing test first, then the implementation.

## Tests

```sh
cargo test
```
