# Versioning

`a1web/engineering-standards` publishes coordinated repository releases. A published release covers the standards, ADRs, reusable agent skills, and templates present at one immutable Git commit.

## Tag Format

Use calendar-version tags:

```text
vYYYY.MM.DD
vYYYY.MM.DD.N
```

Use `vYYYY.MM.DD` for the first standards release on a UTC date. Use `vYYYY.MM.DD.N` for additional releases on the same UTC date, starting at `.2`.

The initial publication tag is `v2026.08.23`.

## Release Boundary

Create a new standards release when any of the following changes:

- a standard in `standards/`;
- an accepted ADR in `adrs/`;
- reusable agent-skill instructions in `skills/`;
- adoption templates in `templates/`;
- repository governance that affects adopting projects.

Do not create a new release for purely editorial fixes that do not change meaning unless an adopting repository needs an immutable corrected reference.

## Clarifications And Policy Changes

Backward-compatible clarifications may be published as a normal calendar release. Policy changes, new ADRs, behavior-changing skill updates, or changed adoption requirements must also publish a new calendar release and explain the impact in the release notes.

Adopting repositories decide when to upgrade. A newer standards release does not automatically become authority for an adopting repository until that repository updates its local adoption record.

## Coordinated Releases

Standards are versioned as a coordinated repository set, not as independently versioned individual documents. Individual standards and ADRs keep stable identifiers such as `AWS-0001` or `ADR-0001`; the Git tag identifies the exact published version of the whole standards set.

## Adoption Upgrades

An adopting repository should record:

- the canonical repository URL;
- the immutable tag or exact commit SHA adopted;
- which standards, ADRs, skills, or templates apply;
- any product-specific local policy that adds stricter gates; and
- the review evidence for the upgrade.

Do not adopt `main` or another moving branch as authority.

## Immutable Tags And Corrections

Published tags are immutable. Never move, delete, reuse, or retarget a published standards tag.

If a published release needs correction, publish a new tag and explain the correction in the new release notes. Adopting repositories can then choose whether to upgrade.
