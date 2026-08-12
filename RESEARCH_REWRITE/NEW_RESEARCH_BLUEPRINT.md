# New Research Blueprint (Revised — Prompt 1.5)

**Manuscript working title:** Hierarchical Selective Anchoring for LMIC Disease Surveillance: Submission Accountability and Explicit Privacy Semantics on a Blockchain Coordination Layer  
**Prepared:** 2026-08-12  
**Revised:** 2026-08-12 (Prompt 1.5 — design refinement only; no prototype implementation)  
**Status:** Implementation-ready research design. No results, deployments, or experiments are claimed as completed.

---

## 0. Evidence Separation Rule (Binding)

| Category | Definition | Use in paper |
|---|---|---|
| **Historical manuscript claims** | Numbers, tests, SUMI, Rinkeby, gas, IPFS latency stated in `original/` without reproducible artefacts | Cite only as *unsupported prior manuscript claims*; never as results |
| **New reproducibility evidence** | Artefacts produced under `prototype/` and `evaluation/` after Prompt 2+ | Sole basis for quantitative claims in the revised paper |
| **Analytical claims** | Storage ratios, threat tables, design comparisons derived without runtime measurement | Allowed if labeled analytical and reproducible from stated assumptions |

**Never** present newly generated evidence as historical evidence. **Never** carry forward unsupported historical figures (gas ranges, 24/24 tests, Rinkeby 12–14 s, IPFS 340 ms, SUMI 60.46–68.96, storage “measured” as experiment without scripts).

---

## Section A — Defensible Claims Assessment

### A. Defensible immediately (no new experiments)

1. **Problem framing:** LMIC DEWS systems face centralization risk, weak cross-organization provenance, resource asymmetry across administrative tiers, and tension between auditability and confidentiality. Defensible from public-health and digital-health literature (to be verified in bibliography rebuild).
2. **Architectural design rationale:** Using a blockchain as a *coordination and accountability layer* (anchors, authorization state, provenance events, submission counts) while keeping encrypted payloads off-chain is a coherent design response. Defensible analytically; **not** claimed as inherently novel solely because it uses Ethereum + IPFS.
3. **Tier mapping:** Mapping roles and scopes to a five-tier public-health hierarchy (community → facility/application → district → regional → national), informed by Ethiopian PHEM-style organization, is a defensible design choice when labeled as a design mapping.
4. **Analytical storage comparison:** Ratio of representative encrypted/plain payload sizes to on-chain anchor calldata size is analytically derivable if payloads and encoding are specified and labeled analytical.
5. **Threat model (analytical):** Threat-to-mitigation tables are defensible when limitations (especially revocation and metadata leakage) are stated honestly.
6. **Consensus design rationale:** Choosing a local permissioned/PoA-style environment for prototype measurement is a design/engineering choice; comparative PoA/PBFT/DPoS remains analytical unless measured.
7. **Completeness as submission accountability:** Period-level counting of expected vs received anchors is a legitimate accountability mechanism when **not** described as epidemiological field completeness.

### B. Claims requiring implementation (Prompt 2 core)

1. Role- and **scope-aware** AccessControl (not generic OpenZeppelin roles alone).
2. ReportCoordinator with reporter registration and authorized anchor submission.
3. ReportFactory as deployment/registry coordinator as needed by the design.
4. CompletenessVerifier for expected-unit submission accounting and gap alerts.
5. HMAC-based integrity commitment (not plaintext SHA-256).
6. Hardhat tests, deployment scripts, gas reporter outputs, local-PoA latency scripts/logs.

### C. Claims requiring optional data-layer integration

1. AES-256-GCM encrypt/decrypt of synthetic payloads.
2. IPFS (or local IPFS-compatible) upload/download and CID recording.
3. Optional IPFS retrieval latency measurements under documented conditions.

### D. Claims requiring stronger analysis only

1. Precise A–E privacy separation and revocation honesty.
2. Metadata leakage analysis (addresses, timing, tier, CID presence).
3. Blockchain necessity vs centralized DB + audit log vs signed transparency log.
4. Analytical comparison to Hyperledger Fabric private data collections.

### E. Remove from current contributions / results

1. ZKP as contribution or implemented feature.
2. FHIR workflows as contribution.
3. Quantitative multi-consensus benchmarking as contribution.
4. Dedicated edge/fog hardware as contribution.
5. Any historical gas, Rinkeby, IPFS, SUMI, or 24/24 figures without new artefacts.
6. End-to-end cryptographic revocation unless implemented and tested.
7. National-scale deployment claims.

### F. Future work (explicit non-contributions now)

ZKP; FHIR; Layer-2 rollups; dedicated edge hardware; proxy re-encryption / full cryptographic revocation; national-scale deployment; quantitative PoA/PBFT/DPoS benchmarking; multi-site usability; production longitudinal operation.

---

## Section B — Research Design Specification

### 1. Final working title

**Hierarchical Selective Anchoring for LMIC Disease Surveillance: Submission Accountability and Explicit Privacy Semantics on a Blockchain Coordination Layer**

Shorter running head (optional): *Hierarchical Selective Anchoring for LMIC Surveillance*

### 2. Research problem

Hierarchical LMIC disease surveillance requires **tamper-evident provenance**, **cross-tier accountability for missing or delayed reports**, and **controlled sharing of sensitive payloads** among actors with unequal infrastructure. Centralized systems concentrate trust and weaken independent audit. Naïve “put health data on blockchain” designs impose untenable cost/bandwidth and expose content or low-entropy integrity digests. Prior hybrid designs often **conflate ledger authorization with off-chain confidentiality** and overclaim completeness or revocation.

### 3. Research gap (tightened novelty; non-absolute)

Prior work extensively covers blockchain–health hybrids, IPFS hash-pointer patterns, and smart-contract ACLs. What remains under-specified for **hierarchical public-health reporting** is a single design that jointly treats:

1. **Hierarchy-aware selective anchoring** with asymmetric participation (who may submit, supervise, grant, and audit);
2. **Event-based submission accountability** (expected reporting units vs received anchors; gap detection) without inspecting encrypted fields;
3. **Explicit separation** of ledger authorization, ciphertext availability, decryption authorization, key distribution, and key revocation/rotation;
4. **Reproducible evaluation of the coordination layer** (correctness, gas, controlled latency, storage/calldata) rather than unreproducible testnet anecdotes.

Novelty is framed as this **combination and honest semantics** for LMIC hierarchical surveillance—not as “Ethereum + IPFS is new,” and **not** as an absolute claim that no prior system shares any component. Literature comparison must include stronger baselines (e.g., Fabric private data, MedRec-class systems, transparency logs) without self-serving all-Yes tables.

### 4. Research questions (answerable chain: RQ → evidence → evaluation → conclusion)

**RQ1 — Hierarchy-aware selective anchoring and authorization.**  
Can a smart-contract coordination layer enforce **role + administrative scope + report scope + operation** authorization for registration, anchoring, grant/revoke/expiry, and supervisory reads, while anchoring only commitments/CIDs/provenance—not raw surveillance payloads—and reducing on-chain footprint relative to direct on-chain storage of equivalent payloads?

| Link | Artefact |
|---|---|
| Evidence | Contract logic + tests + calldata/gas vs analytical full-payload baseline |
| Evaluation | Functional authorization matrix tests; storage/calldata measurement; gas reporter |
| Conclusion | Pass/fail authorization properties; measured/analytical footprint reduction **on the evaluated prototype environment** |

**RQ2 — Privacy and revocation semantics.**  
Under an explicit A–E model, what confidentiality and revocation properties does the architecture provide when payloads are encrypted off-chain and only authorization state and integrity commitments are on-chain—and which properties **require** key distribution/rotation beyond on-chain `revoke`/`expire`?

| Link | Artefact |
|---|---|
| Evidence | Design specification + tests of on-chain grant/revoke/expiry + analytical (and optional integration) threat assessment |
| Evaluation | Authorization state tests; documented non-goals for CID/key recall; threat table |
| Conclusion | Precise claim boundaries: ledger auth ≠ cryptographic revocation |

**RQ3 — Submission accountability (replaces “incentivize timely reporting”).**  
Can period-based verification of **received submissions versus expected reporting units** detect and account for **missing or delayed** anchors (gap alerts / late classification) without decrypting or field-inspecting health payloads?

| Link | Artefact |
|---|---|
| Evidence | CompletenessVerifier + tests with synthetic expected sets and delayed/missing actors |
| Evaluation | Correct counts, ratios, gap events, late flags under scripted scenarios |
| Conclusion | Accountability properties of the coordination layer—not behavioral “incentives,” not epidemiological completeness |

### 5. Objectives

1. Specify and implement hierarchy- and scope-aware selective anchoring contracts.
2. Specify integrity commitments via HMAC over a canonical encoding (not plaintext SHA-256).
3. Implement submission-accountability completeness verification with gap/late detection.
4. Document A–E privacy/revocation semantics without overclaiming.
5. Produce reproducible Hardhat tests, deployment scripts, gas and local-PoA latency measurements, and evaluation logs.
6. Optionally integrate AES-GCM + IPFS CID anchoring for end-to-end data-path demos.
7. Keep historical manuscript claims quarantined from new evidence.

### 6. Three contributions (revised)

**C1. Hierarchy-aware selective anchoring and submission accountability.**  
A coordination-layer design and prototype in which hierarchical public-health roles, administrative scopes, and report scopes govern who may register, anchor, supervise, and manage grants; only selective anchors (integrity commitment, optional CID, provenance metadata, authorization state, submission counts) appear on-chain; **CompletenessVerifier** accounts for expected vs received unit submissions and surfaces gaps/delays.

**C2. Explicit privacy and revocation semantics for blockchain–IPFS health data.**  
A precise A–E separation (ledger authorization; ciphertext availability; decryption authorization; key distribution; key revocation/rotation) stating that on-chain revoke/expire **do not** invalidate a known CID or a previously distributed key. Cryptographic revocation remains architecture/future work unless separately implemented.

**C3. Reproducible empirical validation of the coordination layer.**  
Executable contracts, tests, deployment scripts, gas measurements, controlled local-PoA latency characterization, storage/calldata metrics, and analytical security assessment—with all quantitative claims tied to committed artefacts. **Implementing Solidity is engineering necessary for C3, not the novelty claim itself.**

### 7. Architecture (implementation-facing)

**Principle:** Blockchain = coordination, authorization state, provenance, and submission accountability. Encrypted health payloads stay off-chain.

| Layer | Responsibility | Prompt 2 |
|---|---|---|
| Clients / reporters | Build canonical report encoding; compute HMAC commitment; optional encrypt+IPFS | Core commitment path required; encrypt/IPFS optional |
| Application preprocessing | Schema checks, offline queue (design); not “edge hardware” | Document only unless needed for optional path |
| **ReportFactory** | Create/register coordinator instances or report-type registries as designed | Core |
| **ReportCoordinator** | Register reporters; accept anchors; emit provenance; bind reportId → commitment/CID/scope | Core |
| **AccessControl** | Evaluate authorization 5-tuple; grant/revoke/expire; supervisory scope checks | Core |
| **CompletenessVerifier** | expected units, received count, ratio, gap alert, late detection | Core |
| Off-chain store | IPFS or mock content store for ciphertext | Optional integration |
| Key channel | Wrap/distribute per-report keys out-of-band | Documented architecture; implementation optional |

**Asymmetric participation:** Tier-1/2 submit and hold no consensus duty in the prototype trust model. Supervisory tiers gain scope-bounded grant/read rights. Validator set (local PoA) is an evaluation substrate, not a claimed national deployment.

### 8. Trust model

- Institutional validators (evaluation network) honest-majority assumption for ledger integrity.
- Reporter keys authenticate submission identity; compromise ⇒ false anchors from that identity.
- Contract bytecode trusted as deployed (no silent upgrade path in prototype unless explicitly documented).
- IPFS operators affect availability, not confidentiality of well-encrypted payloads.
- Key channel compromise is catastrophic for confidentiality of affected reports; requires rotation design (future/optional).

### 9. Threat model (summary)

| Threat | Mitigation in scope | Explicit limit |
|---|---|---|
| Unauthorized anchor | Registration + role/scope checks | Stolen reporter key |
| Unauthorized grant/revoke | Owner/supervisor-scope rules | Stolen owner/supervisor key |
| Callerless ACL (historical listing bug) | Mandatory authz on mutating calls | — |
| Plaintext hash grinding | HMAC with integrity key + nonce/reportId | Integrity key leak |
| Revoked user with old key/CID | On-chain deny for honest apps | **No** crypto unreadability without rotation |
| Metadata profiling | Minimize on-chain fields; document residue | Timing/address/CID metadata remain |
| Missing/late reporting | Completeness gap/late events | Does not prove field quality |
| Contract defects | Tests + simple design | Tests ≠ formal verification |
| Content unavailability | Pinning assumptions documented | Not guaranteed by chain |

### 10. Privacy model (A–E) — binding

| ID | Name | Meaning | Prototype |
|---|---|---|---|
| **A** | Ledger authorization | `granted`/expiry/role-scope state consulted by honest applications and tests | **Implemented on-chain** |
| **B** | Ciphertext availability | Party with CID may fetch ciphertext from content network/store | Optional IPFS; treat as available if CID known |
| **C** | Decryption authorization | Ability to decrypt = possession of content key (and successful AEAD) | Optional crypto path |
| **D** | Key distribution | How content keys reach grantees (e.g., public-key wrap out-of-band) | **Documented**; implement only if optional path built |
| **E** | Key revocation/rotation | Making old keys insufficient (re-encrypt, new CID, redistribute) | **Not claimed**; future work |

**Binding honesty statements for Prompt 2 and the paper:**

- On-chain `revoke` / `expire` affect **A** only.
- They do **not** remove **B** for a known CID.
- They do **not** retract **C** if the key was already distributed.
- Do not claim end-to-end or cryptographic revocation unless **E** is implemented and tested.

### 11. Access control model (5-tuple)

Authorization decision:

```
Allow(actor, op, report) iff
  ValidRegistration(actor) when required
  ∧ RoleAllows(actor.role, op)
  ∧ ScopeCovers(actor.adminScope, report.scope)   // hierarchical administrative scope
  ∧ ReportScopeAllows(report.scope, op, grantee.scope?) // for grants: grantee scope constraints
  ∧ OperationSpecificRules(op, report, actor)
```

**Roles (minimum):** `ADMIN`, `REPORTER`, `SUPERVISOR`, `REGIONAL`, `NATIONAL` (names may map 1:1 to tier enums).

**Operations that MUST be enforced in tests:**

| Operation | Who (illustrative rules) |
|---|---|
| `registerReporter` | `ADMIN` (or bootstrap admin) only |
| `anchorSubmit` | Registered `REPORTER` whose **adminScope** matches **report.scope** (or allowed child scope policy as specified in code comments) |
| `grant` | Report owner **or** supervisor+ with scope covering report; grantee must be eligible for requested capability and scope |
| `revoke` | Same as grant authority class for that report |
| `expire` / time check | `checkAccess` false after `expiry`; optional keeper/anyone calling a pure view |
| `supervisoryAccess` | Supervisor+ with covering scope may read authorization/provenance needed for oversight per policy (not automatic ciphertext rights) |
| `crossBoundaryAccess` | Actor whose scope does **not** cover report.scope → **DENY** for mutate and for privileged reads |
| `unauthorizedActor` | No role / unknown address → **DENY** all mutating ops |

**Do not** implement “security” solely as unscoped `onlyRole` modifiers. Scope is first-class.

**Report identity:** `reportId` (bytes32) derived from canonical fields or assigned at anchor time; owner recorded at first accepted anchor.

### 12. Integrity commitment (HMAC) — unambiguous specification

Replace plaintext `SHA-256(payload)`.

#### 12.1 Canonical data representation `canonicalBytes`

- UTF-8 JSON **or** tightly specified field concatenation; **one** MUST be chosen in Prompt 2 and frozen in `prototype/docs/CANONICAL_ENCODING.md`.
- Recommended default: **CIP-JSON**: object keys sorted lexicographically, no insignificant whitespace, numbers in minimal decimal form, fixed field set for each `reportTypeId`.
- Include at least: `reportTypeId`, `reportingUnitId`, `periodId`, `schemaVersion`, and payload body fields used in tests.
- `canonicalBytes = UTF8(CIP-JSON(report))`.

#### 12.2 Keys and identifiers

| Item | Definition |
|---|---|
| `integrityKey` | 256-bit key **not** placed on-chain in cleartext. In tests: provisioned per reporting domain or per test fixture. Distinct from optional AES content key when both exist. |
| `reportNonce` | 128-bit unique per report attempt (or monotic counter ‖ unitId ‖ periodId); bound into MAC. |
| `reportId` | `keccak256(abi.encode(reportingUnitId, periodId, reportNonce, reportTypeId))` or equal documented derivation; unique in coordinator. |

#### 12.3 What is committed

```
mac = HMAC-SHA-256(integrityKey, reportNonce || canonicalBytes)
commitment = bytes32(mac)   // or keccak256(mac) if bytes32 packing required—document choice
```

On-chain anchor stores at minimum:

- `reportId`
- `commitment`
- `reporter` address
- `scopeId` / hierarchical scope
- `timestamp` (block time)
- `optional cid` (`bytes`/`string`, empty if data-layer skipped)
- `periodId`
- `reportingUnitId` (or hash thereof if size-sensitive—document)

**Never** store `integrityKey` or plaintext payload on-chain.

#### 12.4 Verification model

- **Off-chain verifier** with `integrityKey` recomputes `canonicalBytes` and HMAC; compares to on-chain `commitment`.
- Smart contracts **do not** recompute HMAC over plaintext (they never see plaintext).
- Optional: store `commitmentAlgorithmId = 1` for HMAC-SHA-256-v1 to allow future algs without ambiguity.

#### 12.5 Key rotation effect

- Rotating `integrityKey` **does not** rewrite historical anchors; old commitments verify only with the key version applicable at submission time.
- Maintain `integrityKeyVersion` off-chain (and optionally a version id on-chain) so verifiers select the correct key.
- Rotation is **not** content revocation; it limits forging new valid commitments under the old key once reporters receive the new key via the admin channel.

### 13. Completeness model (submission accountability only)

**In scope:**

- Admin/supervisor defines `periodId → expectedReportingUnitSet` (or expected count with registered unit list).
- On each accepted `anchorSubmit` for that period/unit, mark unit received (idempotent per unit/period policy: first valid anchor counts).
- `ratio = receivedUnits / expectedUnits`.
- At `closePeriod` (or query): if `ratio < threshold`, emit `GapAlert(periodId, missingUnits[])`.
- **Delayed submission:** if anchor arrives after `periodDeadline` but before `lateWindowEnd`, count as `LATE` (still received for accountability) or per documented policy; if after `lateWindowEnd`, reject or count `TOO_LATE` without satisfying on-time ratio—**document one policy in code**.

**Out of scope:**

- Inspecting encrypted or plaintext clinical fields for mandatory epidemiology completeness.
- Claiming behavioral “incentives” without a behavioral study.
- ZKP proofs of field completeness.

### 14. Selective anchoring (definition)

On-chain data are limited to coordination artefacts: commitments, optional CIDs, scopes, roles, permissions, provenance events, period accounting. Anchoring is “selective” in **content class** (metadata vs payload) and **policy** (who may anchor which scope; optional batching later). It is **not** marketed as a new storage primitive beyond hash-pointer hybrids; specificity is hierarchical surveillance accountability + honest privacy boundaries.

### 15. Implementation scope

#### 15.1 Core prototype (required)

- Solidity: `ReportCoordinator`, `AccessControl`, `ReportFactory`, `CompletenessVerifier`
- Reporter registration; role/scope authorization; grant/revoke/expiry
- Provenance events; submission-counting completeness + gap/late behavior
- Hardhat project: compile, test, deploy scripts
- Gas measurements (reporter plugin or equivalent)
- Controlled **local PoA / local network** latency measurements
- Docs: encoding, A–E model, authz matrix

#### 15.2 Optional data-layer integration

- AES-256-GCM
- IPFS upload/download
- CID field populated on anchor

#### 15.3 Documented, not required to implement

- Key distribution protocol detail (message formats, KMS)
- Application UI / Next.js
- Full key rotation / re-encrypt pipeline

#### 15.4 Explicit non-goals (Prompt 2)

ZKP; FHIR; Layer-2; edge hardware; proxy re-encryption; full cryptographic revocation; national deployment; multi-consensus benchmarks; SUMI / human subjects; mainnet/Rinkeby; reusing historical numeric results.

### 16. Evaluation design

**Mapping rule:** every retained RQ maps to evaluation → metric → artefact.

| RQ | Evaluation | Metrics | Artefact paths (planned) |
|---|---|---|---|
| RQ1 | Authz matrix tests; anchor success/fail; calldata size; gas | pass/fail cases; gas units; bytes; reduction ratio vs full payload baseline | `prototype/test/*`, `evaluation/results/gas_*`, `evaluation/results/storage_*` |
| RQ2 | Grant/revoke/expiry tests; threat/privacy narrative; optional encrypt path | correct A-state transitions; documented B–E limits | tests + `evaluation/analysis/threat_model.md` |
| RQ3 | Missing unit, late unit, full compliance scenarios | counts, ratio, alerts, late flags | `prototype/test/completeness*`, logs under `evaluation/results/` |

**Methods allowed:**

1. Functional correctness (Hardhat).
2. Gas measurements (local).
3. Local controlled latency (document block time config; **do not** generalize as “Ethereum performance”).
4. Storage/calldata measurements.
5. Optional IPFS retrieval measurements (document daemon vs gateway).
6. Analytical security/threat assessment.

**N=50:** If used, characterize **observed distributions** (mean, sd, min, max, percentiles). **Do not** claim “statistical stability” or population inference solely from N=50 local runs.

**SUMI:** Optional future only. **Do not** reuse unsupported historical SUMI numbers.

### 17. Workload model

- Synthetic report types (≥3): small case notification, medium aggregate, larger weekly aggregate (sizes documented).
- Ops: register, anchor, grant, revoke, expire-elapse, completeness register/close/query.
- Latency sample size: configurable; default up to N=50 per op type for distribution description.
- No real patient data.

### 18. Baselines

| Baseline | Comparison type in revised paper |
|---|---|
| Direct on-chain storage of payload bytes | **Quantitative** (gas/calldata vs anchor) when measured/calculated consistently |
| Centralized DB + audit log | Analytical (trust, integrity, multi-party audit) |
| Signed transparency log | Analytical (programmability of ACL/completeness vs bare log) |
| Hyperledger Fabric private data | Analytical platform comparison |

Only direct on-chain storage is a required quantitative baseline unless others are actually benchmarked.

### 19. Metrics

| Metric | Unit | Notes |
|---|---|---|
| Test outcomes | pass/fail per case | Commit logs |
| Gas per op | gas | Local EVM |
| Latency | ms or s | Local chain only; label environment |
| Calldata / storage footprint | bytes | Per tx / per anchor |
| Reduction ratio | × | vs direct payload baseline |
| Completeness ratio | [0,1] | received/expected |
| Gap/late events | count | Per scenario |
| IPFS latency (optional) | ms | Conditional |

### 20. Required experiments (new evidence only)

| Experiment | Status at blueprint time |
|---|---|
| Full Hardhat test suite | Not run (to be created) |
| Gas reporter | Not run |
| Local latency | Not run |
| Storage/calldata script | Not run |
| Optional IPFS | Not run |
| SUMI | Out of scope unless new ethics-backed study |

### 21–22. Tables and figures (planning)

Prefer artefacts generated from scripts. No fabricated charts. Architecture and A–E diagrams may be drawn without experiments. Omit SUMI figures unless a new study exists.

### 23. Reproducibility strategy

```
prototype/contracts/
prototype/test/
prototype/scripts/
prototype/hardhat.config.js
prototype/package.json  # pinned versions
prototype/docs/CANONICAL_ENCODING.md
prototype/docs/AUTHZ_MATRIX.md
evaluation/scripts/
evaluation/results/
evaluation/analysis/
evaluation/README.md    # exact commands, tool versions, hardware notes
```

Record: Node, solc, Hardhat, OS; network config (block time); seeds if any; commands; output files.

### 24. Limitations (honest)

Local chain ≠ public Ethereum performance; optional IPFS local ≠ LMIC WAN; key distribution/rotation may be design-only; synthetic data only; no national pilot; no ZKP/FHIR/L2; on-chain revoke ≠ crypto revoke; completeness ≠ field completeness; metadata leakage remains; historical manuscript metrics unreproduced.

### 25. Manuscript structure (unchanged intent, tightened claims)

Intro (problem, gap, C1–C3) → Related work (strong comparators) → Methods/scope → Architecture (selective anchoring, A–E, authz 5-tuple, HMAC, completeness) → Implementation (only what exists) → Evaluation (RQ-mapped, new evidence) → Discussion → Conclusion/future work.

Label every quantitative statement with evidence pointer. Separate “Historical claims (unsupported)” footnote or appendix if discussing the original manuscript.

### 26. Claims policy

**Allowed after Prompt 2 success:** statements directly supported by tests/logs/scripts listed in the evaluation map; analytical claims labeled as such; A–E limitations.

**Prohibited without additional evidence:** historical gas/SUMI/Rinkeby/IPFS numbers as facts; ZKP/FHIR/L2/edge as done; crypto revocation; field-level completeness; incentive/behavior claims; mainnet performance; “first ever” absolute novelty; national deployment success.

---

*This revised blueprint supersedes the Prompt 1 blueprint for implementation planning. It invents no results. `original/` remains immutable baseline.*
