+++
title = "Installing and Using Typst"
date = 2025-02-21
authors = ["Hildeberto Mendonca",]
description = "Typst is a modern LaTeX alternative developed in Rust. That's how we installed it."
+++

Typst is a modern, markup-based typesetting system designed as a faster and
simpler alternative to LaTeX. It allows you to write documents using a concise
syntax and compiles them directly to PDF, without the need for intermediate
formats. Developed in Rust, Typst emphasizes speed, ease of use, and powerful
features for creating high-quality documents like papers, reports, and
presentations. We use Typst to create customer facing documents.

Since we work with Rust, we thought it would make more sense to compile it from
source, becoming more confident and knowleadble about the technology. To do
that, we did the following:

1. **Installed OpenSSL**: One of Typst's dependencies need OpenSSL to compile,
   so we need to start with it:

    ```bash
    $ sudo apt update
    $ sudo apt install openssl
    $ sudo apt install libssl-dev
    $ openssl version
    $ which openssl
    ```

2. **Setted environment variables**:

    ```bash
    $ export OPENSSL_DIR=/usr/bin/openssl
    $ export OPENSSL_INCLUDE_DIR=/usr/include/openssl
    $ export OPENSSL_LIB_DIR=/usr/lib/x86_64-linux-gnu
    ```

3. **Installed using Cargo**:

    ```bash
    $ cargo install --locked typst-cli
    ```

It takes sometime to compile, but once it is done, the command `typst` becomes
available in the console:

```bash
$ typst document.typ
```

To upgrade Typst, just compile it again, using the `--forge` argument:

```bash
$ cargo install --force --locked typst-cli
```

It replaces the executable located at `~/.cargo/bin`. To remove it, simply
delete the `typst` file located at this folder.