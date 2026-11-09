# Agent Social Protocol (ASP) Whitepaper
**Version: v0.1**
Organization: cryptic-accord-org
License: Apache 2.0

## Abstract
Large language models and agent runtimes are rapidly maturing, yet agents lack a native trust layer to safely transact, delegate work, and settle value across independent developer ecosystems.
MCP enables tool calling, A2A enables task passing, ANP enables discovery and encrypted transport. ASP fills the missing layer: **verifiable, enforceable deals between autonomous agents**.

ASP is an open, neutral protocol for agent-to-agent contracts, escrow, arbitration, and decentralized reputation.
It does not compete with MCP / A2A / ANP; it composes on top of them.

## 1. Background
Today’s agent interactions are mostly informal: prompts, function calls, ad-hoc JSON payloads.
There is no standard way for two agents to:
- Agree on a task specification, deliverables, pricing and deadline
- Lock funds or collateral until conditions are satisfied
- Preserve non-repudiable proof of every step
- Trigger neutral dispute resolution when outputs diverge from the contract
- Build portable reputation that follows the agent across platforms

Existing frameworks focus on communication or tool invocation, not economic and trust safety.

## 2. Core Vision
We envision a multi-agent internet where agents can safely outsource work to unknown peers, just as humans hire contractors, buy services, and form teams.
ASP’s goal is to make trust programmable, cryptographic, and runtime-agnostic.

Key thesis:
> Intelligence does not equal trustworthiness. Trust must be encoded separately in the protocol.

## 3. Scope & Boundaries
### In scope
- Agent contract schema and negotiation flow
- Cryptographic signing of all messages
- DID-based agent identity and verification
- Escrow primitives for conditional settlement
- Arbitration workflow and evidence submission
- Verifiable reputation credentials
- JSON-RPC protocol bindings

### Out of scope
- Agent reasoning, planning, or model inference
- Built-in payment rails (ASP defines settlement interface; chains/payment systems are pluggable)
- Mandatory on-chain storage (ASP supports both on-chain and off-chain escrow)
- Agent permissioning or censorship (ASP is neutral)

## 4. Design Principles
1. **Trust is the first priority**
   The protocol is optimized for verifiability and non-repudiation, not raw speed.
2. **Private keys stay local**
   Agents never expose signing keys to gateways or intermediaries.
3. **Gateways are relays and verifiers, not judges**
   Gateways forward messages, validate signatures, and serve evidence; they do not decide disputes.
4. **All interactions are signed, non-repudiable, offline-verifiable**
   A third party can audit the full history without asking either agent.
5. **Economic layer is optional**
   Pure reputation-only contracts are allowed; escrow and payments are extensions.
6. **Interoperable, not competing**
   ASP works alongside MCP, A2A, ANP and other agent standards.

## 5. System Overview
An ASP interaction lifecycle has 5 stages:
1. **Discovery**: Agents find each other (ANP / registry / marketplace)
2. **Negotiation**: They propose, counter, and finalize a contract
3. **Execution**: Tasks run, evidence is logged and signed
4. **Settlement**: Conditions trigger release of escrow or reputation updates
5. **Arbitration (optional)**: If contested, evidence is sent to arbitrators for a binding ruling

Every state transition produces a signed, timestamped ASP message.

## 6. Identity & Cryptography
- Each agent controls one or more DID identities
- Ed25519 / secp256r1 signatures for all messages
- Verifiable Credentials (VC) for reputation, attestations, and arbitration results
- Signatures are binding for the agent’s legal or operator identity

## 7. Contract & Escrow Model
An ASP contract is a signed document containing:
- Parties (agent DIDs)
- Deliverables, acceptance criteria, deadline
- Payment / collateral rules
- Arbitrator selection rules
- Dispute window

Escrow holds value until acceptance criteria are satisfied or an arbitrator issues a ruling.
Settlement is conditional and atomic.

## 8. Arbitration
Arbitrators are pluggable: human reviewers, specialized verifier agents, or deterministic oracles.
The protocol only standardizes:
- Evidence submission format
- Ruling signing format
- Ruling effect on escrow and reputation

Arbitrators cannot modify the contract; they only decide whether the contract terms were met.

## 9. Reputation
Reputation is not a single centralized score.
It is a graph of signed attestations:
- “Agent X completed contract Y satisfactorily”
- “Arbitrator Z ruled against Agent X”
- “Agent X provided valid evidence for contract W”

Consuming systems compute their own reputation views from this graph.

## 10. Roadmap
- v0.1: Core specification, message schema, contract model
- v0.2: Escrow interface, arbitration workflow
- v0.3: Reputation credential format
- v0.4: Reference gateway SDK and demo marketplace
- v1.0: Stable spec, formal security audit

## 11. Governance
This specification is developed openly on GitHub.
Changes are proposed via Issues and Pull Requests.
All substantial design tradeoffs must include security and fairness analysis.

## 12. Disclaimer
ASP does not guarantee correct agent outputs.
It guarantees cryptographic integrity of the evidence chain.
All participants bear responsibility for their own cooperation choices.
