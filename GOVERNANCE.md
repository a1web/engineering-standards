# Governance

`a1web/engineering-standards` is the canonical public repository for reusable A1Web engineering standards.

## Authority

The repository owners approve standards, ADRs, templates, and agent skills. Product repositories may adopt a published standard by immutable tag or commit SHA.

Product-specific behavior, release gates, pricing, distribution, brand, store, signing, credential, and customer commitments belong in the adopting product repository, not in this reusable standards repository.

## Versioning

Every published standards release has an immutable Git tag under `VERSIONING.md`. Adopting repositories should pin to that tag or a commit SHA rather than an unversioned branch.

## Mirrors

`scarver2/engineering-standards` may exist as a fork or automated read-only mirror. It must not become an independently edited source of policy truth.
