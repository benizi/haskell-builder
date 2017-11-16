# Dockerfile for haskell-builder

This is a Dockerfile for building Haskell programs for Alpine Linux.

# Background

Alpine uses `musl-libc`, so precompiled binaries aren't usually appropriate,
since the default assumption is that systems use GNU `glibc`.

# Usage

This can be used in multi-stage builds:

    ```
    FROM benizi/haskell-builder:latest as builder
    ADD https://github.com/username/haskell-repo/archive/v1.0.tar.gz /src.tgz
    RUN set -e ; \
      mkdir /build ; \
      cd /build ; \
      tar xf /src.tgz ; \
      cd * ; \
      stack build ; \
      stack install ; \
      cd / ; \
      rm -r /src.tgz /build

    FROM alpine:latest
    COPY --from=builder /opt/ /
    ```
