# Changelog

All notable changes to this fork are documented here.

This is a fork of [openwallet-foundation-labs/sd-jwt-rust](https://github.com/openwallet-foundation-labs/sd-jwt-rust),
diverged at `08d75c7`.

### Added
- Delegate SD-JWT (dSD-JWT / dSD-JWT+KB) chains, behind the `delegate` feature, off by default.
- Custom JWT header parameters on issuance; `alg` cannot be overridden.
- wasm32 target support.

### Changed
- The issuer is generic over a signer, verification takes a key resolver, and issuance, presentation and verification are `async`.
- Always-revealed root claims are now `iss`, `exp`, `nbf`, `aud`; `iat` became selectively disclosable, and `exp` is no longer required at verification time.
- The `sd-jwt-generate` tool under `generate/` no longer builds against the new API.
- Published as `equs-sd-jwt-rs`; the library target stays `sd_jwt_rs`, so `use sd_jwt_rs::…` is unchanged.

### Fixed
- A claim in `claims_to_disclose` absent from the disclosures panicked; it now errors, and may be marked `"optional"` to be skipped instead.
- `nbf` / `exp` on chain links, and `aud` / `nonce` for `kb+sd-jwt(+kb)`, are now validated.
