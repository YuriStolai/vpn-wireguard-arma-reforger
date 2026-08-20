# Specification: Arma Reforger Access Through WireGuard

## 1. Overview

This project must provide access to an Arma Reforger server running locally on
the administrator's computer. A VPS with a public IP address will receive
player connections and forward traffic to the host computer through a
WireGuard tunnel. Configuring port forwarding on the local network router or
installing and configuring a VPN on player devices must not be required.

This specification describes the expected outcome and the project's
motivation. Architecture and implementation details must be documented in
supplementary specifications before implementation begins.

## 2. Problem

The Arma Reforger server runs on a local network and is not directly accessible
to external players by default. Conventional exposure would require router
changes such as port forwarding, which may be unavailable, undesirable, or
difficult to administer.

The project needs to create a private path between the VPS and the computer
hosting the game. Players must use only the VPS public IP address and port, as
they would when accessing a conventional server. This approach is also needed
to support consoles such as PlayStation 5 and Xbox, where configuring a
WireGuard client must not be required.

## 3. Goal

Allow players on computers or consoles to connect to the local Arma Reforger
server by entering only the VPS public IP address and port. Traffic must be
forwarded from the VPS to the host computer through a WireGuard network without
configuring port forwarding on the local router.

## 4. Motivation

The VPN will be used to:

- Eliminate the dependency on changes to the local router.
- Hide VPN and local network complexity from players.
- Allow access from devices without configurable WireGuard support, including
  PlayStation 5 and Xbox.
- Reduce direct exposure of the host computer and game service to the Internet.
- Centralize the publicly accessible address and ports on the VPS.
- Provide a simple connection process compatible with the standard game client.

## 5. Scope

### 5.1 Included

- Define the network architecture between the VPS and the host.
- Configure a WireGuard tunnel between the VPS and the host.
- Receive the traffic required by Arma Reforger on the VPS public IP and ports.
- Forward that traffic through the VPN to the corresponding ports on the host.
- Correctly forward server reply traffic to the player.
- Document installation, configuration, connection, validation, and
  troubleshooting.
- Provide configuration examples without sensitive data.
- Define minimum access controls for the private network and key protection.
- Create checks that prove connectivity without permanently changing the
  network or firewall of the equipment under test.

### 5.2 Out of Scope

- Installing, configuring, or administering the Arma Reforger server beyond
  what is required for VPN connectivity.
- Modifying the local router or enabling port forwarding.
- Installing or configuring WireGuard on player computers or consoles.
- Distributing private keys or other secrets through the repository.
- Administering accounts, permissions, or bans within Arma Reforger.
- Guaranteeing the availability of third-party services or infrastructure.

## 6. Actors

- **Administrator:** maintains the VPS, VPN, forwarding rules, and local server.
- **Player:** enters the VPS IP address and port in the Arma Reforger client
  without participating in the VPN.
- **Game host:** local computer running the Arma Reforger server and WireGuard
  peer.
- **VPS:** server with a public IP address that acts as a WireGuard peer,
  receives player connections, and forwards game traffic to the host.

## 7. Functional Requirements

### RF-01 — Host Connection

The game host must establish and maintain a WireGuard tunnel with the VPS over
an outbound connection without depending on port forwarding on the local
router.

### RF-02 — Public Endpoint

The VPS must provide a public IP address and the ports required for players to
find and access the server.

### RF-03 — Server Access

With the tunnel active, the VPS must forward traffic received on the public
game ports to the corresponding VPN address and ports on the host. Reply
traffic must return to the player through the VPS.

### RF-04 — Traffic Restriction

The configuration must expose only the traffic required for game server access
and VPN administration on the VPS. Other VPS and host services must not be
available to players by default.

### RF-05 — Clients Without VPN Configuration

Players must be able to access the server by entering only the VPS IP address
and port through the flow supported by the game. The solution must not require
software, keys, network profiles, or WireGuard configuration on player devices.

### RF-06 — Diagnostics

The documentation must provide checks that distinguish, at minimum, failures
in tunnel establishment, forwarding, address translation, routing, firewall
rules, and access to game ports.

## 8. Non-Functional Requirements

### RNF-01 — Security

- The VPS and host must have distinct WireGuard key pairs.
- Private keys, real public addresses, and machine-specific configurations must
  not be committed.
- Examples must use clearly identified fictional values.
- Network rules must follow the principle of least privilege.

### RNF-02 — Reproducibility

Procedures must run from the repository root, document prerequisites, and fail
with clear messages when a dependency is missing.

### RNF-03 — Maintainability

Future scripts must be idempotent when practical, use Bash with strict error
handling, and keep reusable configurations separate from secrets.

### RNF-04 — Operational Impact

Tests and configuration procedures must avoid permanent, unintended changes to
the firewall or network. Temporary changes must have an explicit cleanup or
rollback process.

### RNF-05 — Performance

The solution must support a playable session. Measurable targets for latency,
packet loss, and concurrent players must be defined after the architecture and
hosting environment are known.

## 9. Main Flow

1. The administrator prepares the VPS, WireGuard infrastructure, and forwarding
   rules.
2. The administrator registers the VPS and host as WireGuard peers.
3. The host establishes and maintains the tunnel without requiring an
   Internet-initiated connection to the local router.
4. The player enters the VPS public IP address and port in the game.
5. The VPS receives the packets and forwards them through the tunnel to the
   host.
6. The Arma Reforger server replies, and return traffic passes through the VPS
   to the player.

## 10. Acceptance Criteria

### CA-01 — No Port Forwarding

**Given** that no port forwarding rule exists on the host's router,
**when** the host activates its WireGuard configuration,
**then** the tunnel to the VPS must be established over an outbound connection.

### CA-02 — Public Game Access

**Given** that the tunnel between the VPS and host is active,
**when** a player enters the VPS public IP address and port in the game,
**then** traffic must reach the Arma Reforger server and replies must return to
the player through the VPS.

### CA-03 — Service Isolation

**Given** a player accessing the VPS public IP address,
**when** the player attempts to access a port or service that is not explicitly
authorized,
**then** access must be blocked.

### CA-04 — Compatibility Without WireGuard on the Client

**Given** a player on PC, PlayStation 5, or Xbox with a compatible Arma Reforger
client,
**when** the player accesses the VPS public endpoint,
**then** installing WireGuard, importing keys, or changing the device's network
configuration must not be required.

### CA-05 — Secret Protection

**Given** the repository's committed content,
**when** it is inspected,
**then** it must not contain private keys, real endpoints, or participant
machine-specific configurations.

### CA-06 — Reproducible Procedure

**Given** an environment that meets the documented prerequisites,
**when** the administrator follows the procedures from the beginning,
**then** the administrator must be able to configure and validate the complete
path while the player needs only the public VPS IP address and port.

## 11. Constraints and Assumptions

- The VPS must have a reachable public IP address and allow the ports required
  by Arma Reforger and WireGuard to be opened.
- The host computer must be able to initiate outbound connections and run
  WireGuard.
- Only the VPS and host will participate in the VPN. Players will be ordinary
  external clients and will not have WireGuard keys.
- Forwarding must preserve a valid return path. The use of NAT, routing, or a
  combination of both will be defined in the network specification.
- The exact Arma Reforger ports, protocols, and rules must be validated in the
  network specification before implementation.

## 12. Pending Decisions

- Choose the VPS provider, region, and operating system.
- Define the supported host operating system.
- Define the VPN addressing plan.
- Validate the ports and protocols used by Arma Reforger.
- Decide the forwarding, NAT, and routing rules on the VPS and host.
- Establish targets for latency, packet loss, and player count.
- Define a secure storage method for VPS and host configurations.

## 13. Planned Deliverables

- Architecture specification and network diagram.
- Threat model and firewall rules.
- Installation and configuration scripts, when approved.
- Configuration templates with the `.example` suffix.
- An operations guide for configuring, validating, and replacing VPS and host
  peers.
- Connectivity tests and configuration validation.
- An updated `README.md` with commands and the project entry point.

## 14. Traceability

| Goal | Related requirements | Acceptance criteria |
| --- | --- | --- |
| Avoid port forwarding | RF-01 | CA-01 |
| Allow game access | RF-02, RF-03 | CA-02, CA-06 |
| Do not configure clients | RF-05 | CA-04 |
| Limit exposure | RF-04, RNF-01 | CA-03, CA-05 |
| Simplify operations and support | RF-06, RNF-02, RNF-04 | CA-06 |
