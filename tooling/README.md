# CLI used for this report

The crates.io release cargo-proofs 0.3.4 records schema v1, while the current service requires schema v2 including the dependency snapshot. This report therefore uses the public source revision `6f4be8ea9107fc8494521c953e23624ef6da0f12` of https://github.com/proofs-rs/proofs-rs, plus the included `cargo-proofs-harness-name.patch`.

The patch corrects harness-name matching: Kani emits `verification::verify_is_even_u32`, while the recorder expects `even_or_odd::verification::verify_is_even_u32`. The fallback must remove the crate prefix from the expected name. Regression tests also reject unrelated nested names and ambiguous matches. No result, contract, or safety check is fabricated or weakened.

From the parent directory containing this verification repository:

```sh
git clone --depth 1 https://github.com/proofs-rs/proofs-rs.git proofs-rs-public
git -C proofs-rs-public fetch origin 6f4be8ea9107fc8494521c953e23624ef6da0f12
git -C proofs-rs-public checkout 6f4be8ea9107fc8494521c953e23624ef6da0f12
patch -d proofs-rs-public -p1 < even-or-odd-verification/tooling/cargo-proofs-harness-name.patch
cargo test --manifest-path proofs-rs-public/cli/Cargo.toml --locked --features reqwest/rustls-tls-native-roots record::tests
cargo install --path proofs-rs-public/cli --locked --debug --features reqwest/rustls-tls-native-roots --force
```

The native-root feature trusts the execution environment's installed CA certificates; TLS verification remains enabled. `--debug` reuses the test build and does not affect Kani. The original release patch is retained only as a record of the initial diagnosis; it alone cannot migrate the recorder to schema v2.
