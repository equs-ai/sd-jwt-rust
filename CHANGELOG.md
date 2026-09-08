# Changelog

Notable changes to this fork. Format: [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), versioning: [SemVer](https://semver.org/).

Fork point: upstream `openwallet-foundation-labs/sd-jwt-rust` at `08d75c7` (#41), i.e. after `v0.7.1`.
Upstream's later RFC 9901 alignment (#47–#64) is not merged yet.

## [0.8.0] - unreleased

### Added
- `signer::SDJWTSigner` trait and `key::SDJWTKey`: issuer and holder sign through a pluggable signer.
- `resolver::KeyResolver` trait and `key::SDJWTPubKey` for issuer key lookup at verification time.
- `delegate` feature (off by default): Delegate SD-JWT (dSD-JWT / dSD-JWT+KB) chains — `SDJWTHolder::delegate`, `select_delegate_alternative`, `is_delegated`, `delegation_depth`, `SDJWTVerifier::verify_delegation`, `ChainBindingMode` (`sd_hash` or `issuer_jwt_hash`), depth capped at `MAX_DELEGATION_DEPTH` (32).
- `utils::decode_sd_jwt` and `utils::decode_dsd_jwt` for unverified inspection of a token or chain.
- `verifier::unpack_disclosed_claims` is now public.
- Claims in `claims_to_disclose` may be marked `"optional"`, so a missing claim is skipped instead of failing.
- wasm32 target support: `WasmNotSend` / `WasmNotSync` markers and wasm-compatible time handling.
- Errors: `SigningError` plus chain-specific variants (`ChainParseError`, `ChainSignatureFailed`, `ChainTypMismatch`, `MissingChainBinding`, `AmbiguousChainBinding`, `InvalidChainBinding`, `InvalidDelegatePayload`, `ChainExpired`, `ChainNotYetValid`, `ChainDepthLimitExceeded`).

### Changed
- **Breaking:** `SDJWTIssuer` is generic over `S: SDJWTSigner`; `SDJWTIssuer::new(signer)` replaces `new(issuer_key, sign_alg)`.
- **Breaking:** `issue_sd_jwt`, `create_presentation` and verification are `async`.
- **Breaking:** verification is split into `SDJWTVerifier::new(resolver)` and `verify_presentation(...)`; `create_presentation` takes an optional signer instead of a key and algorithm.
- **Breaking:** `iat` is selectively disclosable, per spec update.
- `exp` is no longer required to be present at verification time.
- Supported SD-JWT spec version: 7 → 19.
- Authors and maintainers metadata trimmed to the current maintainer.

### Fixed
- `sd_hash` generation and KB-JWT algorithm selection, so the issuer-signed JWT and the KB-JWT may use different algorithms.
- Panic on a claim in `claims_to_disclose` that is absent from the disclosures; it now returns an error.
- `nbf` / `exp` not validated on chain links.
- Expected `aud` / `nonce` not validated for `kb+sd-jwt(+kb)`; the two must now be supplied together.
- Regression that dropped the `exp` claim.
- Timestamp handling that failed to compile for wasm32.
