# WireGuard VPN for Arma Reforger Implementation Plan

This plan implements the behavior defined in [`specs.md`](specs.md), which
remains authoritative for scope and requirements.

## Current context

The repository is in planning. The intended design is a two-peer WireGuard
tunnel from a locally hosted Arma Reforger server to a publicly reachable VPS.
The VPS receives player traffic and forwards only validated game traffic to the
host through the tunnel. Players never join the VPN.

Architecture choices that depend on verified game port requirements, supported
host operating system, and the chosen VPS remain open; no firewall, NAT, or
routing command is approved by this plan alone.

## Stack and tooling

- WireGuard for the private VPS-to-host tunnel.
- A Linux VPS with a public IP address; the provider, region, and distribution
  are still to be selected.
- Bash for future idempotent setup and diagnostic scripts.
- Safe `.example` configuration templates, with private keys and
  machine-specific values kept outside the repository.
- Native WireGuard, firewall, and routing checks, plus ShellCheck for scripts,
  when implementation begins.

## Architecture and execution flow

1. The host establishes an outbound WireGuard connection to the VPS and keeps
   it reachable despite the absence of router port forwarding.
2. A player sends supported Arma Reforger traffic to the VPS public endpoint.
3. The VPS applies an allowlist and forwards the traffic to the host's VPN
   address.
4. Return traffic follows a valid path through the VPS back to the player.

The implementation must explicitly select and document either NAT, policy
routing, or a combination that preserves the return path. Port and protocol
details must be verified from current authoritative game documentation before
they are encoded in configuration or scripts.

## Configuration and operations

Future configuration will separate committed examples from local secrets and
must provide a dry-run or clear inspection step before any privileged change.
Procedures will document prerequisites, setup, validation, troubleshooting,
and rollback in [`../docs/README.md`](../docs/README.md).

Commands that inspect, validate, or change the VPS, host firewall, routes, or
WireGuard interfaces must be recorded in [`operations/README.md`](operations/README.md)
with sanitized output. External writes require explicit user authorization.

## Security and reliability

- Use separate WireGuard key pairs for the VPS and host; do not commit private
  keys, real endpoints, public IP addresses, or machine-specific configuration.
- Restrict public exposure to the verified WireGuard administration port and
  game traffic only.
- Use least-privilege firewall rules and document cleanup for temporary rules.
- Make scripts idempotent where practical, validate dependencies early, and
  fail with actionable messages.
- Preserve the host's outbound tunnel with an appropriate keepalive value only
  after the selected environment has been tested.

## Quality gates

Current documentation changes require `git diff --check` and review of
`git status --short`; run `markdownlint "**/*.md"` when it is installed.

When scripts and configuration are introduced, add and document their native
syntax or dry-run validation, ShellCheck for Bash, and integration tests that
use temporary interfaces and clean up all test network changes. No new tool or
dependency is introduced by this planning document.

## Deliverables and sequencing

1. Verify the operating environments and Arma Reforger networking requirements.
2. Select the addressing, forwarding, NAT, routing, and firewall design.
3. Produce a threat model, network diagram, and redacted configuration
   templates.
4. Implement idempotent setup and diagnostic scripts with rollback guidance.
5. Add non-destructive connectivity tests and document validation.

The tracked backlog is in [`tasks/README.md`](tasks/README.md).

## Alternatives and deferrals

Direct router port forwarding and player-managed VPN access are out of scope.
The following choices are deferred until evidence is available: VPS provider,
region, VPS and host operating systems, tunnel address range, game ports and
protocols, forwarding mechanism, performance targets, and secure configuration
storage method.

## Documentation synchronization

Review affected governed documents in the same unit of work. Keep requirements
in `specs.md`, technical decisions here, task status and concise evidence in
`tasks/`, external sources in `references/`, and command history in
`operations/`.
