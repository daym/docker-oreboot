FROM rustlang/rust:nightly-alpine AS rust-nightly-with-rust-src
# That's for humility and its deps
RUN apk add make libc-dev libusb-dev libftdi1-dev openssl openssl-dev openssl-libs-static
# does not exist: RUN apk add gcc-arm-linux-gnueabihf
# linker is broken: RUN apk add gcc-arm-none-eabi
# Used by hubris dist
RUN apk add git
# Required by cargo-release and cargo smart-release
RUN apk add openssh-client

# Used by humility; alternative: libudev-zero https://github.com/illiliti/libudev-zero/
RUN apk add eudev-dev

RUN rustup default nightly
#-2022-08-03

# Cortex-M3, qemu
RUN rustup target add thumbv7m-none-eabi

# Nucleo-H7
RUN rustup target add thumbv7em-none-eabi
# Raspberry Pi
RUN rustup target add armv7-unknown-linux-gnueabihf
# Kobo Mini; although why not armv7-unknown-linux-gnueabihf?
RUN rustup target add armv7a-none-eabi
#armv7-unknown-linux-musleabihf
# RISC-V ESP32-C3
RUN rustup target add riscv32i-unknown-none-elf
# x86 bare metal
RUN rustup target add x86_64-unknown-none
# WebAssembly
RUN rustup target add wasm32-unknown-unknown
RUN rustup target add aarch64-linux-android

# RP2350
RUN rustup target add thumbv8m.main-none-eabihf
RUN rustup target add riscv32imac-unknown-none-elf

# Windows
RUN rustup target add x86_64-pc-windows-gnu

## this also sets it per-directory sometimes--be careful
RUN rustup component add rust-src
RUN rustup component add rust-analyzer
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
# disappeared RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install afl
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
# Build failure (cmake missing or something in another package)
#RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install cargo-semver-checks
# broken: RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install cargo-dist  error[E0599]: the method `as_dyn_error` exists for reference `&ParseError`, but its trait bounds were not satisfied    = note: the following trait bounds were not satisfied:            `cargo_dist_schema::target_lexicon::ParseError: StdError`
RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install release-plz
# Optional for cargo-smart-release; github access
RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install gh
# Required by cargo-smart-release
RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install bat
# Now uses libz-ng-sys which uses cmake
#RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install cargo-smart-release
# --no-default-features
RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install --force cargo-make
#RUN cargo install cargo-xtask
#RUN cargo install rustpython
#RUN cargo install cargo-llvm-cov # requires llvm-tools-preview--which is broken
# won't work in musl (does install, though):
#RUN cargo install evcxr_repl # >= v0.5.1

#RUN chmod -R a+rwX /usr/local/cargo/registry

# Add dependency trust checker
RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install --locked cackle

# Inspect rustc LLVM optimization remarks
# See <https://kobzol.github.io/rust/cargo/2023/08/12/rust-llvm-optimization-remarks.html>.
# The intermediate languages (HIR, MIR) render them a bit too far removed from the Rust source code.
RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install cargo-remark

# Allow using external toolchain
RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install cross

# Manual vetting of crates
RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install cargo-vet

# Find unused dependencies
RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install cargo-udeps
RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install cargo-deny

# Find junk in your executables
RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install cargo-bloat

# Generate full call graph
RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install cargo-call-stack

# Show assembly, LLVM-IR, MIR
RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install cargo-show-asm

# emacs rustic uses that
RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install cargo-edit

# lsp
#RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install rust-analyzer
#$ git clone https://github.com/rust-lang/rust-analyzer.git && cd rust-analyzer
#$ cargo xtask install --server

# cargo-unused-features linker error
# TODO: Rudra Rust Memory Safety & Undefined Behavior Detection (old)
# TODO: Rustviz lifetime visualization https://github.com/rustviz/rustviz

RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install cargo-apk
RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install cargo-machete

# TODO: cargo-cache

# For yaiouom
RUN RUSTFLAGS=-Ctarget-feature=-crt-static rustup component add rustc-dev
#RUN RUSTFLAGS=-Ctarget-feature=-crt-static cargo install yaiouom-checker
# Update stable toolchain (otherwise, it's really old--like 2024)
RUN RUSTFLAGS=-Ctarget-feature=-crt-static rustup toolchain install stable-x86_64-unknown-linux-musl

RUN rustup default stable

