<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Pin supporting proc macro crates (M-MACRO-VERSION-PIN) { #M-MACRO-VERSION-PIN }

<why>keep generated code compatible with its library</why>

A crate that re-exports macros from a companion proc macro crate must pin that dependency
to its own exact version (`=x.y.z`). This also applies to any separate macro implementation
crate. Release these crates together with the same version number, even if some crates have
no code changes.

Without exact pins, Cargo may upgrade the macro crate independently of the main crate.
The newer macro may then generate code that uses types or helpers added in a newer library
release, breaking compilation even when those additions were semver compatible.

M-MACRO-VERSION-PIN does not apply to independently consumed macro libraries.

Example:

```toml
# my_crate/Cargo.toml
[package]
version = "1.2.3"

[dependencies]
my_crate_macros = "=1.2.3"

# my_crate_macros/Cargo.toml
[package]
version = "1.2.3"

[dependencies]
my_crate_macros_impl = "=1.2.3"

# my_crate_macros_impl/Cargo.toml
[package]
version = "1.2.3"
```
