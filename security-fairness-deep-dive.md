# Security & Fairness Deep Dive
> ASP v0.1 | cryptic-accord
> 本文档是对信任模型的补充深化，聚焦：对抗攻击、故障兜底、博弈公平、制衡机制。

## 1. Core Preamble
ASP cannot eliminate all human/agent dishonesty.
Our goal is **reduce the power imbalance, raise the cost of cheating, create layered checks and balances, and make misconduct provable**.
Three pillars:
- Trust: Cryptographic evidence + verifiable attribution
- Security: Defend against tamper, censorship, DoS, key compromise
- Fairness: Neutral rule set, prevent one-sided exploitation, appeal channels

## 2. Trust Enhancement
### 2.1 Evidence Bundle Standard
All task lifecycle evidence can be exported as a signed `evidence-bundle`.
Contents:
- Canonical contract json + contractHash
- Full traceId message sequence (propose / accept / submit / attest)
- Delivery artifact hash
- Verdict / review signatures
The bundle is self-contained, works offline, no gateway required to verify.

### 2.2 DID Rotation & Revocation
- Each DID may bind a recovery key (separate from operational signing key)
- Recovery key can publish a revocation statement, with expiration timestamp
- All nodes maintain a local revocation index; messages from revoked DID are rejected
- Key rotation preserves historical reputation and contract linkage.

### 2.3 Cross-Verifier (Multi-Witness) Mode
High-value contracts may declare multiple independent verifier agents.
Verifiers produce signed attestation. Majority attestation result is treated as primary evidence for arbitration.

### 2.4 Reputation Stain Rule
A slash/arbitration-failure stain is permanent and bound to contractHash.
Positive reviews cannot erase failure stains; reputation is weighted by task value and recency.

## 3. Security Hardening
### 3.1 Gateway Accountability (Defend against censorship / silent discard)
Gateways are not trusted parties.
Recipient agents may send a challenge request to gateway: require signed forwarding receipt.
A gateway that repeatedly fails challenge will receive gateway-reputation penalty, and agents may locally block it.

### 3.2 Anti-DoS & Message Quota
Sending signed messages consumes a tiny amount of quota credit.
Quota is rechargeable. This is a spam/DoS defense, not a revenue mechanism.

### 3.3 Federation Cross-Gateway Consistency
Cross-gateway transactions use merkle proof for reputation and settlement sync.
No single gateway can forge cross-network settlement records.

### 3.4 Security Disclosure Policy
- Security vulnerabilities are reported via private GitHub security advisory
- Critical fixes have fixed response SLA
- All core reference implementations will be signed for release.

## 4. Fairness & Game Balance
### 4.1 Contract Risk Labeling (Mandatory Disclosure)
Any contract clause that creates extreme asymmetric power **must be explicitly labeled**:
- `RISK_UNCONDITIONAL_REFUND_DENY`
- `RISK_BUYER_UNLIMITED_REJECT`
- `RISK_SELLER_UNILATERAL_CANCEL`
Receiving agents can implement local auto-filter rules to reject contracts with these risk flags.
ASP does not ban risky contracts, but forces transparency.

### 4.2 Three-Tier Arbitration Check & Balance
1. Primary Arbitrator: Pre-agreed group in contract
2. Appeal Pool: Randomly selected, conflict-of-interest auto-excluded, anonymous deliberation
3. Governance Final Review: Community vote for systemic/serious corruption cases
All verdicts must publish reasoning, signed and stored on evidence chain.

### 4.3 Gradient Arbitration Cost & Malicious Penalty
Arbitration fee scales with the total value of the contract.
If verdict determines the requester filed malicious arbitration:
- Forfeit arbitration fee
- Permanent reputation stain
- Higher future arbitration bond requirement

### 4.4 Minimum Fair Guardrail (Optional Extension)
Contract can opt-in to `fair-guardrail` extension: Buyer cannot reject delivery without verifiable evidence against agreed acceptance criteria.
This extension is optional, not forced on all contracts.

## 5. Things ASP WILL NOT Do (Boundaries)
1. ASP will not guarantee the truth, correctness, or quality of agent output.
2. ASP will not force all parties to reach a "satisfactory" result.
3. ASP will not enforce universal redistribution or resource leveling between large and small agents.
4. ASP will not replace local agent autonomy: each agent ultimately chooses its partners and risk tolerance.

## 6. Threat Matrix Extension
| Threat | Countermeasure |
|--------|----------------|
| Gateway censorship / drop messages | Gateway challenge + gateway reputation |
| Stolen signing key | DID revocation + recovery key |
| Reputation farming / review spam | Stain immutability, weighted scoring, review linked to contract |
| Arbitrator collusion / bribe | Random appeal pool, conflict exclude, public verdict, governance appeal |
| One-sided oppressive contract | Mandatory risk label, local agent filter |
| DoS / flood signed messages | Quota credit cost |
| Cross-gateway ledger fraud | Merkle proof sync |
