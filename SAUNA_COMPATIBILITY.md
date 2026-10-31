# Sauna consumer compatibility

This add-init compatibility PR merges upstream main while retaining the user's public initializers and Linux helper. Crypto 5 support is retained with the minimal upper-bound change used by upstream PR #129. Original LICENSE/source ownership is unchanged.

SwiftCBOR is temporarily pinned to `sauna-studio/SwiftCBOR@eb8664c774024cfd0599757b3f7d75cdd09ca9cc`, the public, unmerged [fork PR #1](https://github.com/sauna-studio/SwiftCBOR/pull/1). That fork cherry-picks the minimal existing upstream PR #120 fix, preserving authorship, rather than opening a duplicate contribution. Its incremental decoder avoids preallocation from untrusted counts; consumers still need their own byte/depth/framing bounds. Remove the fork after a suitable upstream release and successful consumer regressions.

This is a PR stack, not a merged dependency claim. While validating the stack, Sauna API may pin this PR branch and its own lockfile. After the fork PR and this add-init PR pass all merge gates, switch the API back to add-init and regenerate the resolved lockfile. Common reusable workflow references remain pinned to retained default-branch commits, independently of temporary library PR revisions.

Local Apple Swift 6.4.2 frozen build and full 63-test suite passed with the pinned CBOR fork. Package.resolved changes only the CBOR source/revision plus its expected originHash. The dedicated read-only pinned Linux CI remains mandatory. Existing cross-platform Swift CI remains enabled; its inherited callee internals still contain mutable references and credential-persistence defaults, so full transitive hardening is not claimed.

Latest-head GitHub Codex review is required before merge. Other current Sauna PRs hit the account code-review usage limit; this is not a passing review. If this round is also limited, leave the PR open and resume a single review after capacity is restored. Local independent Review/Security review does not replace the GitHub gate.
