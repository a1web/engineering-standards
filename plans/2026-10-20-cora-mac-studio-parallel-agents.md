# Proposed execution plan: Cora Mac Studio and parallel agents

**Date:** 2026-10-20  
**Status:** Proposed implementation plan; not an accepted engineering standard or deployment authorization  
**Scope:** A1Web agent-workstation tooling and Rodeo engineering workflow

## Decision and purpose

Preserve the current Rodeo launch momentum while preparing a faster, isolated multi-agent engineering workstation. Two workstreams run independently:

- **Track A — Rodeo launch:** Cora continues on the 2019 Intel MacBook Pro using the existing working environment, review gates, and launch sequence. Do not interrupt or migrate her until the replacement is proven.
- **Track B — Mac Studio preparation:** Claude Code is the preferred setup agent on the M5 Max Mac Studio (36 GB unified memory), under a dedicated nonadministrator macOS account intended for Cora. GitHub Copilot reviews the proposed changes. Claude prepares worktree tooling and proves safe parallel execution before any migration.

The UGREEN NAS is **not a dependency** for the new daily development architecture because its available RAM is limiting. Preserve its existing Cora environment and recovery artifacts; do not destroy or repurpose them. Defer its container-recreation test.

## Claude Code directive — first tranche: worktree environment

1. Inspect existing `bin/worktrees`, related scripts, repository conventions, and tests **before** proposing changes. Reuse mature OSS/tooling where practical. Avoid replacing functioning infrastructure unnecessarily.
2. Work in an isolated checkout/branch on the Mac Studio; do not mutate Cora's active MacBook branches, worktrees, credentials, or running processes.
3. Make the tooling portable across macOS and Linux. Prefer Bash and Ruby; avoid Python scripts. Use safe quoting, deterministic names, clear help/diagnostics, idempotent setup, and explicit destructive-action confirmation.
4. Support isolated per-agent Git branches and worktrees, safe creation/listing/status/cleanup, collision detection, and safeguards against deleting dirty/unmerged work.
5. Identify and isolate mutable runtime resources as needed: ports, temporary files, caches, Rails/Node build artifacts, test databases, and services. Avoid shared writable state or secret duplication.
6. Follow existing repository CI, security, and code-review conventions. Add automated tests for worktree lifecycle, failure recovery, and cleanup; avoid modifying unrelated application code.
7. Demonstrate **two independent agents in two separate worktrees**, performing bounded, non-conflicting tasks and producing **two green CI results**. Report paths, branches, exact HEADs, test evidence, resource isolation, and any limitations.
8. Submit changes for review. No autonomous merge, deployment, production mutation, permission expansion, or provider configuration changes.

**First-tranche acceptance:** two isolated agents; two independent worktrees; two successful CI runs; zero interference with the active Rodeo launch workstream.

## Second tranche — private inference and security

After the worktree milestone is accepted, evaluate LM Studio/MLX on the M5 Max with a modest quantized local model, benchmarking memory and throughput before choosing a model. Establish a narrowly scoped local API and deterministic information-classification/model-routing controls. Sensitive material must not be sent to cloud models or logged/telemetrized without explicit authorization. Never treat a local LLM as the security boundary; enforce least privilege and network restrictions outside the model. Keep credentials in an appropriate secrets-management boundary rather than agent prompts or repository files.

## Later tranches

- Validate a real, bounded Rodeo assignment on the Mac Studio without changing the active MacBook workflow.
- Introduce Herdr orchestration and additional agents incrementally after isolation is demonstrated.
- Migrate Cora only after durable identity/state, authentication, task recovery, performance, and rollback are verified. Preserve rollback to the MacBook.
- Continue Rodeo's exact-head review, protected CI, and no-unauthorized-deployment rules throughout.

## Explicit non-authorizations

This plan authorizes documentation and a proposed implementation sequence, **not** executing migrations, copying credentials, changing GitHub protections, merging PRs, deploying software, or modifying production/providers. Any required elevated permissions or destructive actions must be separately approved.

## Reporting format

For each tranche report: scope attempted; files/branches/HEADs; tests and CI evidence; security and isolation checks; failures and rollback; remaining risks; and the next approval gate.
