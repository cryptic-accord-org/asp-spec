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
## 3. Field Definitions

- `capabilities`: Array of capability strings / JSON schemas. Example: `["image-generation", "code-review", "data-analysis"]`
- `pricingModes`: Supported billing models: `fixed`, `hourly`, `milestone`, `bid`
- `supportedAspVersions`: Which ASP spec versions this agent understands
- `signatureSuites`: Supported signing algorithms
- `arbitratorPreferences`: Acceptable arbitrator kinds: `human-panel`, `verifier-agent`, `oracle`
- `gatewayEndpoints`: Public relay URLs other agents can use to send messages
- `anpAddress`: Optional ANP discovery address
- `reputationAttestations`: Array of VC URLs or VC blobs the agent chooses to disclose
- `metadata`: Open, unvalidated extra fields (vendor tags, model version, runtime info)

## 4. Validation Rules

1. Issuer DID must match the proof verification method’s controller.
2. Signature must cover the whole card excluding the `proof` block (JCS canonicalized).
3. Expired cards should be ignored by negotiation participants.
4. Capabilities are only claims; they are not automatically verified.

## 5. Usage Flow

1. Agent discovers another agent via registry / marketplace / ANP
2. Fetch or receive its AgentCard
3. Local agent checks: ASP version compatibility, signature suite, pricing/arbitrator preferences
4. If compatible, start `contract.propose` negotiation
5. AgentCard is NOT part of the contract; it is only pre-negotiation metadata.

## 6. Security Notes

- Agent can lie on its AgentCard. ASP does not validate advertised capabilities.
- Trust is established later via contract, evidence, arbitration and reputation attestations.
- Agent may rotate or revoke its AgentCard by publishing a replacement card with newer timestamp.

## 7. Changelog

v0.1: Initial Agent Card schema and negotiation usage rules.
