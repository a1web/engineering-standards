# ADR-0001: Platform-native localization with shared semantics

## Context

A1Web products may target multiple operating systems and frameworks. Users should experience consistent product meaning, but platforms have different native localization systems and accessibility APIs.

## Decision

Use platform-native localization resource systems with a shared semantic catalog and cross-platform conformance checks.

The shared catalog defines semantic message identifiers, source English, meaning, translator context, variables, plural requirements, glossary terms, and conformance expectations. Platform-native resources remain the runtime source for localized strings.

## Alternatives considered

1. One universal runtime localization file format for every platform.
2. Hard-code English copy and translate later.
3. Platform-native resources without shared semantic governance.

## Consequences

Native platform conventions remain intact while product language stays governable. Translation tooling can use platform resources directly. Shared conformance checks can verify semantic coverage without forcing a shared runtime parser.

The approach requires discipline: product teams must maintain semantic identifiers, translator context, and platform resource coverage together.

## Per-platform guidance

- Apple platforms should use String Catalogs or the current native Apple localization resource system.
- Android should use string, plural, and locale-qualified resources.
- Windows should use the resource system established for the selected Windows framework.
- Linux should use gettext/PO or the selected toolkit's established localization system.
- Web applications should use the approved framework localization system and avoid concatenated sentence fragments.

## Migration and adoption

Adopting products should inventory existing user-visible and accessibility-visible strings, create semantic keys, move runtime text into native resources, add translator context where meaning is ambiguous, and pin adoption to a published standards tag or commit SHA.

## Enforcement and tests

Adopting products should add CI checks or review gates for hard-coded user-visible strings, missing native resources, semantic-key drift, pseudolocalization smoke tests, RTL smoke tests where supported, and accessibility-visible text coverage.
