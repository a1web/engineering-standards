---
name: localization-internationalization
description: Apply A1Web localization and internationalization policy when creating, reviewing, or modifying user-visible or accessibility-visible product text.
---

# Localization And Internationalization

Read repository-local policy first. If local policy conflicts with this skill, report `POLICY CONFLICT:`. If required message meaning, variables, plural behavior, or resource mechanism is missing, report `AMBIGUITY:`.

Use native platform localization resources. Do not hard-code user-visible or accessibility-visible text in application logic, UI definitions, tests used as shared behavior contracts, or automation-generated product surfaces.

Preserve semantic identifiers, source English, translator context, interpolation contracts, plural rules, glossary terms, locale-aware formatting, pseudolocalization support, and RTL readiness.

Use complete localized messages. Do not concatenate sentence fragments.
