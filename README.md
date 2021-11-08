# rust-problem-matchers

GitHub Action to setup [Problem Matchers](https://github.com/actions/toolkit/blob/1cc56db0ff126f4d65aeb83798852e02a2c180c3/docs/problem-matchers.md) for Rust.

## Usage

Add this action as a step before running Rust toolchain:

```yaml
name: test

on:
  pull_request:
  push:
    branches:
      - main

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
        with:
          ref: ${{ github.event.pull_request.head.sha }}
      - uses: actions-rs/toolchain@v1
        with:
          toolchain: stable
          components: rustfmt, clippy
      - uses: r7kamura/rust-problem-matchers@v1
      - uses: actions-rs/cargo@v1
        name: cargo fmt
        with:
          command: fmt
          args: --all -- --check
      - uses: actions-rs/cargo@v1
        name: cargo build
        with:
          command: build
      - uses: actions-rs/cargo@v1
        name: cargo clippy
        with:
          command: clippy
          args: -- -D warnings
      - uses: actions-rs/cargo@v1
        name: cargo test
        with:
          command: test
```
