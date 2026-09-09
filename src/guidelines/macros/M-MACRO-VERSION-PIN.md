<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Pin supporting proc macro crates (M-MACRO-VERSION-PIN) { #M-MACRO-VERSION-PIN }

<why>keep generated code compatible with its library</why>

This guideline applies to libraries that re-export macros from supporting proc macro crates
where users depend on the main crate, not the supporting crates. Independently consumed macro
libraries are outside its scope.

A newer macro may generate code that needs types or helpers unavailable in an older library.
Release the main crate and its supporting crates together, with matching version numbers.

The main crate must depend on the exact matching version of its proc macro crate. If there
is a separate macro implementation crate, pin that dependency too. Use `=1.2.3`, not `1.2.3`, 
which allows compatible updates.

Update all package versions and their pins on each release, even if some crates have no code changes.
Choose the version bump based on the main crate's public API, including its macros. Users do not need
to exact-pin their dependency on the main crate.

For example:

```rust,ignore
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
