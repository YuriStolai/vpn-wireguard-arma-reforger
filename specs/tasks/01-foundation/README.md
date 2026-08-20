# Foundation and Design Validation

| Task | Title | Status |
| --- | --- | --- |
| PLAN-01 | Verify deployment and game network requirements | `todo` |
| PLAN-02 | Select and document the forwarding design | `todo` |

## PLAN-01 — Verify deployment and game network requirements

- **Status:** `todo`
- **Description:** Establish supported VPS and host operating systems and
  verify the current Arma Reforger dedicated-server ports and protocols from
  authoritative sources. Store sources and summaries in `specs/references/`.
- **Definition of Done:** The requirements are linked from the reference index,
  the specification's pending decisions are updated, and the selected scope is
  sufficient to design forwarding rules without guessing.
- **Dependencies:** Administrator input on the intended VPS provider, region,
  and host environment.
- **Validation evidence:** Not started.

## PLAN-02 — Select and document the forwarding design

- **Status:** `todo`
- **Description:** Choose the VPN address plan and the forwarding, NAT, and
  routing approach that provides a valid return path while limiting public
  exposure to approved traffic.
- **Definition of Done:** The plan includes a network diagram, least-privilege
  firewall policy, return-path rationale, rollback considerations, and an
  updated threat model.
- **Dependencies:** PLAN-01.
- **Validation evidence:** Not started.
