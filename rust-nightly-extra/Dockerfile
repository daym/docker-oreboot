FROM rustlang/rust:nightly-alpine AS rust-nightly-with-rust-src
# That's for humility and its deps
RUN apk add make libc-dev libusb-dev libftdi1-dev openssl openssl-dev
#RUN apk add gcc-arm-linux-gnueabihf
RUN apk add gcc-arm-none-eabi
# Used by hubris dist
RUN apk add git
# Required by cargo-release--but probably not by cargo smart-release
#RUN apk add openssh-client

# Used by humility; alternative: libudev-zero https://github.com/illiliti/libudev-zero/
RUN apk add eudev-dev
# Cortex-M3, qemu
RUN rustup target add thumbv7m-none-eabi
# Nucleo-H7
RUN rustup target add thumbv7em-none-eabi
# Raspberry Pi
RUN rustup target add armv7-unknown-linux-gnueabihf
# RISC-V ESP32-C3
RUN rustup target add riscv32i-unknown-none-elf
# x86 bare metal
RUN rustup target add x86_64-unknown-none
# WebAssembly
RUN rustup target add wasm32-unknown-unknown
## this also sets it per-directory sometimes--be careful
#RUN rustup default nightly-2022-08-03
RUN rustup component add rust-src
## not always available
#RUN rustup component add miri
RUN rustup component add clippy-preview
#RUN rustup component add llvm-tools
#^ --toolchain nightly-2021-08-01-x86_64-unknown-linux-gnu
RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install cargo-audit
RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install cargo-expand
RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install cargo-fuzz
RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install cargo-outdated
RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install cargo-geiger
RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install afl
#RUN cargo install llvm-tools-preview # broken
RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install cargo-binutils
RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install flamegraph
RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install cargo-with
RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install wasm-bindgen-cli
RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install espflash
RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install cargo-out-dir
RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install cargo-bundle
RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install grcov
# humility needs that
RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install cargo-readme
# Benchmarking
RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install hyperfine
# Required by cargo-release
RUN apk --no-cache add ca-certificates \
    && rm -rf /var/cache/apk/*
# Required by cargo-release
RUN update-ca-certificates
# Required by cargo-release (really, openssl-sys)
RUN apk add perl
# Releasing
RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install cargo-release
# Changelog generation
RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install git-cliff
RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install cargo-semver-checks
RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install cargo-dist
RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install release-plz
RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install cargo-smart-release
# --no-default-features
RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install --force cargo-make
#RUN cargo install cargo-xtask
#RUN cargo install rustpython
#RUN cargo install cargo-llvm-cov # requires llvm-tools-preview--which is broken
# won't work in musl (does install, though):
#RUN cargo install evcxr_repl # >= v0.5.1

#RUN chmod -R a+rwX /usr/local/cargo/registry
