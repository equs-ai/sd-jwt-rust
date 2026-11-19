# Changelog

All notable changes to this fork are documented here.

This is a fork of [openwallet-foundation-labs/sd-jwt-rust](https://github.com/openwallet-foundation-labs/sd-jwt-rust),
diverged at `08d75c7`.

## 0.1.2

### Added
- `KbJwtIatWindow` and `SDJWTVerifier::with_kb_iat_window`, to set how far a Key Binding JWT's `iat` may be from the Verifier's clock.

### Changed
- A Key Binding JWT must carry `iat`, within an acceptable window of the Verifier's clock (RFC 9901 §4.3 and §7.3 step 5e). By default it may be at most 5 minutes old and at most 60 seconds in the future; a fractional NumericDate is accepted. Presentations whose KB-JWT has no `iat`, a non-numeric one, or one outside the window, verified before and are now rejected with `InvalidInput`. This applies to the trailing KB-JWT of a dSD-JWT+KB too.

## 0.1.1 and earlier

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
- A disclosure of the wrong length for its position (a claim needs `[salt, name, value]`, an array item `[salt, value]`) panicked the verifier; it is now `InvalidDisclosure`.
- A decoy digest in an array panicked the holder when the array was disclosed; decoys are now skipped.
