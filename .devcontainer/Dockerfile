# Start with the base image
FROM golang:1.23.1-bookworm AS node-dev

# Set HOME environment variable explicitly
ENV HOME=/root

# Install essential packages
RUN apt-get update && apt-get install -y \
    git \
    curl \
    build-essential \
    cmake \
    npm \
    golang \
    clang \
    make \
    gotestsum \
    wabt \
    lld-13 \
    python3 \
    && npm install --global yarn \
    && ln -s /usr/bin/wasm-ld-13 /usr/local/bin/wasm-ld

# Install Node.js using nvm
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash && \
    bash -c "source $HOME/.bashrc && nvm install 18 && nvm use 18"

#7 [4/6] RUN echo "$(whoami) " && echo "$(pwd)"
#7 0.098 root 
#7 0.099 /go
RUN echo "$(whoami) " && echo "$(pwd)"

# Install Rust
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
ENV PATH="$HOME/.cargo/bin:$PATH"

RUN rustup install 1.80.1 && \
    rustup default 1.80.1 && \
    rustup install nightly

RUN rustup target add wasm32-unknown-unknown --toolchain 1.80.1 && \
    # rustup target add wasm32-wasi --toolchain 1.80.1 && \
    rustup target add wasm32-unknown-unknown --toolchain nightly && \
    # rustup target add wasm32-wasi --toolchain nightly && \
    rustup component add rust-src --toolchain nightly && \
    cargo install cbindgen

# Install Foundry
RUN curl -L https://foundry.paradigm.xyz | bash

ENV PATH="$HOME/.foundry/bin:$PATH"

RUN bash -c "foundryup"
