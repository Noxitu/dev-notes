# Glossary

 - Crate - fancy name for single 
 - Cargo - a build system

# Simple application

`main.rs`:

    fn main() {
        println!("Hello World!");
    }

Can be compiled using:

    rustc main.rs

# Cargo

Create new package directory (defaults to binary executable):

    cargo new <app_name>

Package can be build using:

    cargo build
    cargo build --release

Note: This also initializes git repository. This can be disabled using `--vcs none`.

# Multiple files

Cargo builds all files in `src/`.

File `func.rs` can declare public symbol:

    pub fn my_func() {
        println!("Hello, world!");
    }

The other one now needs to use `mod func;` to import that file.

Such symbol can be used using full scope `func::my_func()` or imported into global scope `use func::*`.

# Library

Cargo project being an app or an library depends on existance of file `src/main.rs` or `src/lib.rs`. A project can also be created as a library using `--lib` argument, which also creates example function with a test.

Rust has no stable ABI - all dependencies need to be build while building a project. But this means there is no complexity involving dependencies. Consider 3 packages: `package1` available online, `package2` available online, but used from local repo clone, and unpublished `package3`. Following `Cargo.toml` works:

    [dependencies]
    package1 = "1.0"
    package2 = "1.0"
    package3 = { path = "../path_to_package3" }

    [patch.crates-io]
    package2 = { path = "../path_to_package2" }

# C Interoperability

## Produce a C library

Rust can produce C library output after adding following section to `Cargo.toml`:

    [lib]
    crate-type = ["cdylib", "staticlib"]

Additionally, exported functions must use:

    #[no_mangle]
    pub extern "C" fn func()

# Tests

Cargo provides convention for placing different tests:

 - documentation tests go as code snippets ("```") in docs ("///")
 - unit tests are in same files as code, inside `src/`
 - integration tests go in `tests/`
 - examples go in `examples/` (not run)
 - benchmarks go in `benches/` (not yet stable??)
