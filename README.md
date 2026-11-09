# ASP-Spec
**Agent Social Protocol**
> Organization: `cryptic-accord`
> Version: v0.1
> License: Apache 2.0

> A trust-first open protocol for autonomous agent collaboration, contract, escrow, arbitration and reputation.
> Compatible with MCP / A2A / ANP.

## Overview
ASP (Agent Social Protocol) is an open standard that enables independent agents from different developers to discover each other, negotiate tasks, sign verifiable contracts, delegate work, settle payments and resolve disputes.

MCP: Agent calls tools.
A2A: Agent passes tasks.
ANP: Agent discovers & encrypt communication.
**ASP: Agent makes trusted deals.**

This repository contains the official specification, whitepaper and trust model.
Reference implementation (gateway, sdk, demo marketplace) will be hosted in separate repositories under `cryptic-accord`.

## 📄 Documents
- [Whitepaper & Execution Manual](./whitepaper.md) — Full vision, roadmap, business & governance
- [Trust Model Specification](./trust-model.md) — Core trust axioms, DID, signing, escrow, arbitration, VC
- [Protocol Primitives](./protocol/primitives.md) — JSON-RPC message definitions, task lifecycle
- [Agent Card Schema](./protocol/agent-card-schema.md)
- [Message Signing Rules](./protocol/message-signing.md)
- [Roadmap](./roadmap.md)

## Key Design Principles
1. **Trust is the first priority**
2. Private keys stay local, never upload to gateways
3. Gateways are neutral relay & verify nodes, not judges
4. All interactions are signed, non-repudiable, offline-verifiable
5. Economic layer (escrow, reputation, arbitration) is native, optional extension
6. Interoperable, not competing with A2A / ANP / MCP

## Community
- All specification changes are discussed via GitHub Issues & Pull Requests
- The protocol is neutral; the official demo marketplace is just one reference node

## Disclaimer
ASP does not guarantee agent output correctness. It only guarantees cryptographic evidence chain integrity.
All participants take responsibility for their own cooperation choices.
