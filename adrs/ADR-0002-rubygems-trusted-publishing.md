<!-- adrs/ADR-0002-rubygems-trusted-publishing.md -->
# ADR-0002: RubyGems Trusted Publishing with human release approval

Status: Proposed; pending owner approval.

## Context

Interactive gem publication couples release execution to local credentials and expiring MFA challenges. Long-lived CI publishing keys avoid that interaction but add credential storage and rotation risk. ABBU's reviewed release workflow demonstrates a reusable alternative.

## Decision

Adopt [AWS-0002](../standards/AWS-0002-rubygems-trusted-publishing.md): tag-triggered GitHub Actions publication using RubyGems OIDC, a protected human-approved environment, and a read-only repository token. Human authorization remains distinct from automated verification. Product repositories retain their own named approvers, version policy, supported Ruby matrix, and release gates.

Keep the standard canonical in `a1web/engineering-standards`. Synchronize `scarver2/engineering-standards` as a read-only fork after canonical changes are accepted; do not maintain independent copies of policy. Adoption pins an approved immutable standards revision.

## Consequences

Routine publication no longer requires a stored RubyGems API key or a local interactive gem-push session. Owners still configure trust, protect environments/tags, approve releases, and maintain their account security. A registered publisher is not proof that a release completed.

Read-only Git permissions require an already-existing matching tag and verified release-tool behavior. Package rebuilds require reproducibility checks. Action/toolchain changes and reusable workflow identities require review. Exceptional manual publication is not an automatic fallback: document and obtain product-owner authorization first.

## Alternatives

- Manual MFA publication: retain only as an explicitly authorized recovery path, not the routine workflow.
- Long-lived publishing secrets: avoid their storage and rotation burden.
- Publication whenever CI is green: rejected because passing checks do not grant release authority.

## References

- [RubyGems guidance](https://guides.rubygems.org/trusted-publishing/)
- [ABBU PR #50](https://github.com/scarver2/abbu/pull/50)
- [Governance](../GOVERNANCE.md)
- [Versioning](../VERSIONING.md)

—
Stan Carver II
Made in Texas 🤠
https://stancarver.com
