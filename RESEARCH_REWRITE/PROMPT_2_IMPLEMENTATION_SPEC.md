# Prompt 2 — Implementation Specification

**Authority:** Derived from `NEW_RESEARCH_BLUEPRINT.md` (Prompt 1.5 revision).  
**Constraint:** Implement only what this document specifies. Do not resurrect unsupported historical results. Do not modify `original/`.

---

## 1. Final title

**Hierarchical Selective Anchoring for LMIC Disease Surveillance: Submission Accountability and Explicit Privacy Semantics on a Blockchain Coordination Layer**

---

## 2. Final research problem / gap

**Problem:** Hierarchical LMIC disease surveillance needs multi-party provenance, scope-aware authorization, and accountability for missing/delayed unit submissions, while keeping sensitive payloads off the ledger and avoiding false equivalence between on-chain ACL flags and cryptographic confidentiality/revocation.

**Gap:** Not “Ethereum+IPFS is unused,” but under-specified joint treatment of (i) hierarchy-aware selective anchoring with asymmetric roles, (ii) event-based submission accountability without payload inspection, (iii) explicit A–E privacy/revocation semantics, and (iv) reproducible coordination-layer evaluation. Frame novelty as this combination; avoid absolute “no prior work” claims.

---

## 3. Final research questions

**RQ1.** Can a smart-contract coordination layer enforce hierarchy-aware authorization (role + administrative scope + report scope + operation) for registration, anchoring, and grant/revoke/expiry, while anchoring only coordination metadata/commitments (optional CID) and reducing on-chain footprint vs direct on-chain payload storage?

**RQ2.** Under an explicit A–E privacy model, what properties do on-chain grant/revoke/expiry provide for off-chain encrypted health data, and which confidentiality/revocation properties require key distribution and key rotation beyond ledger state?

**RQ3.** Can period-based verification of received submissions vs expected reporting units detect and account for missing or delayed anchors (ratios, gap alerts, late classification) without inspecting encrypted health fields?

Each RQ must be closed only with: **tests and/or measurements and/or labeled analytical artefacts** produced in this repo after implementation.

---

## 4. Three contributions

1. **Hierarchy-aware selective anchoring and submission accountability** (design + core contracts + completeness accounting).
2. **Explicit privacy and revocation semantics for blockchain–IPFS (or content-addressed) health data** (A–E model; honest limits of on-chain revoke).
3. **Reproducible empirical validation of the coordination layer** (tests, gas, local latency, calldata/storage, analytical threat assessment).

Do **not** treat “we wrote Solidity” as novelty. Do **not** claim Ethereum+IPFS hybrid storage as inherently novel.

---

## 5. Prototype scope

### Core (must ship)

| Component | Responsibility |
|---|---|
| `ReportFactory` | Deploy/register coordination instances or report-type configuration as needed |
| `ReportCoordinator` | Reporter registration hooks; `anchorSubmit`; report registry; provenance events |
| `AccessControl` | 5-tuple authorization; grant; revoke; expiry; views |
| `CompletenessVerifier` | Expected units; received marks; ratio; gap alert; late policy |
| Tooling | Hardhat; tests; deploy scripts; gas measurement; local latency script |
| Docs | Canonical encoding; authz matrix; A–E note; how to reproduce |

### Optional data-layer integration

- AES-256-GCM encrypt/decrypt helpers (Node or browser-crypto in scripts).
- IPFS (or local mock + real IPFS if available) upload/download.
- Persist CID on anchor.

### Key distribution

- Document message flow (wrap content key to grantee pubkey).
- Implementation optional; **do not** claim production KMS.

---

## 6. Explicit non-goals

- ZKP / SNARK completeness
- FHIR / HL7 integration
- Layer-2 rollups
- Dedicated edge/fog hardware
- Proxy re-encryption
- Full cryptographic revocation / re-encrypt-on-revoke pipeline (unless explicitly added later as extra scope)
- National-scale deployment
- Quantitative PoA vs PBFT vs DPoS benchmarks
- SUMI / human-subjects usability
- Mainnet or deprecated testnets (Rinkeby)
- Any numeric result copied from `original/` without new measurement
- Field-level inspection of encrypted epidemiological payloads
- Claims that local-chain latency is general Ethereum performance

---

## 7. Contract responsibilities

### 7.1 `ReportFactory`

- Own admin bootstrap.
- Create or register `ReportCoordinator` / linked module addresses.
- Emit factory events for discoverability.
- No payload storage.

### 7.2 `ReportCoordinator`

- `registerReporter(address, role, adminScope)` callable only by authorized admin (may delegate registry to AccessControl—pick one and test it).
- `anchorSubmit(...)` only for registered reporters with scope match.
- Store anchor record: `reportId`, `commitment`, `reporter`, `scopeId`, `periodId`, `reportingUnitId`, `timestamp`, optional `cid`, optional `integrityKeyVersion`.
- Emit `ReportAnchored` (and related) events for provenance.
- Reject duplicate `reportId` / duplicate unit-period per policy.
- Interface to notify CompletenessVerifier (call or same-tx hook).

### 7.3 `AccessControl`

- Maintain roles and **administrative scopes** (hierarchical IDs; document parent/child cover relation).
- `grant(reportId, grantee, expiry, granteeScopeConstraint, capability)` with authz checks.
- `revoke(reportId, grantee)`.
- `checkAccess(reportId, actor, capability) → bool` respecting expiry and scopes.
- Supervisory capability distinct from “content decrypt right” (decrypt is off-chain).
- **Every** mutating function authorized; no open grant/revoke as in faulty historical listings.

### 7.4 `CompletenessVerifier`

- Configure period: `expectedReportingUnits[]` or set membership; `threshold`; `deadline`; `lateWindowEnd`.
- On accepted anchor: mark unit received for period.
- `closePeriod` / view: compute ratio; emit `GapAlert` with missing units if below threshold.
- Late policy: implement **one** documented behavior (recommend: post-deadline within late window ⇒ received+LATE; after late window ⇒ reject or TOO_LATE not counting as on-time).
- **No** reading of payload fields.

Solidity version: choose a single recent 0.8.x and pin in config.

---

## 8. Authorization rules

Decision predicate:

`actor.role + actor.adminScope + report.scope + operation + grantee.scope (when applicable) → ALLOW/DENY`

**Mandatory covered scenarios (tests):**

| # | Scenario | Expected |
|---|---|---|
| 1 | Admin registers reporter in scope S | ALLOW |
| 2 | Non-admin registers reporter | DENY |
| 3 | Registered reporter anchors in own scope | ALLOW |
| 4 | Registered reporter anchors outside scope | DENY |
| 5 | Unregistered address anchors | DENY |
| 6 | Owner grants grantee in allowed scope before expiry | ALLOW; checkAccess true |
| 7 | Random address grants | DENY |
| 8 | Owner revokes | checkAccess false thereafter |
| 9 | After expiry timestamp | checkAccess false |
| 10 | Supervisor with covering scope performs permitted supervisory op | ALLOW |
| 11 | Supervisor without covering scope | DENY |
| 12 | Cross-boundary grant/read attempt | DENY |
| 13 | Unauthorized actor any mutate | DENY |
| 14 | National/regional scope cover children per documented hierarchy | ALLOW only if cover relation true |

Encode hierarchy mathematically in code (e.g., prefix scope IDs or parent mapping). Document in `prototype/docs/AUTHZ_MATRIX.md`.

---

## 9. Privacy / revocation model

Implement and document layers:

| Layer | Name | Implement? |
|---|---|---|
| A | Ledger authorization | **Yes** (core) |
| B | Ciphertext availability | Optional IPFS/mock; assume public-if-CID-known |
| C | Decryption authorization | Optional AES path |
| D | Key distribution | Document; optional scripts |
| E | Key revocation/rotation | **Do not claim**; out of core scope |

**Required comments/docs sentences:**

- On-chain revoke/expire ⇒ updates **A** only.
- Does **not** revoke known **CID** fetchability (**B**).
- Does **not** revoke previously distributed content keys (**C/D**).
- Cryptographic unreadability after revoke requires **E** (future).

---

## 10. Integrity / HMAC specification

### Canonical representation

1. Define `reportTypeId` + fixed field set in fixtures.
2. `canonicalBytes = UTF-8(CIP-JSON)` with lexicographic keys, no extra whitespace, stable number formatting.
3. Write golden vectors in tests (`canonicalBytes` hex known).

### Keys / nonce / id

- `integrityKey`: 32 bytes; test-only provisioning; **never** on-chain cleartext.
- `reportNonce`: 16+ bytes unique per submission.
- `reportId = keccak256(abi.encodePacked(reportingUnitId, periodId, reportNonce, reportTypeId))` (if encodePacked used, document length prefixes to avoid ambiguity—prefer `abi.encode`).

### Commitment

```
mac = HMAC-SHA256(integrityKey, reportNonce || canonicalBytes)
commitmentOnChain = bytes32(mac)  // if need bytes32; else store bytes32(keccak256(mac)) but pick ONE and test vectors match
```

Off-chain verification recomputes MAC; contract only stores/compares equality of provided commitment at anchor (submitter supplies commitment; optional off-chain auditor verifies).

### Rotation

- Support `integrityKeyVersion` uint16 on anchor optional field.
- Old anchors remain verifiable with old versioned key material held off-chain.
- Rotation ≠ content revocation.

### Prohibited

- `sha256(plaintext)` or `keccak256(plaintext)` as sole integrity anchor for health-ish payloads.

---

## 11. Completeness model

- **Metric:** `receivedSubmissions / expectedReportingUnits` (unit-level, per period).
- **Outputs:** ratio; missing list; `GapAlert`; late markers per §7.4.
- **Not:** epidemiological field completeness; incentive effectiveness; decryption.

---

## 12. Threat model (implementation checklist)

Tests or analysis notes must address:

1. Unauthorized anchor / grant / revoke  
2. Scope escape  
3. Expiry bypass attempts  
4. Commitment forgery without integrity key (should fail off-chain verify; on-chain accepts only authorized submitter’s bytes—document trust assumption)  
5. Revoke leaves CID/key usable (document residual risk)  
6. Metadata leakage residual (addresses, times, CIDs)  
7. Completeness gaming: double-count same unit (must be idempotent)

---

## 13. Tests

Framework: Hardhat + Mocha/Chai (or Hardhat bare tests).

**Minimum suites:**

1. `authz.matrix.test.js` — all rows in §8  
2. `anchor.provenance.test.js` — events, duplicate rules  
3. `access.grant_revoke_expiry.test.js`  
4. `completeness.gaps_late.test.js`  
5. `commitment.hmac.vectors.test.js` (off-chain helper + stored commitment match)  
6. Optional: `ipfs_roundtrip.test.js` skipped if no daemon  

**Rule:** Never claim N/N pass without running and saving log under `evaluation/results/`.

---

## 14. Experiments

| ID | Experiment | Environment |
|---|---|---|
| E1 | Full test suite run | Hardhat local |
| E2 | Gas measurement per core op | Hardhat gas reporter / custom |
| E3 | Latency of txs under local PoA or Hardhat automine/interval mining | Document blockTime |
| E4 | Calldata/storage size vs direct payload baseline | Script on fixtures |
| E5 | Optional IPFS retrieval timing | Local daemon only unless documented otherwise |
| E6 | Analytical threat/privacy write-up | Markdown in evaluation/analysis |

**Do not** label E3 as public Ethereum performance.

---

## 15. Metrics

- Test pass/fail counts  
- Gas units / op  
- Latency distribution (mean, sd, p50, p95, min, max) for chosen N  
- Anchor calldata bytes; baseline payload bytes; ratio  
- Completeness ratio; gap count; late count  
- Optional IPFS ms  

N (e.g., 50) = **distribution characterization**, not proof of statistical stability.

---

## 16. Evidence artefacts

| Artefact | Path (required convention) |
|---|---|
| Contracts | `prototype/contracts/*.sol` |
| Tests | `prototype/test/**` |
| Deploy | `prototype/scripts/deploy.js` (or equivalent) |
| Encoding doc | `prototype/docs/CANONICAL_ENCODING.md` |
| Authz doc | `prototype/docs/AUTHZ_MATRIX.md` |
| Privacy note | `prototype/docs/PRIVACY_AE_MODEL.md` |
| Test logs | `evaluation/results/hardhat-test.log` |
| Gas | `evaluation/results/gas-report.*` |
| Latency | `evaluation/results/latency-*.json` |
| Storage | `evaluation/results/storage-baseline.*` |
| Repro instructions | `evaluation/README.md` + `prototype/README.md` |
| Threat analysis | `evaluation/analysis/threat_model.md` |

---

## 17. Historical-vs-new evidence rule

| Source | Treatment |
|---|---|
| `original/` narrative metrics | **Historical manuscript claims only** — unsupported for results sections |
| New `prototype/` + `evaluation/` outputs | **Only** source of empirical claims |

Do not mix timelines in tables. If mentioning history, label “unsupported prior claim.”

---

## 18. Claims allowed in the final paper (after evidence exists)

- Design of hierarchy-aware selective anchoring and submission accountability as implemented.
- Test-backed authorization and completeness behaviors.
- Measured gas/latency/calldata **in the documented local environment**.
- Analytical storage comparison with stated assumptions.
- A–E privacy semantics and limitations as documented.
- Analytical baseline comparisons (DB, transparency log, Fabric) labeled analytical.

---

## 19. Claims prohibited without additional evidence

- Any original SUMI / Rinkeby / IPFS 340 ms / gas dollar ranges / 24/24 as facts  
- ZKP, FHIR, L2, edge hardware, PRE done  
- Cryptographic or end-to-end revocation via on-chain revoke alone  
- Field-level completeness of encrypted reports  
- “Incentivizes timely reporting” behavioral outcomes  
- National deployment / production readiness  
- Absolute priority novelty (“first system ever…”) without literature proof  
- Generalizing local PoA latency to Ethereum mainnet or LMIC WAN IPFS  
- N=50 ⇒ statistical stability / general population inference  

---

## 20. Definition of done (Prompt 2)

Prompt 2 is **done** when all of the following hold:

1. Core four contracts compile under pinned Hardhat/solc.  
2. Authorization 5-tuple enforced; §8 scenarios have automated tests.  
3. HMAC commitment path implemented per §10 with golden vectors.  
4. CompletenessVerifier implements expected/received/gap/late **without** payload field checks.  
5. A–E privacy doc committed; revoke limitations stated in code comments or docs.  
6. Deploy script runs on local network.  
7. `npx hardhat test` (or equivalent) executed; log saved under `evaluation/results/`.  
8. Gas and local latency and storage baseline scripts run; outputs saved.  
9. `evaluation/README.md` lists versions and exact commands.  
10. No files under `original/` modified.  
11. No historical numeric results copied into “Results.”  
12. Optional IPFS/AES either working with tests or clearly marked not implemented.  
13. Non-goals §6 not presented as completed features.

---

## RQ → evaluation → metric → artefact (quick map)

| RQ | Evaluation | Metric | Artefact |
|---|---|---|---|
| RQ1 | Authz+anchor tests; gas; calldata vs baseline | pass/fail; gas; bytes; ratio | tests, gas-report, storage-baseline |
| RQ2 | Grant/revoke/expiry tests; A–E doc; threat note | state correctness; limitation coverage | tests, PRIVACY_AE_MODEL.md, threat_model.md |
| RQ3 | Completeness scenarios | ratio; gaps; late flags | completeness tests + logs |

---

*End of Prompt 2 specification. Proceed to implementation only within this boundary.*
