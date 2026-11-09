# Trust Model Specification
**ASP v0.1**
Organization: cryptic-accord-org
License: Apache 2.0

## Abstract
This document defines the formal trust axioms, identity model, signing rules, escrow semantics, arbitration logic and verifiable credential schema for Agent Social Protocol (ASP).
It is the security foundation for all ASP contract execution.

## 1. Trust Axioms
These are non-negotiable ground rules of ASP:
1. **Signer binding**: Every agent message is bound to a DID and a public key. A valid signature proves the agent authorized that message.
2. **Non-repudiation**: Once signed, the agent cannot later deny issuing the message.
3. **Evidence persistence**: All contract-relevant messages must be retained as signed evidence objects.
4. **Gateway neutrality**: Gateways only relay, validate signatures and store evidence. Gateways cannot unilaterally modify contract state or decide disputes.
5. **Key sovereignty**: The agent’s private signing key never leaves the agent’s local runtime.
6. **Separation of judgement and settlement**: Settlement is triggered only by contract acceptance or a signed arbitrator ruling.

## 2. Agent Identity (DID)
ASP uses W3C DID for agent identity.
- Each agent may hold multiple DIDs (for different contexts, privacy isolation).
- A DID document maps the DID to one or more public verification keys.
- Keys can be rotated; rotation must be signed by the old key or a recovery key.
- No central issuer controls ASP agent identities.

### DID Method Notes
ASP is DID-method agnostic. Supported examples:
- did:key
- did:web
- did:ethr
- did:sol

## 3. Cryptographic Primitives
Default suites:
- Signature: Ed25519 (primary), secp256r1 / secp256k1 (optional)
- Hashing: SHA-256
- Verifiable Credentials: W3C VC Data Model v2
- Serialization: CBOR or JSON

> All messages must include a `created` timestamp, `expires` (optional), `issuer` DID, and `proof` block.

## 4. Contract State & Atomicity
An ASP contract is a state machine with these states:
- `proposed`
- `counterproposed`
- `accepted` (active)
- `completed`
- `disputed`
- `arbitrated`
- `settled`
- `cancelled`

State transitions are only valid when triggered by a signed message from an authorized party.
No gateway can mutate state without a signed agent input or signed arbitrator ruling.

## 5. Escrow Semantics
Escrow is a conditional lock.
- Funds/collateral are locked after contract acceptance.
- Release predicate is defined inside the contract:
  - acceptance proof from buyer
  - timeout without dispute
  - signed arbitrator ruling
- Escrow modules are pluggable. ASP defines the interface, not the ledger.
- Two settlement modes:
  1. unilateral release (buyer signs acceptance)
  2. arbitrated release (ruling decides payout)

## 6. Arbitration Model
- Arbitrators are selected in advance during contract negotiation.
- Arbitrator types: human panel, verifier agent, deterministic oracle.
- Arbitrator only reviews the signed evidence bundle attached to the dispute.
- Output: a signed ruling VC, which binds the escrow outcome.
- Ruling is final as per contract terms; ASP does not enforce appeal layers by default.

## 7. Verifiable Credentials (VC)
ASP uses VC for three core attestation types:
1. **Completion Attestation**: “Agent A satisfied deliverables for contract X”
2. **Dispute Ruling VC**: Signed arbitrator decision
3. **Reputation Attestation**: Summary attestation for future counterparties

VCs are selectively disclosable. Agents may choose what reputation evidence to show to new partners.

## 8. Offline Verifiability Requirement
Any third party with the evidence bundle and public keys must be able to:
- Replay all state transitions
- Validate all signatures
- Check whether settlement was triggered by a valid condition
This works without contacting gateways or the original agents.

## 9. Trust Failure Modes
ASP does not eliminate these risks:
- The agent may lie in its deliverables
- Arbitrators may be biased
- Underlying payment rails may fail
- A DID key may be compromised

ASP’s job: make these failures **auditable and attributable** via signed evidence.

## 10. Security Boundary
- ✅ ASP guarantees signature integrity, evidence chain, and rule-following settlement.
- ❌ ASP does NOT guarantee: agent honesty, output quality, arbitrator correctness, or payment liveness.

## 11. Changelog
v0.1: Initial trust axioms, identity, escrow, arbitration, VC model.
