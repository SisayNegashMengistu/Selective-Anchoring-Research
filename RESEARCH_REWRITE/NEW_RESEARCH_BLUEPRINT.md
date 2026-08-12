# New Research Blueprint

**Manuscript:** Selective Anchoring Architecture for LMIC Disease Surveillance  
**Prepared:** 2026-08-12  
**Stage:** Pre-rewrite forensic audit — blueprint for revised research direction  
**Note:** This document describes what *should* be done. It does not invent results, implementations, or experiments that have not yet been performed.

---

## Section A — Defensible Claims Assessment

### A. Claims Defensible Immediately (without new experiments)

1. **Problem framing**: LMIC DEWS systems suffer from data centralization, metadata-level privacy exposure, unscalable ledger replication, and resource-uniform participation models. This is defensible from the literature.

2. **Architectural design**: A selective-anchoring hybrid architecture — encrypted IPFS off-chain storage + smart-contract coordination layer — is a well-reasoned design response to the identified problems. It can be defended analytically.

3. **Tier mapping**: The five-tier topology maps coherently to the Ethiopian PHEM administrative hierarchy. This is defensible from the PHEM organizational structure.

4. **Analytical storage comparison**: The calculation that IPFS payload (2.4–48.6 KB) is 12–217× larger than the calldata anchor (196–224 bytes) is analytically derivable and defensible as a design-level estimate, provided it is labeled explicitly as analytical.

5. **Threat model (descriptive)**: The threat-to-mitigation table is defensible as an analytical security analysis, provided it is labeled as analytical (not penetration-tested) and the IPFS revocation limitation is corrected.

6. **Consensus design rationale**: PoA selection for the prototype, with a descriptive comparison to DPoS/PBFT/Raft, is defensible as a design rationale if labeled as analytical (not measured).

7. **CompletenessVerifier as event counter**: The verifier as a period-level submission counter (not field-level completeness inspector) is defensible and aligns with what the contract listing actually implements.

---

### B. Claims Requiring Implementation

1. **AccessControl contract with proper caller authentication** — grantAccess/revokeAccess must have access modifiers before any security claim can be made.

2. **ReportCoordinator anchorReport with reporter registration** — unrestricted submission must be fixed.

3. **Key management layer** — AES key distribution, rotation, and revocation. Minimum: design document with pseudocode. Ideal: implemented in prototype.

4. **HMAC-keyed integrity commitment** — replace SHA-256(plaintext) with HMAC-SHA-256(key, nonce, plaintext) or similar.

5. **Mocha/Hardhat test suite** — 24+ test cases for AccessControl, ReportCoordinator, CompletenessVerifier; must be executable and produce logged output.

6. **Deployment scripts** (Hardhat or Truffle) against a local chain.

7. **IPFS integration code** (ipfs-http-client + AES-256-GCM client).

---

### C. Claims Requiring New Experiments

1. **Gas cost measurements** — Hardhat gas reporter against deployed contracts on local chain.
2. **Block confirmation latency** — Hardhat or Ganache local PoA chain measurements (replaces Rinkeby).
3. **IPFS retrieval latency** — local daemon first; distributed gateway under simulated network conditions second.
4. **Functional test results** — Mocha output showing 24+ tests passing.
5. **SUMI evaluation** — must be re-conducted with the actual implemented prototype; requires ethics documentation.

---

### D. Claims Requiring Stronger Analysis Only

1. **Revocation semantics** — a rigorous description of what `revokeAccess()` revokes (on-chain state only), what it does not revoke (IPFS access, key knowledge), and what additional mechanism (key rotation/proxy re-encryption) is required for true data revocation.

2. **Metadata privacy analysis** — formal analysis of what on-chain metadata reveals (reporter pseudonymity, timing, tier, CID) and what countermeasures exist.

3. **Blockchain necessity argument** — explicit comparison to transparency log and signed append-only log alternatives; argument for why smart contract programmability adds necessary value.

4. **"Why Ethereum vs. Hyperledger Fabric"** — analytical comparison.

---

### E. Claims That Should Be Removed from Current Contributions

1. **ZKP as a current contribution** — not implemented; move to future work.
2. **FHIR-aware interoperability workflows as a current contribution** — not implemented; move to future work.
3. **"Comparative consensus framework" as a quantitative contribution** — reframe as design rationale.
4. **"Edge and fog preprocessing" as a current contribution** — reframe as application-layer middleware design.
5. **"24/24 test cases pass"** — remove until tests are implemented and run.
6. **SUMI scores as reported** — remove or strongly qualify until re-conducted.
7. **Gas cost dollar figures** — remove until ETH price and gas price basis are specified and measurements are taken.
8. **Rinkeby block time data** — remove; replace with reproducible local chain data.

---

### F. Future Work That Must Not Appear as Current Contributions

1. ZKP circuit for privacy-preserving completeness verification
2. FHIR SMART on FHIR integration
3. Layer-2 rollup batch settlement
4. Dedicated edge node hardware deployment
5. Multi-site SUMI evaluation
6. Production deployment and longitudinal evaluation
7. Comparative PoA/PBFT/DPoS workload benchmarks

---

## Section B — Research Design Specification

### 1. Proposed Working Title

*"Selective Anchoring for Hierarchical Disease Surveillance in Resource-Constrained Settings: A Privacy-Layered Smart-Contract Architecture for LMIC Public Health Reporting"*

---

### 2. Research Problem

LMIC disease early warning and surveillance (DEWS) systems require multi-institutional, tamper-evident, privacy-preserving data sharing across organizations with asymmetric technical capacity — from resource-constrained community health posts to national surveillance centers. Existing architectures either centralize data (creating single-point-of-failure and governance risks) or treat blockchain as bulk storage (producing prohibitive cost, storage, and bandwidth demands). Neither class of solution adequately addresses the linked problems of privacy, resource asymmetry, completeness accountability, and cross-tier provenance in hierarchical LMIC public health systems.

---

### 3. Research Gap

No prior blockchain-health architecture simultaneously addresses all of:
1. Hybrid storage with explicit selective anchoring policy for LMIC surveillance payloads
2. A privacy model that honestly distinguishes on-chain access state from IPFS content access and key distribution
3. Asymmetric node-role design aligned to LMIC administrative hierarchies
4. A completeness verification mechanism operating on anchoring events (not on encrypted content)
5. A threat model that explicitly addresses the enforcement gap between on-chain revocation and off-chain ciphertext/key access

The specific gap is not the combination of IPFS + blockchain (already published) but the **design-principled, threat-model-grounded application to the Ethiopian PHEM administrative hierarchy** with honest treatment of privacy limitations.

---

### 4. Research Questions

**RQ1.** Can a selective-anchoring smart-contract architecture enforce hierarchical provenance and access control for LMIC disease surveillance while achieving order-of-magnitude storage reduction relative to full on-chain data recording?

**RQ2.** What are the precise semantics and limitations of blockchain-based access control when applied to IPFS-stored encrypted health data, and what additional mechanisms are required for meaningful revocation?

**RQ3.** Can a period-level completeness verification mechanism operating on anchoring events, without inspecting encrypted payloads, effectively incentivize timely reporting in a hierarchical surveillance chain?

---

### 5. Objectives

1. Design a selective-anchoring coordination architecture for five-tier LMIC disease surveillance, explicitly mapping architectural components to the Ethiopian PHEM administrative hierarchy.
2. Implement four smart contracts (ReportCoordinator, AccessControl, ReportFactory, CompletenessVerifier) with correct caller authentication, expiry semantics, and honest completeness counting.
3. Develop and execute a Hardhat test suite demonstrating functional correctness of the smart-contract coordination layer.
4. Characterize gas costs, block confirmation latency, and IPFS retrieval latency through reproducible measurements.
5. Produce a rigorous threat model that explicitly distinguishes ledger integrity guarantees from IPFS content access and key distribution.

---

### 6. Defensible Contributions (Revised List)

**C1. Selective-anchoring architecture for hierarchical LMIC surveillance.**
A principled design separating on-chain coordination (hashes, permissions, provenance, audit events) from off-chain encrypted payloads, mapped explicitly to the Ethiopian PHEM five-tier administrative structure. Novelty: application context, tier-aligned role model, and explicit treatment of enforcement limitations.

**C2. Privacy model with explicit revocation semantics.**
A formal specification of what on-chain access control revokes (contract state, API-layer authorization) versus what it does not revoke (IPFS content access, key knowledge). A design of the additional key management layer required for end-to-end revocability. If implemented: a working key distribution mechanism.

**C3. Implemented and tested smart-contract coordination layer.**
Executable Solidity contracts with caller authentication, Mocha/Hardhat test suite with logged passing results, gas cost measurements from Hardhat gas reporter, and block latency measurements from a reproducible local PoA chain. *This contribution can only be claimed after implementation and test execution.*

---

### 7. Revised Architecture

**Principle:** Blockchain as coordination and proof layer only. Zero raw health data on-chain.

**Tier 1 — Community Capture:**  
Lightweight browser-based client. AES-256-GCM encryption of surveillance payload using a per-report symmetric key. IPFS upload of ciphertext. MetaMask transaction signing. No validator responsibilities.  
*Key design decision:* The symmetric key is generated client-side and distributed separately from the CID. The CID alone is not sufficient for decryption.

**Tier 2 — Application-Layer Preprocessing:**  
Next.js API middleware performing field validation, schema conformance, local queue buffering for offline periods. Label explicitly as application-layer preprocessing, not hardware edge nodes.

**Tier 3 — District Coordination:**  
Supervisory batch aggregation. Batch anchoring transactions. Rework request management.

**Tier 4 — Smart Contract Coordination Layer (implemented):**  
Four contracts: ReportFactory, ReportCoordinator, AccessControl, CompletenessVerifier.  
All sensitive functions protected by role-based caller modifiers.  
Integrity commitment: HMAC-SHA-256(key, nonce, plaintext) — not SHA-256(plaintext).

**Tier 5 — Off-Chain Encrypted Storage:**  
IPFS with content addressing. Private IPFS cluster for data sovereignty. Payloads encrypted before upload. CID stored on-chain; symmetric key distributed through separate key management channel.

---

### 8. Trust Model

- Validators are institutional actors at Tier 3+ with stable infrastructure and institutional accountability.
- Tier 1–2 actors are trusted for submission authenticity (authenticated by Ethereum address) but not for consensus.
- The Ethereum smart contract code is trusted once deployed (immutable contracts, no upgrade keys).
- IPFS gateway operators are trusted for availability but not required for confidentiality (encryption provides confidentiality independently).
- Key distribution infrastructure is trusted for key delivery; its compromise is a system threat requiring key rotation.

---

### 9. Threat Model

| Threat | Asset targeted | Mitigation | Limitation |
|---|---|---|---|
| Unauthorized anchor submission | Provenance integrity | Reporter registration + `onlyRegisteredReporter` modifier | Compromised reporter key can submit false anchors |
| Unauthorized access grant | Access control integrity | `onlyReportOwner` modifier on grantAccess | Reporter key compromise enables false grants |
| IPFS content access by revoked party | Data confidentiality | Key rotation after revocation (design; not yet implemented) | Without key rotation, revocation is on-chain state only |
| SHA-256 re-identification from on-chain hash | Privacy | Replace with HMAC-keyed commitment | Low-entropy records remain at some risk |
| Metadata correlation (timing, reporter, tier) | Surveillance pattern privacy | Acknowledge as limitation; propose batching to reduce timing granularity | Not fully mitigated by the current design |
| Validator collusion (PoA) | Ledger integrity | f < n/2 Byzantine tolerance; institutional validator accountability | Small validator set increases collusion risk |
| Smart contract bug | All guarantees | Formal verification (future); comprehensive test suite (implemented) | Test suite does not prove correctness |
| IPFS content unavailability (GC, de-pinning) | Data availability | Pinning service; institutional IPFS nodes | IPFS availability is not guaranteed |
| Erasure requirement (regulatory) | Compliance | Cryptographic deletion (key destruction) | On-chain hashes and metadata persist |

---

### 10. Privacy Model

**A. Ledger authorization:** Smart contract access state (`permissions[reportId][grantee].granted`). Controls what the application API returns. Does NOT prevent direct IPFS retrieval.

**B. Ciphertext availability:** IPFS content is publicly accessible by anyone with the CID. Privacy depends entirely on key confidentiality, not CID confidentiality.

**C. Cryptographic decryption authorization:** Possession of the per-report symmetric key. Must be distributed through a separate key management channel (asymmetric wrapping using the grantee's public key is the standard approach).

**D. Key distribution:** Symmetric keys are distributed out-of-band using the grantee's public key (ECIES or RSA-OAEP). On-chain `grantAccess()` triggers key distribution in the application layer.

**E. Key revocation/rotation:** On `revokeAccess()`, the application layer must trigger key rotation: re-encrypt the payload under a new key using proxy re-encryption or by fetching the ciphertext, decrypting, re-encrypting, re-uploading to IPFS, updating the CID on-chain, and distributing the new key only to remaining authorized parties. This is architecturally specified; implementation complexity is non-trivial.

**Honest statement:** The current Solidity contract listings do NOT implement B, C, D, or E. They implement only A. The revised paper must clearly state this distinction.

---

### 11. Access Control Model

```
Roles: REPORTER (Tier 1–2), SUPERVISOR (Tier 3), REGIONAL (Tier 4), NATIONAL (Tier 5), ADMIN

Report ownership: The address that calls anchorReport() is the report owner.

Permissions:
- REPORT_OWNER can call grantAccess(reportId, grantee, expiry, tierScope)
- REPORT_OWNER can call revokeAccess(reportId, grantee)
- SUPERVISOR and above can call grantAccess for reports in their administrative scope
- Any address can call checkAccess() (read-only)

Scope enforcement:
- tierScope in Permission struct restricts which tier roles can receive a grant
- checkAccess() returns false if caller's tier is above the tierScope ceiling
```

This model requires:
- A role registry mapping Ethereum addresses to roles
- `onlyRole(SUPERVISOR)` or similar modifier on supervisory functions
- Tier-scope enforcement in `checkAccess()`

---

### 12. Revocation Semantics

**On-chain revocation** (implemented in listing): Sets `granted=false`. Effect: `checkAccess()` returns false. The application API (if consulting the contract) will not return the CID or key.

**What revocation does NOT do:**
- Does not prevent a party who already has the CID from fetching from IPFS directly
- Does not recall a symmetric key already distributed to the grantee
- Does not re-encrypt the payload

**Full revocation** (required for strong privacy guarantee, not yet implemented):
1. Call `revokeAccess()` on-chain
2. Fetch the encrypted payload from IPFS
3. Decrypt using current key
4. Generate a new symmetric key
5. Re-encrypt the payload
6. Upload re-encrypted payload to IPFS → new CID
7. Call an `updateAnchor()` function to register the new CID (requires contract modification)
8. Distribute the new key to all currently-authorized parties (excluding the revoked party)

This is the correct design for meaningful revocation. It adds significant operational complexity. If it cannot be implemented, the paper must state clearly that blockchain revocation is application-layer enforcement only, not cryptographic enforcement.

---

### 13. Completeness Verification Model (Corrected)

The `CompletenessVerifier` contract, as designed in the listings, operates as follows:

- **Input:** Anchor events (`anchorReport()` calls) received from registered reporting units within a defined period
- **Output:** `receivedCount / expectedCount` ratio; `GapAlert` event if ratio < threshold at period close
- **What it verifies:** Whether the expected number of anchoring transactions have been submitted within the period
- **What it does NOT verify:** Whether individual report payloads contain all required fields (this requires either trusted off-chain attestation or ZKP, neither of which is implemented)

**Honest claim:** The contract enforces *submission counting accountability* — it detects whether expected reporters have submitted anchors — but does not enforce *payload completeness*. This is still a legitimate surveillance accountability mechanism; it just needs to be described accurately.

---

### 14. Selective Anchoring Mechanism (Precise Definition)

**Selective anchoring** in this architecture means:
- Only cryptographic hashes (HMAC commitments), IPFS CIDs, access-control state, provenance events, completeness counts, and audit records are stored on-chain
- The triggering event for anchoring is the successful AES-256-GCM encryption and IPFS upload of a surveillance payload
- The anchoring policy is hierarchical: Tier 1 reporters anchor individual submissions; Tier 3 coordinators can batch-anchor multiple submissions in a single transaction
- No raw epidemiological content (case records, patient identifiers, disease codes in plaintext, geographic coordinates) is stored on-chain

---

### 15. Implementation Plan

**Phase 1 (Required for minimal defensible paper):**
1. Implement `ReportCoordinator.sol` with `onlyRegisteredReporter` modifier and `registerReporter()` admin function
2. Implement `AccessControl.sol` with `onlyReportOwner`/`onlyAdmin` modifiers; HMAC-keyed commitment instead of SHA-256(plaintext)
3. Implement `CompletenessVerifier.sol` with `closePeriod()` and `GapAlert` threshold logic completed
4. Implement `ReportFactory.sol` as coordinator registry
5. Write Hardhat test suite: ≥24 test cases covering authorization, revocation, expiry, completeness
6. Write deployment script (Hardhat ignition or migrations)
7. Run Hardhat gas reporter; record output
8. Measure block confirmation time on local Hardhat node

**Phase 2 (Required for full privacy claim):**
9. Implement client-side AES-256-GCM encryption with `crypto.subtle` (Web Crypto)
10. Implement IPFS upload/download with `ipfs-http-client`
11. Design and document key distribution mechanism (at minimum: pseudocode + design document)

**Phase 3 (Optional enhancement):**
12. Implement proxy re-encryption or key-rotation pathway for meaningful revocation
13. Implement batch anchoring in ReportFactory

---

### 16. Evaluation Methodology

**EQ1: Smart contract functional correctness**  
Method: Hardhat test suite with Mocha.  
Artefact: test file + `npx hardhat test` output logged to file.  
Claim type: functional verification.

**EQ2: Gas cost characterization**  
Method: Hardhat gas reporter plugin.  
Artefact: gas reporter output table.  
Claim type: measured (not estimated).  
Metric: gas units per function call; ETH and USD estimate at stated gas price and ETH/USD rate.

**EQ3: Block confirmation latency**  
Method: Local Hardhat/Ganache PoA network; measure `transaction.blockNumber` minus submission timestamp across N=50 transactions.  
Artefact: script + output log.  
Claim type: measured.  
Note: Label explicitly as local PoA, not Rinkeby.

**EQ4: Storage comparison**  
Method: Compute `payload_size_bytes / calldata_size_bytes` for the 10 representative report types.  
Artefact: script + output.  
Claim type: analytical calculation (not experimental, but transparent).

**EQ5: IPFS retrieval latency (optional)**  
Method: Script measuring CID resolution + content retrieval time over N=50 retrievals.  
Artefact: script + log.  
Note: Distinguish local daemon from distributed gateway. Do not extrapolate to production LMIC conditions without evidence.

**EQ6: Usability (if re-conducted)**  
Method: SUMI with actual implemented prototype; ≥26 participants; documented task list; ethics approval.  
Artefact: anonymized response data; SUMI analysis output.

---

### 17. Workload Model

For gas/latency evaluation:
- 10 representative surveillance report types based on Ethiopian PHEM reporting templates
- Sizes: single case notification (~2.4 KB plaintext), weekly district aggregate (~48.6 KB)
- Transaction types: individual anchor, batch anchor (5 reports), access grant, access revoke, completeness registration, completeness query
- N=50 transactions per type for statistical stability

For usability evaluation (if conducted):
- Representative tasks: submit a case report, grant access to a supervisor, view completeness status, request rework on a submission
- Task list pre-specified and documented

---

### 18. Baselines

| Baseline | Type of comparison | Notes |
|---|---|---|
| Direct on-chain storage (all data as calldata) | Analytical | Storage size and gas cost computation |
| Conventional centralized database with audit log | Analytical | Trust model and availability argument |
| Signed append-only transparency log | Analytical | Integrity and auditability comparison |
| Hyperledger Fabric private data collections | Analytical | Platform comparison (no benchmark required) |

Experimental baseline is not required for all comparisons; analytical comparison is scientifically sufficient if clearly labeled.

---

### 19. Metrics

| Metric | Unit | Method |
|---|---|---|
| Gas cost per anchor | gas units | Hardhat gas reporter |
| Gas cost per access grant/revoke | gas units | Hardhat gas reporter |
| Block confirmation latency | seconds | Script on local PoA |
| On-chain calldata size | bytes | Measured from transaction |
| Payload size | bytes | Measured from test payloads |
| Storage reduction ratio | dimensionless (×) | Payload / calldata |
| IPFS retrieval latency | ms | Script with timing |
| Test pass rate | fraction | Mocha output |
| SUMI scores | 10–73 per scale | SUMI instrument |

---

### 20. Required Experiments

| Experiment | Required for which claim | Status |
|---|---|---|
| Hardhat test suite run | C3 (functional correctness) | Not yet conducted |
| Hardhat gas reporter | C3 (gas cost) | Not yet conducted |
| Block latency measurement (local PoA) | C3 (performance) | Not yet conducted |
| IPFS latency (local daemon) | C3 (retrieval performance) | Not yet conducted |
| SUMI evaluation with implemented prototype | If usability claim retained | Not yet conducted |

---

### 21. Expected Tables

1. **Technology stack** — languages, frameworks, versions (can be created without experiments)
2. **Storage comparison** — payload size vs. calldata size vs. reduction ratio (requires test payloads)
3. **Gas cost table** — per-function gas cost (requires Hardhat gas reporter)
4. **Block confirmation latency** — per-transaction latency statistics (requires local chain measurement)
5. **Threat model table** — threat / mitigation / limitation (can be created without experiments)
6. **SUMI results table** — mean scores, CIs (requires usability study)
7. **Related work comparison** — dimensions vs. prior work (can be updated now)
8. **Platform comparison** — Ethereum vs. Hyperledger Fabric vs. transparency log (can be updated now)

---

### 22. Expected Figures

1. Five-tier architecture diagram (redesign from Mermaid source; create actual image)
2. Selective anchoring data flow (from Mermaid source)
3. Smart contract interaction topology (from Mermaid source)
4. Privacy layer model (replace ZKP component with key management design)
5. SDRM process diagram (straightforward to create)
6. Prototype software architecture
7. Gas cost bar chart (from gas reporter output)
8. Block latency bar chart (from measurement data)
9. SUMI mean scores with CI (from usability study)
10. SUMI boxplots (from usability study)

All placeholder figures must be replaced. Figures 1–6 can be created as vector diagrams from the Mermaid source code already in the manuscript. Figures 7–10 require experimental data.

---

### 23. Reproducibility Strategy

1. All smart contracts committed to `prototype/contracts/`
2. All test files committed to `prototype/test/`
3. Hardhat configuration committed to `prototype/hardhat.config.js`
4. `package.json` with pinned dependency versions
5. All measurement scripts committed to `evaluation/scripts/`
6. All raw output logs committed to `evaluation/results/`
7. Analysis scripts (R or Python) committed to `evaluation/analysis/`
8. `evaluation/README.md` documenting all commands to reproduce results
9. Node.js version, Solidity version, Hardhat version, IPFS version recorded in README

---

### 24. Limitations (Honest, Post-Revision)

1. Block confirmation latency measured on a local PoA chain; production multi-validator PoA performance under LMIC network conditions is untested.
2. IPFS latency measured with local daemon; distributed gateway performance under variable LMIC bandwidth is not characterized.
3. Key management design is specified but not fully implemented (Phase 2); full cryptographic revocation is a future requirement.
4. Prototype evaluated with synthetic payloads; validation against real PHEM reporting data requires institutional data access.
5. SUMI evaluation (if conducted) is single-site; multi-site generalizability is unestablished.
6. Comparative consensus evaluation is analytical; empirical workload benchmarks for PoA vs. PBFT vs. DPoS are future work.
7. ZKP, FHIR integration, Layer-2 rollup, and dedicated edge node deployment are future engineering tasks.
8. Production deployment, longitudinal operation, and staff turnover effects are not evaluated.

---

### 25. Recommended Manuscript Section Structure

```
1. Introduction
   1.1 Problem statement (LMIC DEWS architectural failures)
   1.2 Research gap (precise)
   1.3 Contributions (C1, C2, C3 — 3 contributions only)
   1.4 Paper organization

2. Background and Related Work
   2.1 Blockchain foundations for health surveillance
   2.2 Hybrid storage patterns (hash-pointer, IPFS-blockchain)
   2.3 Blockchain access control
   2.4 LMIC disease surveillance and DHIS2
   2.5 Architectural gap analysis (table with Hyperledger Fabric included)

3. Research Methodology
   3.1 Research design (SDRM)
   3.2 Scope and methodological boundaries

4. Proposed Architecture
   4.1 Design principles
   4.2 Five-tier topology
   4.3 Selective anchoring mechanism (precise definition)
   4.4 Privacy model (A–E explicit separation)
   4.5 Access control model (with honest limitations)
   4.6 Completeness verification model (event-counting, corrected)
   4.7 Threat model
   4.8 Consensus design rationale

5. Prototype Implementation
   5.1 Technology stack (with versions)
   5.2 Smart contract architecture
   5.3 Contract listings (correct, with modifiers)
   5.4 Test suite
   5.5 Deployment environment

6. Evaluation
   6.1 Evaluation framework (EQ1–EQ5)
   6.2 Functional correctness (test results)
   6.3 Gas cost and storage (gas reporter output)
   6.4 Performance (block latency, IPFS latency)
   6.5 Security analysis (threat-to-mitigation)
   6.6 Usability (SUMI, if re-conducted)

7. Discussion
   7.1 Interpretation of results
   7.2 Comparison with prior work
   7.3 Limitations

8. Conclusion and Future Work
   8.1 Summary of contributions
   8.2 Future work: ZKP, FHIR, Layer-2, edge nodes, key management, multi-site evaluation
```

---

*This blueprint is based solely on evidence in the original/ directory and is designed to produce a scientifically defensible paper. No results, implementations, or experiments are described as having occurred unless they have. No claims are made beyond what the evidence supports.*
