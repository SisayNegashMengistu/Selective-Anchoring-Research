# Blueprint Revision Log — Prompt 1.5

**Date:** 2026-08-12  
**Inputs read:** `AUDIT_REPORT.md`, `CLAIM_EVIDENCE_MATRIX.md`, `NEW_RESEARCH_BLUEPRINT.md` (Prompt 1), `REVIEWER_QUESTIONS.md`, `LITERATURE_GAP_PLAN.md`  
**Outputs:** Revised `NEW_RESEARCH_BLUEPRINT.md`; new `PROMPT_2_IMPLEMENTATION_SPEC.md`; this log  
**`original/` modified:** No  

---

## Purpose

Refine Prompt 1 design artefacts so Prompt 2 can implement a minimal, scientifically defensible prototype without ambiguity, unsupported historical metrics, or overclaimed privacy/completeness/novelty.

---

## Major changes

### 1. Research questions

| Before (Prompt 1) | After (Prompt 1.5) | Rationale |
|---|---|---|
| RQ3: completeness mechanism “effectively **incentivize timely reporting**” | RQ3: **detect/account for missing or delayed** submissions via expected vs received units, gap alerts, late classification | Prototype cannot answer behavioral incentive questions; can answer accountability detection |
| RQ1–RQ3 loosely tied to eval | Each RQ has explicit **evidence → evaluation → conclusion** chain | Journal defensibility; Prompt 2 traceability |
| — | RQ2 centered on **A–E semantics and revoke limits** | Matches audit P0 revocation/IPFS failures |

### 2. Contributions

| Before | After |
|---|---|
| C1 selective-anchoring architecture | C1 **hierarchy-aware selective anchoring and submission accountability** |
| C2 privacy model with revocation semantics | C2 **explicit privacy and revocation semantics** (A–E; no crypto-revoke claim) |
| C3 “implemented and tested smart-contract coordination layer” (Solidity-as-contribution tone) | C3 **reproducible empirical validation of the coordination layer** (tests/gas/latency/storage/analysis)—implementation is means, not novelty |

Removed contribution inflation: ZKP, FHIR, consensus benchmark, edge hardware remain future work.

### 3. Novelty framing

- **Before:** Risk of “no prior work simultaneously…” absolute gap list.  
- **After:** Novelty = **combination** of hierarchical roles, selective anchoring, asymmetric participation, event-based submission accountability, and ledger-vs-crypto separation; **no** “Ethereum+IPFS is novel”; **no** absolute priority claims without literature proof; require stronger comparators (Fabric, transparency logs, MedRec-class).

### 4. Security / privacy model

- Retained A–E separation with a **binding honesty table**.  
- Explicit: on-chain revoke/expire ≠ CID invalidation ≠ key recall.  
- Key distribution documented; full rotation/PRE **out of core claim surface**.  
- Integrity: plaintext SHA-256 path **prohibited**.

### 5. Integrity / HMAC design

- **Before:** `HMAC-SHA-256(key, nonce, plaintext)` left ambiguous.  
- **After:** Specified **canonical CIP-JSON (or frozen alternative)**, `integrityKey`, `reportNonce`, `reportId` derivation, `mac = HMAC-SHA256(key, nonce || canonicalBytes)`, on-chain fields, off-chain verify model, **key version / rotation effects**, golden-vector requirement.

### 6. Access control

- **Before:** Role modifiers + tierScope sketch; risk Prompt 2 ships generic `onlyRole`.  
- **After:** Mandatory **5-tuple** `role + adminScope + reportScope + operation + granteeScope → ALLOW/DENY` with enumerated scenarios (register, anchor, grant, revoke, expiry, supervisory, cross-boundary, unauthorized).

### 7. Completeness

- Kept corrected model: **submission accountability only**.  
- Added **late vs missing** policy requirement (one documented behavior).  
- Forbids field inspection and incentive language.

### 8. Implementation scope

- Core four contracts + registration + authz + completeness + Hardhat tests/deploy/gas/local latency.  
- Optional: AES-GCM, IPFS, CID.  
- Future: ZKP, FHIR, L2, edge HW, PRE, full crypto revoke, national deploy, multi-consensus benches, SUMI.

### 9. Evaluation design

- Forced `RQ → evaluation → metric → artefact`.  
- Local latency **must not** be called general Ethereum performance.  
- N=50 = distribution characterization, **not** “statistical stability.”  
- SUMI optional; historical SUMI **banned** as evidence.  
- Quantitative baseline: **direct on-chain storage** only unless others measured; DB/log/Fabric analytical.

### 10. Historical vs new evidence

- New binding Section 0 / Spec §17.  
- Historical manuscript metrics quarantined; new results only from `prototype/` + `evaluation/`.

### 11. Title

- **Before:** *Selective Anchoring for Hierarchical Disease Surveillance in Resource-Constrained Settings: A Privacy-Layered Smart-Contract Architecture for LMIC Public Health Reporting*  
- **After:** *Hierarchical Selective Anchoring for LMIC Disease Surveillance: Submission Accountability and Explicit Privacy Semantics on a Blockchain Coordination Layer*  
- Reflects accountability + honest privacy semantics rather than vague “privacy-layered.”

### 12. Deliverable for implementers

- Added **`PROMPT_2_IMPLEMENTATION_SPEC.md`** with 20 required sections (title through definition of done) and no narrative fluff beyond implementation needs.

---

## What was intentionally not changed

- Rejection of fabricating results remains.  
- `original/` immutability.  
- Analytical baselines list (DB, transparency log, Fabric) retained.  
- PoA as local evaluation substrate retained without multi-consensus experiment mandate.  
- SDRM may still be discussed as methodology framing but is not a substitute for artefacts.

---

## Consistency checks performed

| Check | Result |
|---|---|
| Every RQ answerable by planned eval | Yes (RQ1 authz+footprint; RQ2 A–E+tests; RQ3 gaps/late) |
| Unsupported historical results carried as facts | No |
| Unimplemented features as implemented | No |
| Ledger auth vs IPFS access separated | Yes (A vs B/C) |
| On-chain vs cryptographic revocation separated | Yes (A vs E) |
| Completeness = submission accountability | Yes |
| Scope small enough for reproducible Prompt 2 | Yes (core chain + optional data layer) |
| `original/` touched | No |

---

## Residual risks for later prompts

1. Scope hierarchy encoding must be chosen carefully (prefix vs parent map) to avoid ambiguous cover tests.  
2. HMAC helpers in JS must match on-chain stored bytes exactly—golden vectors mandatory.  
3. Optional IPFS may tempt overclaiming privacy; A–E docs must ship even if IPFS skipped.  
4. Literature rebuild still required before final paper (per `LITERATURE_GAP_PLAN.md`); not part of Prompt 1.5 implementation freeze.  
5. CLAIM_EVIDENCE_MATRIX still reflects pre-implementation state; update after Prompt 2 artefacts exist.

---

## Files touched this revision

| File | Action |
|---|---|
| `RESEARCH_REWRITE/NEW_RESEARCH_BLUEPRINT.md` | **Replaced** with Prompt 1.5 revision |
| `RESEARCH_REWRITE/PROMPT_2_IMPLEMENTATION_SPEC.md` | **Created** |
| `RESEARCH_REWRITE/BLUEPRINT_REVISION_LOG.md` | **Created** |

---

## Readiness

**Ready for Prompt 2 implementation** under `PROMPT_2_IMPLEMENTATION_SPEC.md`, provided implementers do not expand into non-goals and do not import historical numbers.
