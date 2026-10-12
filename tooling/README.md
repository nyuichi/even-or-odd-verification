# cargo-proofs 0.3.4 harness-name fix

Kani 0.68.0 emits `verification::verify_is_even_u32`, while cargo-proofs 0.3.4 records the expected name as `even_or_odd::verification::verify_is_even_u32`. Its fallback strips a crate prefix from the observed name instead of the expected name, leaving no matched contracts even when all 99 checks pass.

The patch corrects that comparison and adds a regression test covering crate-relative names, unrelated nested names, and ambiguous matches. It changes no verification result, contract, or safety check.

Apply it to the published cargo-proofs 0.3.4 source archive:

```sh
cd ..
curl -fL https://static.crates.io/crates/cargo-proofs/cargo-proofs-0.3.4.crate -o cargo-proofs-0.3.4.crate
tar xzf cargo-proofs-0.3.4.crate
patch -d cargo-proofs-0.3.4 -p1 < even-or-odd-verification/tooling/cargo-proofs-0.3.4-harness-name.patch
cargo test --manifest-path cargo-proofs-0.3.4/Cargo.toml --locked --features reqwest/rustls-tls-native-roots record::tests
cargo install --path cargo-proofs-0.3.4 --locked --debug --features reqwest/rustls-tls-native-roots --force
```

Run the installation outside the verification repository to keep its worktree clean. The native-root feature trusts the execution environment's installed CA certificates; TLS verification remains enabled. `--debug` reuses the test build and does not affect the verifier.
