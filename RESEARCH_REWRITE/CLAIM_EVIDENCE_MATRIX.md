# Claim/Evidence Forensic Matrix

**Manuscript:** Selective Anchoring Architecture for LMIC Disease Surveillance  
**Audit date:** 2026-08-12  
**Source files:** original/sections/*.tex  

**Classification key:**  
- **SUPPORTED** — claim backed by verifiable, reproducible, or independently computable evidence  
- **PARTIALLY SUPPORTED** — some evidence exists but is incomplete, unverifiable, or underqualified  
- **UNSUPPORTED** — no evidence found in the repository or external sources  
- **CONTRADICTED** — claim is directly contradicted by other text or by the actual artefacts  
- **DESIGN/FUTURE WORK ONLY** — claim is a design aspiration; not yet implemented or evaluated  

---

## Section A — Architecture and Selective Anchoring Claims

| ID | Claim Summary | Location | Category | Implementation Evidence | Test Evidence | Experimental Evidence | Mathematical/Analytical Evidence | Citation Evidence | Classification | Problem | Severity | Required Action |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| A1 | "Selective anchoring: encrypted payloads reside off-chain via IPFS; blockchain retains only hashes, ACL, provenance, alerts, audit events" | Abstract, Intro, Methodology | Architecture | None (LaTeX listing only) | None | None | Analytically sound design | Partial (FHIRChain) | DESIGN/FUTURE WORK ONLY (if prototype absent) | No executable implementation | P0 | Implement contracts and test |
| A2 | "Hierarchical event processing with district-level batching aligned to public-health administrative tiers" | Abstract, Sec. 4 | Architecture | None | None | None | Described logically | None specific | DESIGN/FUTURE WORK ONLY | No implementation | P1 | Implement batch aggregator or reframe as design |
| A3 | "Five-tier hybrid architecture (community → edge/fog → district → blockchain → IPFS)" | Sec. 4 | Architecture | None | None | None | Coherent mapping to Ethiopia PHEM tiers | None | DESIGN/FUTURE WORK ONLY | No implementation of Tiers 2, 3 | P1 | Implement or clearly label as design |
| A4 | "Raw surveillance payloads never stored on-chain" | Sec. 4, 5 | Architecture | Listings show CID + hash only, consistent | Listings only | None | Consistent with design | None | PARTIALLY SUPPORTED | No running code; design is consistent with this claim | P1 | Implement to verify |
| A5 | "Factory pattern ensures each coordinator retains unmodified copy; prevents post-deployment tampering" | Sec. 5 | Architecture | Listing shows factory deploying coordinators | None | None | Plausible | None | PARTIALLY SUPPORTED | Factory pattern does not by itself prevent upgrades | P2 | Clarify proxy/upgrade risk |
| A6 | "Batch anchoring reduces per-report transaction costs" | Sec. 5 | Architecture | Listed in ReportFactory listing | None | None | Analytically true | None | PARTIALLY SUPPORTED | No gas measurement for batch vs. individual | P1 | Measure with gas reporter |
| A7 | "Content-addressed IPFS storage: any modification invalidates the CID" | Sec. 6, 7 | Architecture/Security | None (design claim) | None | None | Correct property of content-addressing | None | SUPPORTED (analytically) | This is a correct analytical property of IPFS | P2 | Can remain as analytical claim if clearly labeled |

---

## Section B — Privacy Claims

| ID | Claim Summary | Location | Category | Implementation Evidence | Test Evidence | Experimental Evidence | Mathematical/Analytical Evidence | Citation Evidence | Classification | Problem | Severity | Required Action |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| B1 | "Layered privacy model combining zero-knowledge verification, dynamic grant/revoke/expire permissions, and encrypted off-chain records" | Abstract | Privacy | None (ZKP explicitly deferred in limitations) | None | None | ZKP architecture described but not realized | None | CONTRADICTED | Abstract claims ZKP as current contribution; limitations say it is not implemented | P0 | Remove ZKP from abstract claimed contributions |
| B2 | "Zero-knowledge verification for privacy-preserving validation" | Intro (Contribution 2), Sec. 4 | Privacy | None | None | None | zk-SNARK circuit described in commented Mermaid only | None | DESIGN/FUTURE WORK ONLY | ZKP not implemented | P0 | Move to future work |
| B3 | "AES-256-GCM client-side encryption before any network transmission" | Sec. 5 | Privacy | Web Crypto API reference in tech stack description | None | None | Standard practice | None | PARTIALLY SUPPORTED | No code, no key management design | P1 | Implement and document key distribution |
| B4 | "revokeAccess() removes previously granted permission" | Sec. 5, 6 | Privacy/Access Control | Listing sets `granted=false` | None | None | Sets flag only; does not revoke IPFS access | None | CONTRADICTED | Revoking on-chain state does not prevent IPFS retrieval by a party with the CID and key | P0 | Redesign revocation model; add key rotation |
| B5 | "Authorized parties verify integrity, provenance, and access status through on-chain proofs and permission logic; retrieve encrypted payloads from IPFS only when access is granted" | Sec. 4 | Privacy | None | None | None | Depends on client voluntarily checking contract | None | PARTIALLY SUPPORTED | Enforcement is client-side voluntary; not contract-enforced | P0 | Clarify or enforce |
| B6 | "Neither the blockchain nor the IPFS layer alone exposes identifiable health data" | Sec. 5 | Privacy | None | None | None | Partially true: IPFS has ciphertext; blockchain has hashes | None | PARTIALLY SUPPORTED | Blockchain has SHA-256 of plaintext (re-identification risk) and reporter addresses | P0 | Address SHA-256 leakage; add HMAC |
| B7 | "No epidemiological content — patient identifiers, disease codes, geographic coordinates — appears on the public ledger" | Sec. 7 | Privacy | None | None | None | Consistent with design if encryption is enforced | None | PARTIALLY SUPPORTED | SHA-256(plaintext) hash is a form of epidemiological content proxy | P0 | Fix hash design |
| B8 | "Dynamic grant/revoke/expire permissions" | Abstract, Sec. 4, 5 | Privacy/Access Control | Listing shows grant/revoke/setExpiry | None | None | `checkAccess()` checks expiry correctly | None | PARTIALLY SUPPORTED | No caller authentication on grantAccess/revokeAccess (any address can call) | P0 | Add modifier to restrict callers |
| B9 | "Selective disclosure ensures specimen-level data is accessible only to authorized laboratory tiers" | Sec. 4 | Privacy | None | None | None | Depends on key distribution (absent) | None | DESIGN/FUTURE WORK ONLY | No key management; IPFS content is public | P0 | Design key management layer |
| B10 | "Automatic expiry preventing persistent overbroad access" | Sec. 4 | Privacy | checkAccess() listing checks timestamp | None | None | Correct for on-chain state; does not affect IPFS access | None | PARTIALLY SUPPORTED | Expiry stops contract returning `true`; does not revoke ciphertext/key access | P1 | Clarify scope of expiry |

---

## Section C — Access Control Claims

| ID | Claim Summary | Location | Category | Implementation Evidence | Test Evidence | Experimental Evidence | Mathematical/Analytical Evidence | Citation Evidence | Classification | Problem | Severity | Required Action |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| C1 | "24 test cases, 100% pass rate for AccessControl contract" | Sec. 6 | Access Control / Testing | No test files exist | None | None | N/A | None | UNSUPPORTED | No test suite exists anywhere in repository | P0 | Implement and run tests |
| C2 | "Authorized transactions succeeded, unauthorized transactions reverted with appropriate error codes" | Sec. 6 | Access Control / Testing | No modifier in listing prevents unauthorized calls | None | None | N/A | None | CONTRADICTED | grantAccess/revokeAccess have no caller restriction; any address can call | P0 | Add access modifiers before re-testing |
| C3 | "24/24 permission scenarios: authorized access at each hierarchical level, cross-boundary attempts, grant/revoke sequences, expired permission rejection" | Sec. 6 | Access Control / Testing | No test file | None | None | N/A | None | UNSUPPORTED | No evidence of test scenarios being defined or run | P0 | Build test suite |
| C4 | "On-chain permission state gates off-chain data retrieval" | Sec. 6, 7 | Access Control | None | None | None | Not technically accurate as stated | None | CONTRADICTED | IPFS retrieval does not consult the blockchain contract | P0 | Correct the claim; specify client-side enforcement |
| C5 | "Permissions scoped by tier level, reporter–supervisor relationship, and data category" | Sec. 5 | Access Control | Listing shows `tierScope` in Permission struct | None | None | Partially designed | None | PARTIALLY SUPPORTED | tierScope stored but not enforced in grant/revoke logic | P1 | Enforce tierScope in checkAccess |
| C6 | "Reporter registration" (reporters registered before submitting) | Sec. 4 | Access Control | Not in any listing | None | None | Not described | None | UNSUPPORTED | No reporter registration mechanism in any listing | P1 | Add reporter registry or clarify |

---

## Section D — Completeness Verification Claims

| ID | Claim Summary | Location | Category | Implementation Evidence | Test Evidence | Experimental Evidence | Mathematical/Analytical Evidence | Citation Evidence | Classification | Problem | Severity | Required Action |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| D1 | "CompletenessVerifier ensures only reports meeting mandatory field requirements enter the provenance chain" | Sec. 7 | Completeness | None — listing only counts anchor events | None | None | Not logically possible given encrypted payloads | None | CONTRADICTED | Encrypted payloads cannot be field-inspected by contract; listing counts anchor calls only | P0 | Correct claim: verifier counts submissions, not fields |
| D2 | "Completeness ratio (received/expected) queryable on-chain without accessing payload data" | Sec. 5 | Completeness | Listing supports this claim | None | None | Correct for event-counting design | None | SUPPORTED (design) | Defensible analytical claim for event-counting verifier | P2 | Retain with accurate description |
| D3 | "Gap alert emitted when ratio falls below configurable threshold at period close" | Sec. 5 | Completeness | Listing includes GapAlert event | None | None | Correct | None | PARTIALLY SUPPORTED | Threshold parameter in listing not shown in full; closing logic incomplete | P1 | Complete listing and test |
| D4 | "Addresses persistent challenge of incomplete weekly reports in Ethiopian PHEM" | Sec. 7 | Completeness | None | None | None | Reasonable motivation | DHIS2/PHEM context | PARTIALLY SUPPORTED | Motivation is valid; mechanism only counts anchoring events | P1 | Clarify scope |

---

## Section E — Security Claims

| ID | Claim Summary | Location | Category | Implementation Evidence | Test Evidence | Experimental Evidence | Mathematical/Analytical Evidence | Citation Evidence | Classification | Problem | Severity | Required Action |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| E1 | "Zero unauthorized access in 24 security scenarios" | Sec. 6 | Security | No test files | None | None | N/A | None | UNSUPPORTED | Dependent on C1 above | P0 | Implement tests |
| E2 | "Defense-in-depth: separation of confidentiality (IPFS) from integrity/provenance (blockchain)" | Sec. 7 | Security | Design only | None | None | Conceptually sound if key management exists | None | DESIGN/FUTURE WORK ONLY | Security separation depends on absent key management | P1 | Design key management |
| E3 | "Adversary who compromises IPFS layer obtains only ciphertext" | Sec. 7 | Security | Design claim | None | None | True only if keys not compromised | None | PARTIALLY SUPPORTED | True analytically under key assumption; key management absent | P0 | Add key management |
| E4 | "Adversary who reads blockchain obtains only hashes and permission metadata" | Sec. 7 | Security | Design claim | None | None | True by design if SHA-256 leakage addressed | None | PARTIALLY SUPPORTED | SHA-256(plaintext) on-chain enables re-identification | P0 | Replace with HMAC |
| E5 | "Man-in-the-middle resistance through blockchain's native cryptographic properties" | Sec. 8 | Security | Design claim | None | None | Partially correct for on-chain transactions | None | PARTIALLY SUPPORTED | Off-chain IPFS retrieval channel is not protected by blockchain | P1 | Clarify scope |
| E6 | "Expanded threat model addresses validator collusion, metadata leakage, overbroad tier exposure, equity-aware governance" | Abstract | Security | Threat table in results section | None | None | Analytical threat model only | None | PARTIALLY SUPPORTED | Threat model is descriptive; no penetration testing | P1 | Retain as analytical; label clearly |
| E7 | "Immutable on-chain anchors prevent retroactive tampering" | Throughout | Security | Property of blockchain | None | None | Correct | None | SUPPORTED (analytically) | Standard blockchain property; defensible | P3 | Retain |

---

## Section F — Performance/Scalability Claims

| ID | Claim Summary | Location | Category | Implementation Evidence | Test Evidence | Experimental Evidence | Mathematical/Analytical Evidence | Citation Evidence | Classification | Problem | Severity | Required Action |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| F1 | "Block generation 12–14 s (stable range)" | Abstract, Sec. 6, 7, 8 | Performance | No deployment; Rinkeby deprecated | None | No reproducible logs | Expected for PoA | None | UNSUPPORTED | Rinkeby deprecated; no logs; no reproducibility | P0 | Re-measure on Hardhat/Sepolia; provide logs |
| F2 | "On-chain anchoring: 196–224 bytes of calldata regardless of payload size" | Sec. 6 | Storage | Design-consistent | None | None | Analytically computable from struct | None | PARTIALLY SUPPORTED | Could be verified analytically; needs actual measurement | P1 | Verify with gas reporter |
| F3 | "Storage reduction 12:1 to 217:1 vs. direct on-chain storage" | Sec. 6, 7 | Storage | Design-consistent calculation | None | None | Derivable from payload size / calldata size | None | PARTIALLY SUPPORTED | Analytical derivation plausible; presented as experimental | P1 | Clearly label as analytical comparison |
| F4 | "Gas cost: 68,400–94,200 gas per anchoring event" | Sec. 6 | Performance (Gas) | No deployment receipts | None | None | Plausible estimate | None | UNSUPPORTED | No gas reporter output; no deployment | P0 | Run Hardhat gas reporter |
| F5 | "$0.15–$0.42 per anchoring event; $12–$180 for direct storage" | Sec. 6 | Performance (Cost) | No data | None | None | Depends on ETH price and gas price, both unspecified | None | UNSUPPORTED | ETH/USD rate and gwei price not specified or dated | P1 | Specify basis; use range analysis |
| F6 | "50 IPFS retrievals, 100% integrity verification, mean latency 340 ms (SD=89 ms)" | Sec. 6 | Performance (IPFS) | No scripts, no data files, no logs | None | None | N/A | None | UNSUPPORTED | No artefact; local daemon result non-representative | P0 | Re-measure; provide scripts and logs |
| F7 | "End-to-end workflow: 18–22 s (client encryption → IPFS → anchor → confirmation)" | Sec. 6 | Performance | No logs | None | None | Consistent with block time + IPFS latency sum | None | UNSUPPORTED | Derived; not measured; no artefact | P1 | Measure if prototype is implemented |
| F8 | "One outlier of 114 s in block time (Experiment 5, Block 1) attributed to network congestion" | Sec. 6 | Performance | No logs | None | None | N/A | None | UNSUPPORTED | Cannot verify outlier or attribution | P1 | Report with logs |
| F9 | "Consistent 180 s for 12 blocks (excluding outlier)" | Sec. 6 | Performance | No logs | None | None | 12 × 15 s ≈ 180 s is consistent | None | PARTIALLY SUPPORTED | Arithmetic consistent but outlier exclusion unjustified | P1 | Report with complete dataset |
| F10 | "System scales to national deployment level" | Sec. 7 | Scalability | Design claim | None | None | N/A — explicitly acknowledged as unverified | None | UNSUPPORTED | Prototype evaluated with ≤10 nodes; acknowledged in limitations | P1 | Retain in limitations; remove from claims |

---

## Section G — Interoperability and FHIR Claims

| ID | Claim Summary | Location | Category | Implementation Evidence | Test Evidence | Experimental Evidence | Mathematical/Analytical Evidence | Citation Evidence | Classification | Problem | Severity | Required Action |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| G1 | "FHIR-aware interoperability workflows supporting standards-compliant selective disclosure" | Abstract, Intro | Interoperability | None | None | None | Design description only | FHIRChain (2018) | DESIGN/FUTURE WORK ONLY | No FHIR library, no FHIR message handling | P1 | Move to future work or implement |
| G2 | "Cross-organizational data exchange within the privacy architecture" | Intro | Interoperability | None | None | None | Described | None | DESIGN/FUTURE WORK ONLY | No implementation | P1 | Move to future work |

---

## Section H — Edge/Fog and Resource-Constraint Claims

| ID | Claim Summary | Location | Category | Implementation Evidence | Test Evidence | Experimental Evidence | Mathematical/Analytical Evidence | Citation Evidence | Classification | Problem | Severity | Required Action |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| H1 | "Edge and fog preprocessing to reduce bandwidth, latency, and compute burden at peripheral facilities" | Abstract, Intro | Edge/Fog | Next.js middleware described as edge/fog proxy | None | None | Motivation sound | None | DESIGN/FUTURE WORK ONLY | "Edge/fog" is server-side Next.js; no actual edge nodes | P1 | Reframe as application-layer preprocessing |
| H2 | "Resource-constrained facilities participate without operating as heavy validator nodes" | Intro, Sec. 4 | Resource Constraints | Design specification | None | None | Role separation consistent with design | None | PARTIALLY SUPPORTED | Architectural commitment; not tested under real constraints | P1 | Label as design; note future testing requirement |
| H3 | "Local buffer queue for submissions during connectivity interruptions" | Sec. 5 | Edge/Fog | Described in Tier 2; no code | None | None | Standard pattern | None | DESIGN/FUTURE WORK ONLY | No implementation or test | P1 | Implement or reframe |
| H4 | "Production IPFS latency 3–8 s higher (architectural analysis; not empirically validated)" | Sec. 6 | Resource Constraints | Explicitly labeled as architectural analysis | None | None | Reasonable estimate | None | PARTIALLY SUPPORTED | Honest qualification; retain with label | P2 | Keep as is |

---

## Section I — Consensus Claims

| ID | Claim Summary | Location | Category | Implementation Evidence | Test Evidence | Experimental Evidence | Mathematical/Analytical Evidence | Citation Evidence | Classification | Problem | Severity | Required Action |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| I1 | "Comparative consensus framework evaluating PoA, DPoS, PBFT, Raft" | Abstract, Intro | Consensus | None | None | None | Descriptive table only | None | DESIGN/FUTURE WORK ONLY | No measurements; description only | P1 | Conduct measurements or reframe as design rationale |
| I2 | "PoA selected for prototype for low latency and deterministic finality" | Sec. 2 | Consensus | Ganache/Rinkeby use implies PoA | None | None | Rationale stated | None | PARTIALLY SUPPORTED | Rationale stated but not empirically compared | P2 | Retain as design choice with literature support |
| I3 | "Validator appointment from Tier 3+ institutional actors" | Sec. 4 | Governance | Design specification | None | None | Reasonable governance model | None | DESIGN/FUTURE WORK ONLY | Design; not tested | P2 | Retain as design |

---

## Section J — Usability Claims

| ID | Claim Summary | Location | Category | Implementation Evidence | Test Evidence | Experimental Evidence | Mathematical/Analytical Evidence | Citation Evidence | Classification | Problem | Severity | Required Action |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| J1 | "SUMI evaluation n=26 domain professionals: 13 epidemiologists, 7 PHWs, 6 lab technicians" | Sec. 6 | Usability | No raw data, no SUMI response records | None | No reproducible data | N/A | bevan1995measuring | UNSUPPORTED | No data; no ethics approval documentation | P0 | Re-conduct or remove; document ethics |
| J2 | "SUMI scores 60.46–68.96 across all six scales" | Sec. 6, 7 | Usability | No raw data | None | No reproducible data | N/A | None | UNSUPPORTED | No data supporting these specific values | P0 | Re-conduct with implemented prototype |
| J3 | "All 95% CIs entirely above 50-point population mean" | Sec. 6 | Usability | No raw data | None | None | N/A | None | UNSUPPORTED | Cannot compute CIs without raw data | P0 | Re-conduct |
| J4 | "High control score (64.23) suggests hierarchical design aligns with mental model" | Sec. 7 | Usability | No raw data | None | None | Interpretive | None | UNSUPPORTED | Interpretation of unsupported score | P0 | Remove or re-conduct |
| J5 | "26 evaluators interacted with system 15–20 minutes" | Sec. 6 | Usability | No prototype existed | None | None | N/A | None | CONTRADICTED | No prototype exists; participants cannot have interacted with it | P0 | Re-conduct with implemented system |
| J6 | "Academic SUMI license obtained" | Sec. 8 | Ethics | Claimed in acknowledgments | None | None | N/A | None | PARTIALLY SUPPORTED | Plausible claim; no license number provided | P2 | Document license |

---

## Section K — Ethics, Governance, and Social Claims

| ID | Claim Summary | Location | Category | Implementation Evidence | Test Evidence | Experimental Evidence | Mathematical/Analytical Evidence | Citation Evidence | Classification | Problem | Severity | Required Action |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| K1 | "Participants briefed and provided voluntary agreement to participate" | Sec. 8 (Ethics) | Ethics | Stated claim | None | None | N/A | None | PARTIALLY SUPPORTED | Not equivalent to documented informed consent; no IRB/ethics committee approval | P1 | Document ethics procedure; confirm exemption or approval |
| K2 | "No personal health data collected; questionnaire responses anonymous" | Sec. 8 | Ethics | Stated claim | None | None | N/A | None | PARTIALLY SUPPORTED | If true, minimal-risk exemption plausible | P2 | Confirm and document |
| K3 | "82.85% affirmative responses in blockchain suitability survey (n=10)" | Sec. 3 | Suitability | Reported in methodology | None | None | N/A | None | UNSUPPORTED | n=10 at single institute; no raw data; no survey instrument | P1 | Provide instrument and data or qualify heavily |
| K4 | "Equity-aware governance as part of threat model" | Abstract, Sec. 4 | Governance | Described in threat model section | None | None | Analytical | None | PARTIALLY SUPPORTED | Descriptive governance model; no empirical equity analysis | P2 | Retain as design aspiration |

---

## Section L — Auditability and Immutability Claims

| ID | Claim Summary | Location | Category | Implementation Evidence | Test Evidence | Experimental Evidence | Mathematical/Analytical Evidence | Citation Evidence | Classification | Problem | Severity | Required Action |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| L1 | "Immutable on-chain provenance trail including rework requests and supervisory feedback" | Sec. 5, 7 | Auditability | Listing emits events for review/rework | None | None | Standard Ethereum event log property | None | PARTIALLY SUPPORTED | Events are immutable; no log pruning risk for light clients | P2 | Clarify event log vs. state distinction |
| L2 | "Complete supervisory audit trail" | Sec. 5 | Auditability | Events listed in ReportCoordinator | None | None | Analytically follows from event design | None | PARTIALLY SUPPORTED | Events are incomplete without caller authentication | P1 | Add caller validation |

---

*Matrix covers all major scientific and technical claims identified through complete reading of original/sections/*.tex. Claims not listed are minor framing statements or background facts.*
