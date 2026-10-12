# rust-is-even-or-odd

A simple library containing two methods for determining whether a number is even or odd: "is_even" and "is_odd."
## Kani verification

This fork verifies `is_even::<u32>` for every `u32` input using Kani 0.68.0. The postcondition is `result == (number % 2 == 0)`. The function body is unchanged from even-or-odd 0.1.0; the fork adds a `Copy` bound so the postcondition can reuse the input.

- Public report: https://proofs.rs/report/5
- Verified source commit: `2a31ef64bbff220d9fcf2e1d4b13fb04934a5621`
- Recorded run: `b46304e8-8a30-4616-ab3a-9e22566b3577`
- Result: 99 checks, 0 failures; 1 verified contract harness.
- Scope: `u32` only. Other type instantiations and `is_odd` are not verified.

```sh
cargo kani -Z function-contracts
```

See [tooling/README.md](tooling/README.md) for the exact recorder source revision and patch used to publish this report. The crates.io cargo-proofs 0.3.4 release alone is not sufficient for the current service schema. Evidence logs are in [evidence/](evidence/).
