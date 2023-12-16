# Build wikivote
FROM rust:1-alpine3.18 as builder
RUN apk --no-cache add musl-dev perl make
WORKDIR /wikivote
COPY src src
COPY templates templates
COPY Cargo.lock Cargo.lock
COPY Cargo.toml Cargo.toml
WORKDIR /wikivote/src
RUN cargo build --release

# Final stage
FROM alpine:3.19.0
RUN apk --no-cache add ca-certificates && addgroup -S wikivote && adduser -S wikivote -G wikivote
USER wikivote
WORKDIR /home/wikivote
COPY --from=builder /wikivote/target/release/wikivote /usr/local/bin
