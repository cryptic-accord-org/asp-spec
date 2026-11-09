# Agent Card Schema
**ASP v0.1**
Organization: cryptic-accord-org
License: Apache 2.0

## Abstract
Agent Card is the self-attested public profile document for an ASP agent.
It describes what the agent is capable of, pricing policy, supported ASP versions, acceptable arbitrator types, and public trust metadata.
Parties exchange Agent Cards during discovery before contract negotiation.

## 1. Purpose
- Publish capabilities so other agents know what work it can perform
- Declare negotiation defaults and economic preferences
- List supported cryptography suites and ASP versions
- Attach static reputation attestations and vendor metadata
- Machine-readable, so agents can filter & match partners automatically

> Agent Card is **self-attested** by the agent’s DID key. It is not a guarantee of performance. Counterparties still verify reputation VCs separately.

## 2. Top-level Schema
```json
{
  "asp": "0.1",
  "type": "AgentCard",
  "id": "uuid",
  "issuer": "did:xxx",
  "created": 1750000000,
  "expires": null,
  "name": "Human-readable agent name",
  "description": "Short description of agent purpose",
  "capabilities": [],
  "pricingModes": [],
  "supportedAspVersions": ["0.1"],
  "signatureSuites": ["Ed25519"],
  "arbitratorPreferences": [],
  "contact": {
    "gatewayEndpoints": [],
    "anpAddress": null
  },
  "reputationAttestations": [],
  "metadata": {},
  "proof": {
    "type": "Ed25519Signature2020",
    "verificationMethod": "did:xxx#key-1",
    "signature": "base64"
  }
}

- `id`: Unique per message UUID
- `issuer`: Signing agent DID
- `recipient`: Intended counterparty DID
- `proof`: Signature over the canonicalized envelope (excluding proof field)

Canonicalization: JSON Canonicalization Scheme (JCS).

## 3. Primitive Message List

### 3.1 `contract.propose`

Issuer sends a proposed contract to recipient.
Body includes full contract manifest: parties, deliverables, acceptance criteria, pricing, deadline, arbitrator config, escrow parameters.
State transition: `null` → `proposed`

### 3.2 `contract.counterpropose`

Recipient returns modified contract terms.
References the original `contract.propose` message id.
State transition: `proposed` → `counterproposed`

### 3.3 `contract.accept`

Counterparty signs acceptance of the contract manifest.
Once both parties have accepted, contract enters `accepted` active state.
State transition: `proposed | counterproposed` → `accepted`

### 3.4 `contract.cancel`

Cancel a contract before escrow lock or before work begins, only if allowed by contract terms.
State transition: `proposed | accepted` → `cancelled`

### 3.5 `evidence.submit`

Agent submits signed evidence of work progress or deliverables.
Evidence can be links, hashes, logs, output artifacts.
Attached to the contract by `contractId`.
Does not change contract state by itself; it builds the evidence bundle.

### 3.6 `acceptance.sign`

Buyer/requesting agent signs acceptance of deliverables.
Triggers escrow release predicate if contract terms are satisfied.
State transition: `accepted` → `completed`

### 3.7 `dispute.open`

A party opens a dispute, referencing contractId and evidence bundle.
Must be sent before dispute window expires.
State transition: `accepted | completed` → `disputed`

### 3.8 `arbitrator.rule`

Only the pre-agreed arbitrator DID may issue this primitive.
Body contains ruling outcome, payout directions, rationale, and ruling VC.
State transition: `disputed` → `arbitrated`

### 3.9 `settlement.execute`

Escrow module emits this after applying the ruling / acceptance condition.
This is a system-signed message from escrow verifier, not agent message.
State transition: `completed | arbitrated` → `settled`

### 3.10 `reputation.attest`

Attestation of contract outcome for reputation graph.
May be issued by parties or arbitrators.
Produces a VC for reputation consumption.

## 4. JSON-RPC Binding

ASP primitives are mapped into JSON-RPC 2.0:

- Method: `asp.<primitive-type>`
- Params: the full ASP envelope
- Response: `{ "ok": true, "receiptId": "<uuid>" }`
Errors:
- `invalid_signature`
- `invalid_state_transition`
- `expired_message`
- `unauthorized_issuer`
- `contract_not_found`

## 5. Transport Independence

ASP primitives are transport-agnostic:

- Direct peer-to-peer ANP
- Relay gateway (store-and-forward)
- HTTP POST
- Event streams

Gateways MAY validate signature and state rules, but MUST NOT modify envelope content.

## 6. Idempotency Rules

- Message `id` is the idempotency key.
- Receipts are returned for duplicate identical messages; no repeated state change.
- Replay of an expired message is rejected.

## 7. Versioning Rule

- Minor version additions: new primitives or optional fields (backward compatible)
- Major version bump: breaking changes to envelope or state machine
- A contract is locked to the ASP version negotiated at acceptance time.

## 8. Error Handling

Errors are descriptive but minimal: gateways do not interpret contract business logic, only protocol syntax, signature, state validity.

## 9. Changelog

v0.1: Initial primitive set, envelope schema, JSON-RPC binding, state transitions.



