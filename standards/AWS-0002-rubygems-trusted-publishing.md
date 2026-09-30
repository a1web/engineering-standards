<!-- standards/AWS-0002-rubygems-trusted-publishing.md -->
# AWS-0002: RubyGems Trusted Publishing

Status: Proposed; effective only after owner approval.

## Scope and authority

Use GitHub Actions Trusted Publishing for RubyGems releases across A1Web and scarver2 projects. This standard defines reusable safeguards, not permission to release any product. Each repository names its release authority, supported versions, default branch, and approval process.

**Green automation is evidence, not authorization to publish.** Version changes, release tags, and publication require explicit product-local authorization. An agent must not manufacture a test release to validate this setup.

## Trust setup

An authorized gem owner registers a RubyGems trusted publisher with the exact GitHub owner, repository, workflow filename (for example, `release.yml`), and environment (`release`). The workflow job must use that same environment. OIDC exchanges the job identity for a short-lived, gem-scoped publishing credential; do not store a long-lived RubyGems API key in GitHub secrets.

Protect the GitHub environment with a designated human reviewer, disable administrator bypass, and restrict deployment to release tags. Configure tag rules to restrict creation, update, and deletion to authorized actors. Record whether self-review is permitted in the product's release policy; do not silently choose it. Confirm the repository's plan supports the required protections before declaring setup complete.

Publisher registration and environment protection are separate settings: registering a publisher does not create an approval gate. Verify both. Reusable workflows require additional publisher identity fields; follow current RubyGems guidance rather than copying a same-repository configuration.

## Workflow contract

- Trigger on an explicitly authorized release tag, conventionally `v*`; reject tags outside the product's supported version syntax before publication.
- Use workflow-level `permissions: {}` and grant only `contents: read` and `id-token: write` to the publishing job. Do not grant repository write access merely to upload a gem.
- Use the protected `release` environment. Serialize publication with a concurrency group and `cancel-in-progress: false`.
- Check out the tagged commit with `persist-credentials: false` and sufficient history to verify its relationship to the canonical default branch.
- Require the tag's version to equal the gem's declared version. Verify the tagged commit is reachable from the approved default branch; this ancestry check supplements, but does not replace, human approval of the exact commit.
- Run canonical project commands such as `bin/spec`, `bin/lint`, and `bin/package`. CI calls these commands rather than maintaining a second implementation. A project may consolidate invariants in `bin/release-check`.
- Require the supported Ruby matrix to pass for the release commit; document the chosen publishing Ruby and Bundler versions locally.
- Use a reviewed revision of `rubygems/release-gem`. Review action and Bundler upgrades for changes to publication, build, and Git behavior.

The permission fragment below is not a complete workflow:

```yaml
permissions: {}
jobs:
  release:
    permissions:
      contents: read
      id-token: write
    environment: release
```

The release action invokes Bundler's release task. Verify that the selected Bundler version recognizes the already-existing matching tag and skips tag creation/Git pushes. This is why the tag-first workflow can use `contents: read`, even though upstream examples may grant `contents: write`. Do not expand permissions to hide a failing invariant. GitHub Release creation, if desired, is a separately reviewed responsibility.

## Artifact evidence and completion

Inspect the package manifest for credentials, personal data, fixtures, and unintended files. Record the release commit, tag, toolchain, package checksum, and test results. Set a consistent `SOURCE_DATE_EPOCH` from the release commit timestamp for verification and publication builds when supported by the toolchain.

Do not assume the publishing action uploads the previously inspected file: it may rebuild. Verify the actual action behavior and compare the downloaded published artifact against the reviewed package. Investigate differences, including timestamp metadata; record reproducible evidence instead of declaring unequal hashes equivalent without inspection. Validate an isolated install and the public API or CLI version after publication.

After a failed or interrupted run, inspect RubyGems and GitHub before retrying: publication may already have succeeded. Never move a published release tag or overwrite a published version. Any yank or corrective release needs separate product-local authorization.

Setup is complete when trust identity and approval controls are verified. End-to-end publication remains unproven until the next authorized release succeeds; report those states separately.

## Adoption and references

Adopt this standard by an approved immutable standards tag or commit, using the [adoption template](../templates/adoption.md). Keep release-owner names and product-specific gates in the product repository. Use the [release checklist](../templates/rubygems-release-checklist.md).

- [Decision record](../adrs/ADR-0002-rubygems-trusted-publishing.md)
- [RubyGems Trusted Publishing guide](https://guides.rubygems.org/trusted-publishing/)
- [RubyGems release action](https://github.com/rubygems/release-gem)
- [ABBU reviewed implementation, PR #50](https://github.com/scarver2/abbu/pull/50)
- [ABBU workflow at its merged commit](https://github.com/scarver2/abbu/blob/85990f9db821798be6ab9f9abc0e3c3a635a3322/.github/workflows/release.yml)

ABBU is implementation evidence, not a source of universal product rules. Its Sheriff gate remains ABBU-local.

—
Stan Carver II
Made in Texas 🤠
https://stancarver.com
