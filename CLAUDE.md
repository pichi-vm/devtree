# devtree — zero-copy `no_std` devicetree (FDT v17) parser, writer, overlay applier

Rust (stable, edition 2024), single crate, zero dependencies. An optional
`alloc` feature (off by default) adds an owned representation; the default build
stays pure `no_std`. Library overview: `README.md`.

## Commands

```sh
cargo fmt --all -- --check
RUSTFLAGS=-D warnings cargo clippy --all-targets --all-features
RUSTFLAGS=-D warnings cargo test  --all-targets --all-features
hawkeye check                                    # year-less SPDX headers
cargo deny check                                 # license / advisory policy
pipx run gitlint --commits origin/main..HEAD     # commit messages
```

## Done

Green on all of the above — the repo's four CI jobs (fmt+clippy+test, license
headers, cargo-deny, commit messages) — before reporting done.

## Rules

- **Stay `no_std` and unsafe-free.** The crate sets `unsafe_code = "deny"` and
  the default build must not pull in `std`. New allocation goes behind the
  `alloc` feature (default-off) so bare-metal consumers (e.g. tatu) stay pure
  `no_std`.
- **Preserve parse-once-then-infallible.** `Tree::parse` does full structural
  validation in a single pass so later walks are infallible; keep walks over
  untrusted bytes bounded (depth limits) and validate phandle math against
  reserved values before observing or emitting them.
- **Regenerate binary fixtures via the script.** `tests/data/*.dtb{,o}` are built
  from the checked-in `*.dts{,o}` with `tests/data/regenerate.sh` (needs `dtc`) —
  don't hand-edit the binaries.

<!-- Shared rules live in the .agent submodule. If .agent/ is empty, run:
     git submodule update --init --recursive -->
@.agent/karpathy/CLAUDE.md
@.agent/pichi.md
@.agent/rust.md
