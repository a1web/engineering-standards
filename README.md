# A1Web engineering standards

Reusable architecture decisions, engineering policies, and AI-agent standards for A1Web applications.

## Status

This repository is the canonical public source for reusable A1Web engineering standards once published by an authorized repository owner.

Product-specific repositories may adopt these standards by immutable tag or commit SHA. Product-specific policy remains local to the adopting repository.

## Standards

- `standards/AWS-0001-localization-internationalization-accessibility.md`
- [AWS-0002: RubyGems Trusted Publishing (proposed)](standards/AWS-0002-rubygems-trusted-publishing.md)

## ADRs

- `adrs/ADR-0001-platform-native-localization-with-shared-semantics.md`
- [ADR-0002: RubyGems Trusted Publishing (proposed)](adrs/ADR-0002-rubygems-trusted-publishing.md)

## Agent skills

- `skills/localization-internationalization/SKILL.md`
- `skills/accessibility-first/SKILL.md`
- `skills/platform-native-product-consistent/SKILL.md`

## Adoption

Use `templates/adoption.md` in the adopting repository and pin the adoption to a published tag or commit SHA.

Use the [RubyGems release checklist](templates/rubygems-release-checklist.md) when adopting Trusted Publishing. This repository is canonical; the `scarver2/engineering-standards` fork mirrors accepted changes without independent policy edits.

## Versioning

See `VERSIONING.md`. Published standards releases use immutable calendar-version tags such as `v2026.08.23`; adopting repositories must pin to a tag or exact commit SHA, never an unversioned branch.
