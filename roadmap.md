
---

# 第二个：roadmap.md
```markdown
# ASP Roadmap
**ASP v0.1**
Organization: cryptic-accord-org
License: Apache 2.0

## Abstract
This roadmap outlines specification milestones, reference implementations, audit and stability targets for Agent Social Protocol.
It is living and will be updated by GitHub proposal.

## Phase 1: Spec Foundation (Current, v0.1)
Goal: Lock the core semantic model for contracts, messages, identity and trust.
Deliverables:
- Whitepaper
- Trust Model Specification
- Protocol Primitives
- Agent Card Schema
- Message Signing Rules

Status: In progress.
Target outcome: Stable written specification for builders to implement.

## Phase 2: Escrow & Arbitration (v0.2)
Goal: Formal escrow interface, arbitrator API, dispute evidence bundle format.
Deliverables:
- Escrow interface spec
- Arbitrator input/output schema
- Evidence bundle packaging standard
- Simple test vector suite

Status: Planned.

## Phase 3: Reputation Credentials (v0.3)
Goal: Standardize portable reputation attestation graph.
Deliverables:
- Reputation VC schema
- Selective disclosure rules
- Reputation query interface

Status: Planned.

## Phase 4: Reference Implementation (v0.4)
Goal: Working minimal gateway, SDK and demo marketplace.
Deliverables:
- ASP Gateway (relay + signature verifier + state store)
- Language SDK (TypeScript / Python)
- Demo agent pair that can negotiate, contract, submit evidence and settle
- Local testnet escrow adapter

Status: Planned.

## Phase 5: Stable Release & Audit (v1.0)
Goal: Production-ready specification.
Deliverables:
- Full formal security audit
- Frozen v1.0 spec
- Interoperability test suite
- Registry & discovery standard

Status: Long-term target.

## Non-Goals on Roadmap
- Not building a native agent model or reasoning engine.
- Not mandating a single blockchain or payment rail.
- Not building a centralized reputation database.
- Not enforcing censorship or permissioning.

## Contribution Flow
Roadmap changes are proposed via GitHub Issues and Pull Requests.
Major milestone changes require community discussion.

## Changelog
v0.1: Initial roadmap.
