FROM rustlang/rust:nightly AS rust-nightly-with-rust-src
# this also sets it per-directory sometimes--be careful
RUN rustup override set nightly-2021-09-01
RUN rustup component add rust-src
