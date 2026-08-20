# Next session handoff

> This is an operational entry point, not a source of truth. The
> specification, plan, tracked task, and operational journal prevail.

**Last updated:** 2026-08-20 — repository governance normalized during planning.

## Current state

- **Active task:** [`PLAN-01`](tasks/01-foundation/README.md#plan-01--verify-deployment-and-game-network-requirements)
- **Status:** `todo`
- **Completed in the last session:** Established the canonical plan, backlog,
  documentation map, and operational-journal rules.
- **Open decisions or blockers:** The VPS environment, host operating system,
  verified game ports and protocols, and network design remain undecided.

## Required reading before acting

1. `AGENTS.md`
2. `specs/specs.md`
3. `specs/plan.md`
4. `specs/tasks/README.md` and `specs/tasks/01-foundation/README.md`

## Next safe action

Gather authoritative evidence for the supported Arma Reforger server ports and
protocols and the target VPS and host environments. Record sources under
`specs/references/`, update `PLAN-01` with concise evidence, and do not run
privileged network changes without explicit authorization.

## Essential context

- **Checkout / branch:** Current repository checkout; inspect `git status --short` before editing.
- **Environment / scope:** Planning only; no scripts, live WireGuard peers, or
  infrastructure configuration are in scope yet.
- **Operational evidence:** [`specs/operations/README.md`](operations/README.md)
  defines the required sanitized journal; no external operational command has
  been performed for this project.
- **Safety notes:** Never commit private keys, real endpoints, public IP
  addresses, or machine-specific configuration. External writes need explicit
  authorization.
