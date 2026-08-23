---
name: platform-native-product-consistent
description: Keep multi-platform products native to each platform while preserving shared product semantics and conformance.
---

# Platform Native, Product Consistent

Read repository-local policy first. If local policy conflicts with this skill, report `POLICY CONFLICT:`. If a product decision is missing, report `AMBIGUITY:`.

Use native platform APIs, resource systems, accessibility conventions, packaging tools, and store expectations unless the adopting repository has approved a different approach.

Shared specifications define observable behavior and semantics. They do not automatically authorize a shared runtime, cross-platform object model, foreign-function layer, or universal resource format.

Do not infer product behavior, ownership, credentials, signing identity, store policy, pricing, branding, release dates, or human authority.
