# Mission-based autonomous engineering

Status: proposed for repository-owner review. This document specifies an operating model and implementation contract; it does not establish that the runtime, dispatcher, credentials, or host services are configured.

## Goal

Deliver substantial features and verified operational capabilities concurrently, without a human relaying routine messages or assigning implementation steps. Measure accepted outcomes, integration quality, time blocked, cost, and recovery success rather than prompts, commits, or agent utilization.

## Responsibilities

| Role | Responsibility |
|---|---|
| Human owner and Deputy | Define outcomes, priority, acceptance criteria, authority, and consequential decisions. |
| Cora, working lead | Own substantial implementation work, interpret missions, coordinate interfaces and integration, and delegate bounded outcomes to other agents. |
| Drovers | Own missions through planning, implementation, testing, PR creation, review fixes, and verification. |
| Herdr | Supply supported agent sessions and communication interfaces. Inspect the installed version before relying on any lifecycle or recovery behavior. |
| Background supervisor | Reliably deliver authorized missions, wake the lead, reconcile state after interruption, enforce resource bounds, and expose stalled or failed work. Do not duplicate the lead's technical planning. |
| Copilot Code Review and CI | Provide routine review findings and automated checks; authors address findings against the latest revision. |
| Authorized merge/release mechanism | Enforce the adopting repository's protections and explicitly granted authority. |

Herdr is infrastructure, not the reasoning agent. Cora supplies technical judgment. The supervisor supplies reliable execution mechanics. Cora need not review or merge every PR; additional review follows local policy and material risk.

## Mission contract

Each mission records:

- An outcome and concrete acceptance evidence, with scope and exclusions.
- Priority, dependencies, responsible agent, and links to related work.
- Approved repositories, permitted actions, data classification, and escalation boundaries.
- Resource, concurrency, spending, and retry limits.
- Specification revision, state, current checkpoint, branch/worktree, PRs, and delivered evidence.

Agents choose implementation steps and may propose or delegate supporting outcomes within mission authority. Decomposition is an implementation choice, not a request for repeated human approval. A general backlog may contain ideas; dispatchable missions must have enough definition and authority to proceed.

## Continuous parallel execution

1. An authorized actor submits a mission revision through a durable ingress.
2. The supervisor persists a receipt and delivers it to Cora through an interface demonstrated on the installed Herdr.
3. Cora claims a primary mission and delegates independent missions to eligible drovers, considering demonstrated skills, permissions, dependencies, capacity, and cost.
4. Agents execute concurrently in isolated worktrees and coordinate shared interfaces before conflicting changes accumulate.
5. PR authors receive Copilot findings and CI failures directly and complete remediation without a human relay.
6. Completion is checked against mission evidence. Integration and deployment remain distinct states.
7. Available agents continue the highest-priority eligible mission within authority; human-dependent items are recorded while independent work proceeds.

Periodic health checks inspect meaningful progress, not requests for another small task. A healthy long-running build or feature implementation must not be interrupted merely because a check-in interval elapsed. Backlog exhaustion, budget limits, or absence of authorized work are valid idle states.

## Durable handoff and recovery

Use the adopting repository's approved issue system as the specification and status record. A label or comment can serve as ingress only with authenticated actor validation and a defined specification revision. Issue prose cannot expand permissions.

Require unique mission and assignment identifiers, atomic claims, leases or equivalent ownership, duplicate suppression, and durable checkpoints. Recover interrupted delivery by reconciling agent sessions, branches, and existing PRs before creating replacements. Do not promise exactly-once external side effects: reconcile uncertain outcomes before retrying.

Keep a lead intake channel separate from active implementation input so new directives do not overwrite an ongoing prompt. Define acknowledgment, completion, failure, cancellation, and timeout behavior. The supervisor must remain effective when the lead process exits.

A GitHub update does not automatically wake a ChatGPT conversation. Prove both directions of the actual ingress and reporting connection; record any unavailable channel honestly.

## Permissions and sensitive work

Use distinct verified agent identities and least-privilege process credentials. Supply secrets directly to authorized tools through the approved secrets mechanism; avoid model context and retained logs. A local model is not itself a security boundary.

Local-only missions require enforced filesystem/tool restrictions and tested network boundaries, with no cloud fallback. Test with synthetic credentials. Cloud agents cannot inspect sensitive local-only material through shared worktrees, logs, or artifacts. Model suggestions cannot override enforced permissions.

Pause only affected work for missing authority or consequential uncertainty. Record the exact decision, impact, and recommendation. Product-specific release gates and operational authority remain in the adopting repository.

## Build sequence

### 1. Inspect the existing installation

Record authoritative upstream source/docs, installed version, runtime APIs, supported agent CLIs, session lifecycle, remote interfaces, and extension points. Distinguish implemented-and-proven from documented-but-untested and missing. Select the existing A1Web tooling repository before adding runtime code; do not turn this standards repository into an orchestration application.

### 2. Implement the minimum execution loop

Reuse native Herdr capabilities. Add only the missing authenticated ingress, persistent assignment/checkpoint storage, wake/reconciliation service, and status reporting. Use approved repository languages and host service conventions. Provide start, status, stop, and recovery procedures. Keep adapters narrow and do not build a second agent framework.

### 3. Close review and integration

Connect PR/CI/review updates to the author. Preserve repository protections and current approval requirements. Evidence must match the latest commit; changed scope invalidates affected acceptance evidence. A green test suite alone does not establish feature acceptance.

### 4. Qualify concurrent missions

Start with a working lead plus two drovers as an initial configurable ceiling, subject to measured host capacity. Run substantial independent missions, not a synthetic sequence of tiny steps. Coordinate integration and prove continued progress when one mission requires a human.

### 5. Expand from results

Enable additional runtimes and local-only work separately after qualification. Tune concurrency using host health, completed outcomes, integration friction, and spending. Do not delay product delivery while building optional orchestration features.

## Required acceptance evidence

- Two substantial missions advance concurrently; each has an accountable owner and isolated worktree.
- At least one mission traverses implementation, PR, review remediation, fresh CI, and acceptance evidence without human message relays.
- After a mission completes, an eligible agent continues the next authorized mission without a human supplying the next instruction.
- A human-blocked mission does not stop independent delivery.
- Duplicate submission and supervisor/lead restart do not produce duplicate logical assignments or blind retries of uncertain external actions.
- Agent failure is detected, bounded recovery succeeds or yields an actionable escalation, and checkpoints survive restart.
- Unauthorized directives cannot dispatch work or expand permissions.
- Cancellation stops new dispatch and safely reconciles active assignments.
- Concurrency and cost ceilings hold.
- Synthetic sensitive material does not reach prohibited model providers or retained logs; local-only execution cannot fall back to cloud.
- Enabled runtimes, remaining gaps, receipts, commits, checks, review dispositions, and outcomes are recorded.

## Implementation dispatch

The first implementer should inspect the host and native Herdr interfaces, then deliver the minimum loop and its recovery tests in the selected tooling repository. Cora and drovers continue authorized product missions in parallel wherever dependencies permit. A standards PR is not proof of dispatch: record the implementer's explicit acknowledgment and implementation PR link.

## Adoption

This proposal grants no production permissions, changes no adopting repository's gates, and publishes no standards release. On acceptance, publish according to VERSIONING.md and adopt by immutable tag or commit. Keep product priorities, host-specific paths, identities, credentials, and deployment decisions in local adoption records.
