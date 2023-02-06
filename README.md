# rust-base-image
A preconfigured base image to build WASI Services

In your Dockerfile 

```
FROM quay.io/knawd/rust-base:TAG as builder

WORKDIR /usr/src/app
COPY . .

# Install production dependencies and build a release artifact.
RUN cargo build --release --target wasm32-wasi

RUN wasmedgec target/wasm32-wasi/release/{{project-naum}}.wasm {{project-naum}}.wasm

FROM scratch

COPY --from=builder /usr/src/app/target/wasm32-wasi/release/{{project-name}}.wasm /

# Run the web service on container startup.
CMD ["/{{project-name}}.wasm"]
```
