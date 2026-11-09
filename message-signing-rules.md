# Message Signing Rules
**ASP v0.1**
Organization: cryptic-accord-org
License: Apache 2.0

## Abstract
This document defines the canonicalization, signing, verification, proof validation and replay protection rules for all ASP envelopes, Agent Cards and Verifiable Credentials.
It is the reference implementation checklist for signing and verification logic.

## 1. General Signing Mandate
Every ASP envelope and Agent Card MUST be signed.
Signing is always performed by the issuer’s local private key; private keys MUST never leave the agent runtime.

## 2. Canonicalization Rule
Before signing:
1. Remove the `proof` field from the object.
2. Serialize the remaining object with **JSON Canonicalization Scheme (JCS)** (RFC 8785).
3. Compute the digest over the JCS output bytes.
4. Sign the digest.

> Do NOT sort keys manually, do NOT use pretty-print JSON. Only JCS is accepted.

## 3. Supported Signature Suites
Required (MUST implement):
- Ed25519Signature2020

Optional (MAY implement):
- secp256r1 (P-256)
- secp256k1

The suite is declared in `proof.type`.
A verifier must reject unknown signature suites.

## 4. Proof Block Schema
```json
"proof": {
  "type": "Ed25519Signature2020",
  "verificationMethod": "did:example:123#key-0",
  "signature": "base64-encoded-signature-bytes"
}
- `verificationMethod`: DID URL pointing to the public key inside the issuer’s DID document.
- `signature`: raw signature bytes encoded in base64.

## 5. Verification Steps (strict order)

1. Check the envelope is well-formed JSON.
2. Check `expires` (if present) is in the future. Reject expired messages.
3. Resolve the `verificationMethod` to fetch the public key.
4. Confirm the controller of the verificationMethod matches the envelope `issuer`.
5. Strip the `proof` field, JCS canonicalize the rest.
6. Recompute digest and verify the signature against the public key.
7. If all pass: signature valid. Otherwise: invalid_signature.

## 6. Timestamp & Replay Protection

- `created`: Unix seconds timestamp, set by the issuer.
- `expires`: Optional unix seconds deadline.
- Gateways and verifiers SHOULD maintain a recently-seen message index keyed by `id` to prevent replay.
- Even with valid signature, an expired message is rejected.

## 7. Cross-Document Consistency

Same signing pipeline applies to:

- ASP primitive envelopes
- AgentCard documents
- Arbitrator ruling VCs
- Reputation attestation VCs

## 8. Forbidden Practices

- Do not sign pretty-printed JSON.
- Do not sign payloads that include the `proof` block.
- Do not forward or relay a message without preserving the original signature envelope.
- Do not modify any field inside a signed envelope after issuance.
- Do not use weak hash algorithms (SHA-1, MD5).

## 9. Signature Failure Classification

- `invalid_signature`: cryptography check failed
- `key_not_found`: verificationMethod cannot be resolved
- `issuer_mismatch`: key controller != envelope issuer
- `message_expired`: timestamp check failed
- `invalid_canonicalization`: implementation uses non-JCS serialization

## 10. Changelog

v0.1: Initial signing, canonicalization and verification rules.
