FROM rustlang/rust:nightly-alpine AS rust-nightly-with-rust-src
# That's for humility and its deps
RUN apk add make libc-dev libusb-dev libftdi1-dev openssl openssl-dev
# Cortex-M3, qemu
RUN rustup target add thumbv7m-none-eabi
# Nucleo-H7
RUN rustup target add thumbv7em-none-eabi
# RISC-V ESP32-C3
RUN rustup target add riscv32i-unknown-none-elf
# x86 bare metal
RUN rustup target add x86_64-unknown-none
# WebAssembly
RUN rustup target add wasm32-unknown-unknown
## this also sets it per-directory sometimes--be careful
RUN rustup default nightly-2022-08-03
RUN rustup component add rust-src
## not always available
#RUN rustup component add miri
RUN rustup component add clippy-preview
#RUN rustup component add llvm-tools
#^ --toolchain nightly-2021-08-01-x86_64-unknown-linux-gnu
#RUN cargo install evcxr_repl # >= v0.5.1
RUN cargo install cargo-audit
RUN cargo install cargo-expand
RUN cargo install cargo-fuzz
RUN cargo install cargo-outdated
RUN cargo install cargo-geiger
RUN cargo install afl
#RUN cargo install llvm-tools-preview # broken
RUN cargo install cargo-binutils
RUN cargo install flamegraph
RUN cargo install cargo-with
RUN cargo install wasm-bindgen-cli
RUN cargo install espflash
RUN cargo install cargo-out-dir
RUN cargo install cargo-bundle
RUN cargo install grcov
#RUN cargo install rustpython
#RUN cargo install cargo-llvm-cov # requires llvm-tools-preview--which is broken
RUN chmod -R a+rwX /usr/local/cargo/registry
