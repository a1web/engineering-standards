<!-- templates/rubygems-release-checklist.md -->
# RubyGems Trusted Publishing adoption and release checklist

Based on [AWS-0002](../standards/AWS-0002-rubygems-trusted-publishing.md). Record the adopted immutable standards revision and product-local release authority before use.

## One-time setup

- [ ] Reviewed workflow merged; exact owner/repository/workflow/environment registered by a RubyGems owner.
- [ ] Environment requires the designated human reviewer; administrator bypass disabled; self-review choice documented.
- [ ] Deployment restricted to release tags; tag creation/update/deletion restrictions verified.
- [ ] Workflow permissions empty by default; publishing job has only `contents: read` and `id-token: write`.
- [ ] Checkout credentials not persisted; no long-lived RubyGems publishing secret required.
- [ ] Selected action/Bundler behavior verified to skip Git writes for an existing matching tag.
- [ ] Version, ancestry, canonical test/lint/package checks, and release serialization configured.
- [ ] Setup verification recorded separately from the first successful OIDC publication.

## Each authorized release

- [ ] Product release authority approves exact version and commit; supported CI matrix passes.
- [ ] Package contents inspected; toolchain, timestamp policy, and checksum recorded.
- [ ] Authorized matching tag created/pushed; protected environment approval granted.
- [ ] Publishing job succeeds; published package downloaded and compared with reviewed artifact.
- [ ] Isolated install and public API/CLI smoke check pass; evidence recorded in product release notes.
- [ ] On failure, remote publication state inspected before retry; no tag rewriting or unauthorized yank.

## Shared documentation publication

- [ ] Canonical standards PR approved and merged by an authorized owner.
- [ ] `scarver2/engineering-standards` mirror synchronized from canonical history without independent policy edits.
- [ ] Any coordinated standards tag separately authorized; adopters pin an approved immutable revision.

—
Stan Carver II
Made in Texas 🤠
https://stancarver.com
