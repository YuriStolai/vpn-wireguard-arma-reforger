# WireGuard VPN for Arma Reforger

This project aims to provide public access to an Arma Reforger server running
on a local network without configuring port forwarding on the router. A VPS
with a public IP address receives player traffic and forwards it to the host
computer through a WireGuard tunnel.

The project is in the planning phase. The detailed architecture, game ports
and protocols, forwarding rules, and supported environments will be defined
before implementation begins.

## How It Works

```text
Player (PC or console)
          |
          | Public VPS IP and port
          v
      Public VPS
          |
          | WireGuard tunnel
          v
Host on the local network
          |
          v
Arma Reforger server
```

The host initiates an outbound connection to the VPS and keeps the tunnel
active. Players access only the VPS public endpoint and do not need to install
WireGuard, import keys, or change their devices' network configuration. Reply
traffic returns to the players through the same VPS.

## Goals

- Avoid changes to the local network router.
- Provide players with a conventional public endpoint.
- Allow PC, PlayStation 5, and Xbox connections without a client-side VPN.
- Expose only the ports required by the game and VPN administration.
- Keep network complexity limited to the VPS and the host.
- Provide reproducible installation, validation, and diagnostic procedures.

## Scope

The project will include the network architecture, WireGuard tunnel
configuration, traffic forwarding between the VPS and the host, minimum
security controls, examples without sensitive data, and connectivity checks.

The following are out of scope:

- Installing or administering the Arma Reforger server beyond its
  connectivity.
- Configuring port forwarding on the local router.
- Installing WireGuard on player devices.
- Administering accounts, permissions, or bans within the game.
- Guaranteeing the availability of third-party services or infrastructure.

## Planned Prerequisites

- A VPS with a public IP address and permission to open the required ports.
- A host computer capable of running WireGuard and initiating outbound
  connections.
- An Arma Reforger server running on the host.
- Administrative access to the VPS and the host.

Supported distributions and versions will be documented after the architecture
is defined.

## Project Status

There are no scripts, ready-to-use configurations, or installation commands
yet. Before implementation begins, the following must be defined:

- The VPS provider, region, and operating system.
- The host operating system.
- The VPN addressing plan.
- The ports and protocols used by Arma Reforger.
- The firewall, NAT, and routing rules.
- The targets for latency, packet loss, and concurrent players.
- Secure storage for each peer's configuration.

Planned deliverables include a network diagram, a threat model, configuration
templates with the `.example` suffix, idempotent scripts, an operations guide,
and connectivity tests.

## Security

Never commit private keys, real public addresses, endpoints, or
machine-specific configurations. The VPS and host must use distinct WireGuard
key pairs, and network rules must follow the principle of least privilege.

Carefully review every script before running it with administrative privileges.
Tests and procedures must avoid permanent firewall or network changes and
provide an explicit rollback method when they make temporary changes.

## Documentation

The current specification, including requirements, acceptance criteria,
constraints, and pending decisions, is in
[`specs/specs.md`](specs/specs.md).

The selected technical approach and delivery sequence are in
[`specs/plan.md`](specs/plan.md). The implementation backlog is indexed in
[`specs/tasks/README.md`](specs/tasks/README.md), while the operational
procedures and troubleshooting guide will live in
[`docs/README.md`](docs/README.md).

For contributors, follow [`AGENTS.md`](AGENTS.md). No installation command is
available yet because the project remains in planning.
