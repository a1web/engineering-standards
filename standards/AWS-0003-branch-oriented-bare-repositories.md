# AWS-0003 — Branch-oriented bare repositories and agent workspaces

Status: **Proposed — owner approval and versioned publication required**

## Purpose

Standardize human-readable, isolated Git working directories for parallel human and AI development. No central `.worktrees` directory.

## Canonical layout

```text
~/Projects/
  a1web/
    rodeo/
      .bare/                  # shared Git object database
      .git                    # pointer to .bare; required for tested Herdr root workflow
      main/
      feature/implement-bluebonnet-theme/
      chore/upgrade-ruby/
      hotfix/fix-session-timeout/
    engineering-standards/
      .bare/
      main/
  OSS/
    amiga-rosetta/
      .bare/
      main/
```

The filesystem directories under `feature/`, `chore/`, `fix/`, and `hotfix/` are **Git worktrees**, not nested Git repositories. A branch name is not itself a checked-out directory. The canonical root is `~/Projects` for the dedicated agent account. The layout is a convention, not a mandate to rename existing working repositories without a migration plan.

## Repository initialization

Use a disposable repository first. Qualify Git's behavior with the installed Git version, Herdr, Claude Code, Codex, Cursor Agent CLI, `gh`, and any automation before applying to production repositories.

A possible initialization pattern:

```bash
mkdir -p ~/Projects/a1web/example
cd ~/Projects/a1web/example
git clone --bare git@github.com:a1web/example.git .bare
git --git-dir=.bare config remote.origin.fetch '+refs/heads/*:refs/remotes/origin/*'
git --git-dir=.bare fetch origin
printf 'gitdir: ./.bare\n' > .git\ngit --git-dir=.bare worktree add main main\ngit --git-dir=.bare branch --set-upstream-to=origin/main main
```

Confirm the actual default branch and whether `main` already exists before running the final commands. For a bare clone of a repository whose default branch is `main`, the local `main` branch already exists: do **not** use `-b main` or `--track -b main`. If the default branch differs, adapt the branch and path. Herdr's disposable acceptance test required a root `.git` pointer (`gitdir: ./.bare`) when creating worktrees from the repository root. Never overwrite an existing `.git`; qualify all other agents and tools before final adoption. Agents must run inside checked-out worktrees, not the root. Use explicit `--git-dir` where appropriate.

For new work:

```bash
cd ~/Projects/a1web/example
git --git-dir=.bare worktree add -b feature/implement-bluebonnet-theme feature/implement-bluebonnet-theme main
```

These commands require confirming the bare clone's current HEAD and upstream refs; fail closed if ambiguous. Validate commands against a disposable repository before codifying scripts. Do not treat Herdr idle/working detection as reliable while an agent awaits sign-in or other interactive onboarding. Restarting Herdr can restore layout without restoring running agents, and orphan subprocesses require explicit reconciliation.

## Agent isolation and provenance

- One bounded assignment, unique branch, and dedicated worktree per active worker.
- Agents execute from their assigned working directory, not the bare repository.
- Herdr provides runtime visibility, not authoritative acceptance or durability.
- Git/GitHub and the durable Drover/Cora ledger provide source, PR, disposition, and lifecycle evidence.
- Record task ID, agent, worktree, base/head SHA, tests, PR, and acceptance outcome.
- Never delete a worktree with uncommitted changes, unpushed commits, unresolved reviews, or an unknown disposition. Require verified archival/recovery evidence.
- Workers may prepare commits and draft PRs under scoped authority; no self-approval, protected-branch bypass, unauthorized merge, credential mutation, or deployment.
- Repository-local launch/security rules remain controlling; report policy conflicts rather than silently overriding them.

## Credentials and supervision

Use a dedicated restricted machine identity where available (Cora currently uses `a1web-cora`). Prefer scoped, revocable credentials and short-lived issuance where practical. Infisical is the intended secrets-management control plane, subject to separate approval and setup. The macOS Keychain is appropriate for locally scoped secrets and login material, but is not an authorization boundary between agents running as the same macOS user. Do not copy personal credentials, expose secret values in logs, or grant workers broad production access.

Trail Code is the selected successor to Graft in Cora's toolchain plan; verify its actual distribution, installation, and integration contract before making it a required standard dependency.

## Adoption and rollout

This standard becomes authoritative only after owner review, merge, immutable release tag, and explicit adoption in each repository. Test in a disposable repo first; migrate active work only through a separately approved plan. Do not disturb Rodeo's launch environment.

## Ruby toolchain security baseline (23 October 2026)

For new A1Web Ruby-based engineering tooling, target **Ruby 4.0.9** or a subsequently approved security-patched release. Ruby 4.0.9 addresses regular-expression compilation vulnerabilities identified in the Ruby upstream security advisory (including GitLab-associated CVEs). Source: https://www.ruby-lang.org/en/news/2026/10/23/ruby-4-0-9-released/ .

- Install and select the version under the dedicated agent user's `mise` environment; preserve the previous Ruby until validation completes.
- Pin `ruby 4.0.9` in the tooling project's `.mise.toml` or approved version file, rather than relying only on a mutable global default.
- Verify `ruby -v`, OpenSSL, Psych/YAML, readline, Bundler and the complete worktree tooling suite including disposable end-to-end checks.
- Record tool versions, test results, rollback instructions and source/build provenance. Never print secrets.
- Do not infer that changing Cora's Ruby upgrades Rodeo containers, CI, or production; those require separate exact-head qualification and release authorization.
- This proposed baseline remains subject to repository-owner approval and immutable standards publication.
