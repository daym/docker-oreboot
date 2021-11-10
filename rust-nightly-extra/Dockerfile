FROM rustlang/rust:nightly AS rust-nightly-with-rust-src
#RUN rustup override set nightly-2020-10-25
# this also sets it per-directory sometimes--be careful
RUN rustup override set nightly-2021-09-01
RUN rustup component add rust-src
RUN rustup component add miri
RUN rustup component add clippy-preview
#RUN rustup component add llvm-tools
#^ --toolchain nightly-2021-08-01-x86_64-unknown-linux-gnu
#RUN cargo install evcxr_repl # >= v0.5.1
RUN cargo install cargo-expand
RUN cargo install cargo-fuzz
RUN cargo install afl
#RUN cargo install llvm-tools-preview # broken
RUN cargo install cargo-binutils
RUN cargo install flamegraph
ADD oreboot /oreboot
RUN cd /oreboot \
&& egrep 'toolchain|channel|components' rust-toolchain | sed 's;"llvm-tools-preview", ;;'  >R \
&& mv R rust-toolchain \
&& umask 000 \
&& make OBJCOPY=objcopy -C src/mainboard/amd/romecrb || true \
&& (cd tools/layoutflash && cargo build --target x86_64-unknown-linux-gnu --release) || true \
&& (cd tools/layoutflash && cargo build --target x86_64-unknown-linux-gnu) || true \
&& chmod -R +rwX /usr/local/cargo/registry \
&& rm -rf /oreboot
