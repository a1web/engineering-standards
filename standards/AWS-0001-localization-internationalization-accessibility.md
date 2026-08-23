# AWS-0001: Localization, internationalization, and accessibility

## Policy

English is the required initial locale for A1Web applications unless a product-specific policy says otherwise. Every application must be internationalization-ready from its first user interface.

No user-visible or accessibility-visible text may be hard-coded outside an approved localization resource mechanism for the platform or framework.

Each operating system and framework should use its native or established localization system. Shared specifications govern semantic keys, meaning, variables, glossary, and conformance expectations; they do not require a universal runtime localization file format.

## Complete-message localization

Localized messages must be complete messages, not concatenated sentence fragments.

Use the platform's native support for interpolation, pluralization, grammatical ordering, and translator context. Identical English text with different meanings must use distinct semantic identifiers.

## Locale-aware formatting

Use native locale-aware facilities for dates, times, numbers, currencies, units, percentages, file sizes, relative times, lists, collation, and culturally formatted values.

Do not assume left-to-right layout, fixed text length, ASCII-only input, English word order, English plural rules, fixed punctuation, or fixed decimal separators.

## Accessibility

Accessibility-visible text participates in localization. Labels, hints, roles, values, announcements, keyboard command descriptions, permission descriptions, errors, and recovery guidance must use approved localization resources when they can be perceived by users or assistive technology.

Accessibility support is part of the release gate, not post-release cleanup.

## Release gates

Before release, an adopting product should verify:

- all user-visible and accessibility-visible text is loaded from approved localization resources;
- a complete English locale exists;
- missing required resources fail visibly during development or CI;
- pseudolocalization or equivalent expanded-text smoke testing has been performed;
- right-to-left layout smoke testing has been performed where the platform or framework supports RTL;
- Unicode and text expansion do not break core workflows;
- semantic-key or conformance checks pass where a shared catalog exists.

## Permitted exceptions

Hard-coded text is permitted only when it is not user-visible or accessibility-visible, such as source comments, developer documentation, localization keys, protocol identifiers, schema identifiers, database identifiers, test names, controlled fixtures, invariant machine-readable error codes, and internal diagnostic logs that cannot appear in product UI.

Tests may contain expected English copy only when explicitly validating the English locale. Shared behavior tests should prefer semantic identifiers or observable outcomes rather than treating English wording as the product contract.

## AI-agent requirements

AI agents must never introduce hard-coded user-visible or accessibility-visible text. They must preserve localization keys, translator context, native resource structures, interpolation and plural contracts, locale-aware formatting, pseudolocalization support, and RTL readiness.

When the correct message meaning, variable contract, or platform resource mechanism is unclear, agents must report `AMBIGUITY:` rather than inventing a shared contract.

When repository-local policy conflicts with this standard, agents must report `POLICY CONFLICT:`.
