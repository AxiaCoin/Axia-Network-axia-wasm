# axia-wasm

Low-level WebAssembly format library.

[![Build Status](https://travis-ci.org/axia-tech/axia-wasm.svg?branch=master)](https://travis-ci.org/axia-tech/axia-wasm)
[![crates.io link](https://img.shields.io/crates/v/axia-wasm.svg)](https://crates.io/crates/axia-wasm)

[Documentation](https://docs.rs/axia-wasm)

## Rust WebAssembly format serializing/deserializing

Add to Cargo.toml

```toml
[dependencies]
axia-wasm = "0.42"
```

and then

```rust
let module = axia_wasm::deserialize_file("./res/cases/v1/hello.wasm").unwrap();
assert!(module.code_section().is_some());

let code_section = module.code_section().unwrap(); // Part of the module with functions code

println!("Function count in wasm file: {}", code_section.bodies().len());
```

## Wabt Test suite

`axia-wasm` supports full wabt testsuite (https://github.com/WebAssembly/testsuite), running asserts that involves deserialization.

To run testsuite:
- make sure you have all prerequisites to build `wabt` (since axia-wasm builds it internally using `wabt-rs`, see https://github.com/WebAssembly/wabt)
- checkout with submodules (`git submodule update --init --recursive`)
- run `cargo test --release --manifest-path=spec/Cargo.toml`

Decoder can be fuzzed with `cargo-fuzz` using `wasm-opt` (https://github.com/WebAssembly/binaryen):

- make sure you have all prerequisites to build `binaryen` and `cargo-fuzz` (`cmake` and a C++11 toolchain)
- checkout with submodules (`git submodule update --init --recursive`)
- install `cargo fuzz` subcommand with `cargo install cargo-fuzz`
- set rustup to use a nightly toolchain, because `cargo fuzz` uses a rust compiler plugin: `rustup override set nightly`
- run `cargo fuzz run deserialize`

## `no_std` crates

This crate has a feature, `std`, that is enabled by default. To use this crate
in a `no_std` context, add the following to your `Cargo.toml` (still requires allocator though):

```toml
[dependencies]
axia-wasm = { version = "0.41", default-features = false }
```

# License

`axia-wasm` is primarily distributed under the terms of both the MIT
license and the Apache License (Version 2.0), at your choice.

See LICENSE-APACHE, and LICENSE-MIT for details.

### Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in axia-wasm by you, as defined in the Apache-2.0 license, shall be
dual licensed as above, without any additional terms or conditions.
