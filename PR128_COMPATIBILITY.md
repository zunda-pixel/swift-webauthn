# Upstream PR 128 compatibility

The user selected [brokenhandsio/swift-webauthn PR #128](https://github.com/brokenhandsio/swift-webauthn/pull/128), head `3fe310ab82b4c7dc5429ca3dbcc1308b3ff8abbb`. This branch keeps the complete upstream commit ancestry, modern Swift settings and new `brokenhandsio/swift-cbor` dependency. LICENSE and upstream ownership remain unchanged. It supersedes the older add-init/Crypto/legacy-CBOR consumer experiment, without modifying the existing add-init branch.

The library delta over PR #128 is four lines:

- Allow Swift Crypto 5 with an upper bound below 6 (the same compatibility approach as upstream PR #129).
- Remove two unnecessary outer `unsafe` markers reported by Swift 6.4.2: AAGUID.bytes and ByteCasting's withContiguousStorageIfAvailable call. Inner pointer operations retain explicit `unsafe`; strict memory and concurrency settings remain enabled.

- Explicitly import Foundation for Base64Utilities when using FoundationEssentials. Actual Linux CI failed at String.replacingOccurrences (lines 46/93) under MemberImportVisibility; FoundationEssentials alone does not expose those String extensions on Linux. The existing Foundation codec is unchanged. Static known-vector regressions cover empty input, both URL alphabet substitutions, and padding round trips.

No public initializers are added: Sauna consumes the manager's verified outputs and does not need to construct Credential or VerifiedAuthentication manually. No legacy SwiftCBOR parser/helper is transplanted into the new CBOR implementation.

The generated public-only lock resolves new swift-cbor 0.2.0 (`52432a1118bcbabb9a973330c3af85d8ddf3f015`) and Crypto 5.0.3. Local Apple Swift 6.4.2 frozen build and all 64 tests (including four Base64 vector cases) pass with warnings-as-errors. Consumer API migrations, bounded framing and actual browser tests are separate checks in sauna-api and its clients.

CI retains supported Swift 6.3/6.4/nightly platform tests and excludes toolchains below the manifest's Swift 6.3 minimum. Caller refs are pinned; narrow Sauna compatibility CI uses the retained common workflow commit. Existing swiftlang callee internals still contain mutable action/script refs and persistent-checkout defaults, so full transitive hardening is not claimed. Package.resolved is explicitly tracked for frozen CI; consumers maintain their own lockfile.

This is a temporary contribution stack. Track the existing upstream PRs rather than opening duplicates. Merge only after latest-head GitHub Codex review has completed without unresolved findings and actual required CI succeeds; usage limits or pending runs are not passing reviews. Switch the consumer from the temporary PR branch to the reviewed merged fork revision after those gates pass, and return to an adequate upstream release when available.
