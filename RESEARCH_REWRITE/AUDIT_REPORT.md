# Forensic Audit Report — Selective Anchoring Surveillance Architecture

**Prepared:** 2026-08-12  
**Repository:** SisayNegashMengistu/Selective-Anchoring-Research  
**Auditor role:** Senior research architect / blockchain-security researcher / harsh peer reviewer  
**Status:** Pre-rewrite audit — no manuscript changes made  

---

## 1. Executive Assessment

The manuscript describes a sophisticated, multi-contribution blockchain-based disease surveillance architecture. The **written architecture is coherent and the problem framing is legitimate**. However, the manuscript claims a far larger body of implemented, tested, and experimentally validated work than actually exists in the repository. The gap between claimed and actual evidence is substantial enough that several core claims would be rejected or significantly downgraded by any competent reviewer.

The **single most serious problem** is that no executable implementation exists anywhere in the repository. The entire `prototype/` directory is empty (contains only `.gitkeep`). The Solidity contract code, React/Next.js application, Mocha test suite, deployment scripts, and all other artefacts described in the implementation and evaluation sections exist **only as LaTeX listings and descriptive prose**. All numerical results — gas costs, block times, IPFS retrieval latencies, SUMI scores, and the "24/24 tests pass" claim — are stated without any reproducible experimental artefact.

The paper is, at present, a detailed **design proposal with fabricated-looking quantitative support**, not a validated system. It can be honestly defended as a well-reasoned architecture paper; it cannot be defended as a prototype-and-evaluation paper without substantial new work.

---

## 2. What the Original Project Actually Contains

### 2.1 Files Present

| Component | Location | Type | Actually Executable? | Evidence of Execution? | Role in Manuscript |
|---|---|---|---|---|---|
| main.tex | original/main.tex | LaTeX entry point | No | No | Document root |
| abstract.tex | original/sections/abstract.tex | LaTeX section | No | No | Abstract |
| introduction.tex | original/sections/introduction.tex | LaTeX section | No | No | Sec. 1 |
| related_work.tex | original/sections/related_work.tex | LaTeX section | No | No | Sec. 2 / Background |
| methodology.tex | original/sections/methodology.tex | LaTeX section | No | No | Sec. 3–4 |
| results.tex | original/sections/results.tex | LaTeX section | No | No | Sec. 5–6 (implementation + evaluation) |
| discussion.tex | original/sections/discussion.tex | LaTeX section | No | No | Sec. 7 |
| conclusion.tex | original/sections/conclusion.tex | LaTeX section | No | No | Sec. 8 |
| references.bib | original/references.bib | BibTeX | No | No | Bibliography |
| WileyNJDv5.cls | original/WileyNJDv5.cls | LaTeX class | No | No | Formatting |
| Wiley style files (*.bst, *.sty) | original/*.bst, *.sty | Style support | No | No | Formatting |
| Fonts/ | original/Fonts/ | TTF/OTF font files | No | No | Typesetting only |
| empty.eps / empty.pdf | original/ | Placeholder figure | No | No | Placeholder |
| RESEARCH_REWRITE/ | RESEARCH_REWRITE/ | Empty workspace | — | — | Reserved for output |
| prototype/ | prototype/ | Empty (.gitkeep only) | — | — | No implementation |
| evaluation/ | evaluation/ | Empty (.gitkeep only) | — | — | No data |
| REVISED_PAPER/ | REVISED_PAPER/ | Empty (.gitkeep only) | — | — | Reserved |

### 2.2 What Is Absent

- **No Solidity source files** (.sol) outside of LaTeX `verbatim` listings
- **No Hardhat, Foundry, or Truffle configuration** (no hardhat.config.js, truffle-config.js, foundry.toml)
- **No package.json** of any kind
- **No Mocha or other test files** (.test.js, .spec.js)
- **No deployment scripts** outside a LaTeX `verbatim` listing
- **No IPFS integration code**
- **No React/Next.js source code**
- **No Python, JavaScript, or TypeScript source files**
- **No datasets** (synthetic or real)
- **No benchmark scripts or output files**
- **No generated/recorded outputs** (transaction receipts, gas measurements, IPFS CIDs, SUMI raw data)
- **No CLAUDE_REVIEW.md** (referenced in the task description but not present in the repository)
- **No figures** (all figure positions contain LaTeX `\fbox{...Figure placeholder...}` constructs)

---

## 3. What Is Genuinely Implemented

**Nothing is implemented.** The prototype directory is empty. All code appears exclusively as LaTeX verbatim/lstlisting/figure environments within the manuscript sections. These listings are **design illustrations**, not executable artefacts.

The manuscript itself is internally honest about several unimplemented components (edge/fog Layer-2 rollup, ZKP circuits are explicitly called "architecturally specified" or "future work"). However, it claims that the core smart-contract layer **was** implemented and tested (24/24 test cases, SUMI evaluation with 26 participants, gas measurements, block timing on Rinkeby). No artefacts supporting these claims exist.

---

## 4. What Is Only Manuscript-Level Design

All five claimed contributions are manuscript-level design:

1. **Selective anchoring architecture** — described and illustrated with Mermaid diagram source (commented out in LaTeX), LaTeX Solidity listings. Not executable.
2. **Layered privacy model (ZKP + dynamic ACL + encrypted off-chain)** — partially described; ZKP explicitly deferred to future work in the limitations section despite appearing in the abstract as a core contribution.
3. **Edge/fog preprocessing** — described as "architecturally specified"; the implementation section acknowledges it "executes within the Next.js server" in the current prototype, with production deployment deferred.
4. **Comparative consensus framework** — a descriptive table comparing PoA, DPoS, PBFT, and Raft. No empirical comparison performed or reported.
5. **FHIR-aware interoperability workflows** — one paragraph description; no FHIR library integration, no HL7 message handling, no implementation evidence.

---

## 5. Major Claim/Evidence Failures

| # | Claim | Evidence Status | Severity |
|---|---|---|---|
| 1 | "24 test cases, 100% pass rate" for AccessControl | No test files exist anywhere. Claim is unverifiable. | **P0 — Critical** |
| 2 | "50 IPFS retrievals, 100% integrity, mean latency 340 ms" | No scripts, no data files, no output logs. Fabricated-looking. | **P0 — Critical** |
| 3 | "Block generation 12–14 s on Rinkeby" | Rinkeby was deprecated by Ethereum developers in Q4 2022. No transaction records exist. | **P0 — Critical** |
| 4 | Gas cost measurements (68,400–94,200 gas, $0.15–$0.42) | No deployment receipts, no gas reporter output. No contracts deployed. | **P0 — Critical** |
| 5 | "Storage reduction 12:1 to 217:1" | Plausible analytically, but presented as experimental result. No scripts. | **P1 — High** |
| 6 | SUMI evaluation: n=26, scores 60.46–68.96 | No raw data, no questionnaire responses, no ethics approval documentation. | **P0 — Critical** |
| 7 | "Zero-knowledge verification" as core contribution | Explicitly deferred to future work in limitations. Contradicts abstract. | **P0 — Critical** |
| 8 | "FHIR-aware interoperability workflows" as contribution | One paragraph description; no implementation. | **P1 — High** |
| 9 | "Comparative consensus framework" as contribution | Descriptive prose table. No workload measurements. | **P1 — High** |
| 10 | "Edge/fog preprocessing" as contribution | Application-layer Next.js middleware described. No edge node deployment. | **P1 — High** |
| 11 | All figures are "Figure placeholder" | 15+ figure positions are unfilled `\fbox` placeholders. | **P0 — Critical** |
| 12 | SHA-256 of plaintext stored on-chain as integrity anchor | This leaks hash-based re-identification risk for low-entropy health records. Claimed as security feature. | **P0 — Critical** |
| 13 | "Revoking access" revokes IPFS ciphertext access | Smart contract `revokeAccess()` only flips `granted=false`. IPFS content remains retrievable by anyone with the CID. No key rotation or re-encryption implemented. | **P0 — Critical** |
| 14 | Blockchain access control state "gates" IPFS retrieval | This conflates on-chain state with actual data inaccessibility. IPFS is public. | **P0 — Critical** |
| 15 | "Functional prototype validates the smart-contract coordination layer" | No executable prototype exists. | **P0 — Critical** |

---

## 6. Security Issues

### 6.1 Revocation Semantic Failure (P0)
`revokeAccess()` sets `permissions[reportId][grantee].granted = false`. This does **not** prevent the grantee from accessing the IPFS-stored ciphertext if they already possess or cached the CID. IPFS is a public content-addressed network. Once a CID is known to a party, the content remains accessible unless:
- The content is not pinned anywhere the party can reach (unreliable)
- The encryption key is rotated and the old key withdrawn (not implemented)
- Proxy re-encryption is used to re-encrypt under a new key (not implemented)

The manuscript does not mention key management, key distribution infrastructure, or key rotation anywhere. The "layered privacy model" claim depends on a key management system that is entirely absent from the design.

### 6.2 Plaintext Hash Leakage (P0)
`contentHash` stores `SHA-256(plaintext)` on-chain. For health surveillance records, many fields are low-entropy (disease codes from a set of 20, Boolean flags, small integer counts, standard geographic identifiers). SHA-256 is not a keyed function. An attacker with access to the blockchain and a hypothesis about the record content can verify it by computing the hash. This is a re-identification attack vector the manuscript does not address.

### 6.3 No Access Control Enforcement on callers (P0)
The `grantAccess()` and `revokeAccess()` functions in `AccessControl` have **no `require` statement limiting who can call them**. Any address can grant access to any report to any address. Any address can revoke anyone else's access. This is a critical access control vulnerability in the manuscript code listings themselves.

### 6.4 Metadata Exposure on Public Ledger (P1)
The manuscript claims "no epidemiological content appears on the public ledger." However, reporter addresses, timestamps, tier levels, CID pointers, and completeness counts are on-chain. Traffic analysis over time reveals reporting patterns, facility identities, submission timing, and throughput — all sensitive for disease surveillance.

### 6.5 Rinkeby Deprecation (P1)
The evaluation claims results measured "on the Rinkeby proof-of-authority test network." Rinkeby was officially deprecated by the Ethereum Foundation and shut down in September 2023. Results claimed on a deprecated, unavailable network cannot be reproduced or verified.

### 6.6 CompletenessVerifier from Encrypted Data (P0)
The manuscript states that `CompletenessVerifier` verifies completeness of reports. The evaluation section and discussion claim it verifies field-level completeness. However, payloads are encrypted before reaching the contract. The actual listing shows the verifier only counts `anchorReport()` calls per period — it cannot inspect encrypted field content. The claim that "incomplete submissions are rejected at the smart-contract level" is not supported by the contract logic shown.

---

## 7. Privacy Issues

1. **Key management absent**: The entire privacy model depends on encryption keys being distributed only to authorized parties. No key distribution mechanism, key escrow, or key rotation protocol is described or implemented.
2. **IPFS confidentiality assumption**: IPFS content is public by default. The manuscript's privacy claims assume IPFS acts as private storage, which it does not.
3. **Revocation without re-encryption**: Revoking a permission does not revoke knowledge of the encryption key already distributed.
4. **ZKP claimed, not implemented**: The abstract claims "zero-knowledge verification" as a core privacy contribution. The limitations section acknowledges ZKP is not implemented. This is a direct contradiction between the abstract and the body.
5. **SHA-256 of plaintext on-chain**: See Security Issues §6.2.

---

## 8. Methodology Issues

1. **SDRM applied without evidence**: The five SDRM stages are described but there is no evidence of the iterative design cycles, stakeholder feedback, or artefact revision that SDRM requires.
2. **n=10 blockchain suitability survey** reported in one place, while the paper also reports n=26 SUMI participants recruited from the same institute. These are different populations; the relationship is not made explicit.
3. **Rinkeby test network**: Performance evaluation on a deprecated public test network is not reproducible, not representative of production conditions, and not controllable.
4. **No baseline comparison**: The manuscript claims storage reduction "relative to direct on-chain storage." There is no baseline measurement of the alternative system; the comparison is purely analytical (payload size vs. calldata size).
5. **SUMI evaluation**: 26 participants from a single institute evaluating a prototype they saw for 15–20 minutes. No learning effect control, no between-groups design, no pre-existing benchmark comparison beyond the normative SUMI population mean.
6. **Ethics treatment**: The ethics section states participants "provided voluntary agreement to participate." This is not formal informed consent documentation. No institutional ethics committee approval is mentioned. For a study involving human participants at a named institution (Amhara Public Health Institute), this is inadequate.

---

## 9. Evaluation Issues

1. **No reproducible artefacts**: Every numerical result lacks a corresponding script, data file, or output log.
2. **All figures are placeholders**: The manuscript describes 15+ figures but provides only `\fbox{Figure placeholder}` constructs.
3. **Gas price basis unclear**: "$0.15–$0.42 per anchoring event" uses an unspecified ETH/USD rate and an unspecified gas price (gwei). The comparison ("$12–$180 for direct storage") uses "$5.12 per KB of calldata" — this figure is not sourced or dated.
4. **IPFS latency**: "340 ms (SD=89 ms)" from a local daemon. This is essentially a localhost test. Production IPFS over variable-bandwidth LMIC networks would be vastly different.
5. **Block time outlier suppressed**: An outlier of 114 seconds is reported for one block, then excluded from the discussion of "stable 12–14 s" performance. The exclusion is methodologically questionable and is not statistically justified.
6. **24 test cases**: The test methodology section describes a "permission matrix of 24 test cases" but provides no test code, no output, and no specification of the 24 scenarios.

---

## 10. Literature Weaknesses

1. **Placeholder references throughout**: The bibliography contains entries with `journal={Journal placeholder}` and `author={others}`. These are not real bibliographic records.
2. **Narrow coverage**: Only 14 distinct references, all pre-2022. No references to post-2022 work on blockchain healthcare, LMIC digital health, ZKP systems, or CP-ABE.
3. **Missing key prior work**: No citation of Hyperledger Fabric, MedRec, BurstIQ, Aultman/Linn or other blockchain-health systems directly comparable to the proposed architecture.
4. **FHIRChain cited**: The Zhang et al. 2018 FHIRChain citation is likely verifiable, but the bibliographic entry uses `author={Zhang, Peng and others}` — incomplete author metadata.
5. **Foundational blockchain reference is NIST IR 8202**: Cited as "NIST Interagency/Internal Report" — the actual document is NISTIR 8202, and the metadata is incomplete.
6. **`finlayson2019adversarial`**: This appears to be Finlayson et al., Science 2019, "Adversarial attacks on medical machine learning" — real paper, but it is about ML adversarial attacks, not blockchain metadata privacy. Its use here is a citation stretch.
7. **`bohme2015building`**: "Building a Better Bitcoin" — this does not appear to be the correct title for a Bitcoin/Ethereum security paper. Likely a fabricated or misremembered title.

---

## 11. Logical-Flow Weaknesses

1. **Abstract vs. body contradiction on ZKP**: Abstract claims ZKP as a core contribution; limitations section says it is not yet implemented. This is the most visible reviewer-catching contradiction.
2. **Contribution list inflation**: Six contributions are listed in the introduction. Contributions 4 (comparative consensus) and 5 (FHIR workflows) are each one descriptive paragraph with no new analytical or experimental support.
3. **"Completeness from encrypted payloads"**: The discussion claims "CompletenessVerifier ensures that only reports meeting mandatory field requirements enter the provenance chain." The actual contract listing counts anchor events per period, not field-level completeness. The claim is not supported by the code.
4. **"On-chain permission state gates IPFS retrieval"**: This architectural claim is stated as if enforced, but IPFS retrieval does not query the blockchain. The gate exists only if the client application voluntarily consults the contract. No enforcement mechanism is described.
5. **"First" and "novel" language used without basis**: The related work table marks the current work "Yes" on all five dimensions and all prior work "No" on at least some — but the table ignores Hyperledger Fabric private data collections, which address several of the same dimensions.
6. **Usability of what, exactly?**: SUMI evaluates the usability of a software system that the participants interacted with for 15–20 minutes. The implementation section acknowledges that the prototype is incomplete. What exactly did participants interact with?

---

## 12. Novelty Assessment

**Genuine novelty** (if properly implemented and evaluated):
- The *selective anchoring* framing for LMIC disease surveillance, where the blockchain is explicitly repositioned as a coordination layer rather than a data repository, applied to the specific Ethiopian/Sub-Saharan African PHEM hierarchy — this is a defensible and practically motivated contribution.
- The hierarchical role-differentiated node participation model, mapping blockchain responsibilities to administrative tiers, is practically motivated and underrepresented in the literature.

**Integration rather than novelty**:
- IPFS + blockchain hash anchoring: established pattern (Swarm, Storj, many prior papers since 2017)
- AES-256 encryption of off-chain data: standard practice
- Dynamic ACL smart contracts: described in Griggs 2018, Dubovitskaya 2017, and many others
- FHIR + blockchain: FHIRChain 2018 already covers this
- PoA vs. PBFT vs. DPoS comparison: multiple survey papers exist

**Overstated as novel**:
- ZKP integration (not implemented)
- FHIR interoperability (not implemented)
- Edge/fog for blockchain (not implemented)
- Comparative consensus evaluation (descriptive, not experimental)

---

## 13. Highest-Risk Reviewer Objections

| Rank | Objection | Risk |
|---|---|---|
| 1 | "The claimed 24/24 test pass result: where is the test code? Where is the test output?" | Rejection |
| 2 | "Rinkeby was deprecated in 2023. Results measured on this network cannot be reproduced." | Rejection |
| 3 | "Abstract claims ZKP as a core contribution; limitations say it is not implemented. This is a contradiction." | Rejection |
| 4 | "All figures are placeholders. The manuscript is incomplete." | Rejection |
| 5 | "IPFS revocation: revoking a smart contract permission does not prevent access to IPFS content by a party who already has the CID. The privacy model is broken." | Rejection |
| 6 | "grantAccess() and revokeAccess() have no access control themselves. Any account can grant or revoke." | Major revision / rejection |
| 7 | "SHA-256 of plaintext on-chain enables hash-based re-identification of low-entropy health records." | Major revision |
| 8 | "SUMI n=26 at a single institute, 15-minute interaction. No ethics committee approval documented." | Major revision |
| 9 | "The bibliography contains entries with 'Journal placeholder' and 'others' as authors." | Rejection |
| 10 | "Why is Hyperledger Fabric not compared? Private data collections address the same hybrid storage problem." | Major revision |

---

## 14. What Can Be Repaired Without New Experiments

1. Fix all placeholder bibliography entries (requires literature verification)
2. Correct the abstract to remove ZKP as a current contribution (move to future work)
3. Correct the discussion claim that CompletenessVerifier verifies field-level completeness
4. Correct the access control narrative to distinguish on-chain state from IPFS enforceability
5. Add `onlyOwner`/`onlyGranter` modifiers to grantAccess/revokeAccess
6. Acknowledge SHA-256 re-identification risk and propose HMAC-keyed hash alternative
7. Replace Rinkeby references with a reproducible test network (Hardhat/Anvil local chain or Sepolia)
8. Remove the "first" and "novel" overstatements in the related work table
9. Properly document ethics procedures or clarify that minimal-risk exemption applies
10. Reduce contribution list from six to three tightly-supported contributions

---

## 15. What Requires New Implementation

1. **Executable Solidity contracts** with access control enforcement on all sensitive functions
2. **Key management layer**: AES key distribution, rotation, and revocation mechanism
3. **Mocha/Hardhat test suite** with the 24 described test scenarios (plus additional coverage)
4. **Deployment scripts** using Hardhat or Foundry against a local chain
5. **Storage comparison benchmark**: script measuring calldata size vs. payload size (analytically simple)
6. **Gas measurement**: Hardhat gas reporter on local chain (straightforward)
7. **IPFS integration**: actual ipfs-http-client code with encryption/decryption

---

## 16. What Requires New Experiments

1. **Block latency measurement** on a reproducible local PoA chain (replaces Rinkeby data)
2. **IPFS retrieval latency** over distributed gateways (replaces local-daemon result)
3. **SUMI re-evaluation** with the actual implemented prototype (if human-subjects work is to be retained)
4. **Gas cost measurements** from actual Hardhat gas reporter output
5. **Completeness verification functional test** demonstrating the contract behavior accurately

---

## 17. What Should Be Removed

1. **ZKP as a core contribution** — move to explicitly-labeled future work
2. **FHIR interoperability as a contribution** — downgrade to design aspiration or future work
3. **Comparative consensus "evaluation"** — either conduct actual measurements or reframe as an architectural design rationale discussion
4. **All "Figure placeholder" constructs** — replace with real figures or remove figure references
5. **"24/24 test cases pass" claim** — remove until tests are actually executed
6. **Rinkeby performance data** — remove or replace with local chain data
7. **"Functional prototype validates..." as an established fact** — qualify as "design of a prototype" until implementation exists
8. **SUMI scores as reported** — do not claim statistical significance without a properly conducted evaluation
9. **"$0.15–$0.42 per transaction"** — do not present as measured result until gas reporter output exists

---

## 18. Recommended Revised Research Direction

The paper's strongest defensible contribution is:

> **A selective-anchoring coordination architecture for hierarchical LMIC disease surveillance**, combining encrypted IPFS off-chain storage with a smart-contract permission layer, designed around the Ethiopian PHEM administrative tier structure.

This contribution can be substantiated with:
- An implemented and tested Solidity contract suite (3–4 weeks of engineering work)
- A reproducible Hardhat-based test suite for correctness and gas cost
- An honest threat model that distinguishes on-chain state from IPFS access control
- A key management design (can be analytical if implementation is deferred)
- A reduced but honest evaluation (gas costs, block time, IPFS latency — all on local chain)

The ZKP, FHIR, edge/fog, and consensus comparison contributions should either be properly implemented and evaluated, or moved entirely to future work. The SUMI evaluation can be retained if re-conducted with the implemented prototype under ethics oversight.

---

## 19. Recommended Manuscript Structure

```
1. Introduction
   - Problem: LMIC DEWS architectural failures
   - Gap: no hybrid selective-anchoring architecture for hierarchical African surveillance
   - Contributions: (1) selective-anchoring design + threat model, (2) implemented+tested Solidity contracts, (3) gas/latency characterization
2. Background and Related Work
   - Blockchain healthcare (with verified references)
   - IPFS-blockchain hybrid designs
   - LMIC surveillance systems
   - Architectural gap table (with Hyperledger Fabric added)
3. Architecture
   - Five-tier design
   - Privacy model (honest: encryption + ACL + key management design)
   - Threat model
   - Access control semantics (honest about IPFS enforcement gap)
4. Implementation
   - Solidity contracts (with actual code, not just design listings)
   - Key management approach
   - Test suite description
5. Evaluation
   - Correctness: test suite results (from actual test runner)
   - Gas/storage: Hardhat gas reporter results
   - Latency: local PoA chain measurements
   - Security analysis: threat-to-mitigation matrix
6. Discussion
   - Limitations (honest)
   - Comparison to prior work
7. Conclusion and Future Work
   - ZKP, FHIR, edge/fog, consensus as future work
```

---

## 20. Priority Actions

### P0 — Must resolve before submission

1. Implement the Solidity contracts with proper access control modifiers
2. Build and run the Mocha/Hardhat test suite; record output
3. Record gas measurements with Hardhat gas reporter
4. Measure block confirmation time on local Hardhat/Ganache PoA chain
5. Remove ZKP from abstract's claimed contributions
6. Remove all "Figure placeholder" constructs; create real figures
7. Fix all placeholder bibliography entries
8. Correct the revocation semantic: acknowledge IPFS limitations and specify key management
9. Add access control modifiers to grantAccess/revokeAccess
10. Fix the CompletenessVerifier claim (counts anchor events, not field completeness)

### P1 — Strongly recommended

11. Replace Rinkeby with a reproducible test environment
12. Re-conduct SUMI evaluation with the actual implemented prototype under ethics oversight
13. Add Hyperledger Fabric private data collections to related work comparison
14. Document key distribution and rotation design
15. Add HMAC-keyed integrity commitment to replace plaintext SHA-256 on-chain

### P2 — Useful improvement

16. Conduct actual PoA vs. PBFT comparative measurements (even modest scale)
17. Evaluate retrieval latency over a distributed IPFS gateway
18. Expand SUMI sample to multiple sites
19. Add formal security definition for the access control model

### P3 — Future work (do not present as current contributions)

20. ZKP circuit for privacy-preserving completeness verification
21. FHIR SMART on FHIR integration
22. Layer-2 rollup batch settlement
23. Dedicated edge node deployment
24. Longitudinal production evaluation

---

*This audit was prepared based on a complete reading of all manuscript files in original/. The original/ directory was not modified. No implementation artefacts were found outside of LaTeX verbatim listings.*
