# Serde Pickle Serialization Library

[![Build Status](https://github.com/spinorml/serde-pickle-rs/actions/workflows/main.yml/badge.svg)](https://github.com/spinorml/serde-pickle-rs/actions/workflows/main.yml)
[![Latest Version](https://img.shields.io/crates/v/serde-pickle-rs.svg)](https://crates.io/crates/serde-pickle-rs)

[Documentation](https://docs.rs/serde-pickle-rs)

This crate is a Rust library for parsing and generating Python pickle
streams, forked from [serde-pickle](https://github.com/birkenfeld/serde-pickle). That crate has not been updated in a while, and does not support some parts of the pickle spec (e.g. PERSID), which is required to parse the Meta LlaMA models.

It is built upon [Serde](https://github.com/serde-rs/serde), a high
performance generic serialization framework.

# Installation

This crate works with Cargo and can be found on
[crates.io](https://crates.io/crates/serde-pickle-rs) with a `Cargo.toml` like:

```toml
[dependencies]
serde = "1.0.192"
serde-pickle-rs = "1.0.1"
```

# Requirements

Minimum supported Rust version is 1.73.0.

# Usage

As with other serde serialization implementations, this library provides
toplevel functions for simple en/decoding of supported objects.

Example:

```rust
use std::collections::BTreeMap;

fn main() {
    let mut map = BTreeMap::new();
    map.insert("x".to_string(), 1.0);
    map.insert("y".to_string(), 2.0);

    // Serialize the map into a pickle stream.
    // The second argument are serialization options.
    let serialized = serde_pickle_rs::to_vec(&map, Default::default()).unwrap();

    // Deserialize the pickle stream back into a map.
    // Because we compare it to the original `map` below, Rust infers
    // the type of `deserialized` and lets serde work its magic.
    // The second argument are additional deserialization options.
    let deserialized = serde_pickle_rs::from_slice(&serialized, Default::default()).unwrap();
    assert_eq!(map, deserialized);
}
```

Serializing and deserializing structs and enums that implement the
serde-provided traits is supported, and works analogous to other crates
(using `serde_derive`).

