# Reviewer Questions Matrix

**Manuscript:** Selective Anchoring Architecture for LMIC Disease Surveillance  
**Prepared:** 2026-08-12  
**Purpose:** Anticipate hostile but realistic reviewer questions before manuscript resubmission  

Questions are organized by theme. Each entry includes the question, why a reviewer would ask it, the current manuscript answer, evidence currently available, evidence missing, required revision, and whether additional implementation or experimentation is required.

---

## Theme 1: Novelty

---

**RQ-N1. What exactly is new in this work?**

*Why a reviewer asks this:* The combination of IPFS, blockchain, AES encryption, and hierarchical access control has been described in dozens of papers since 2017. The reviewer needs a precise statement of what is new.

*Current manuscript answer:* Lists six contributions, including selective anchoring, ZKP privacy, edge/fog, consensus comparison, FHIR workflows, and a functional prototype.

*Evidence available:* Architectural description of selective anchoring for LMIC surveillance hierarchy.

*Evidence missing:* ZKP not implemented. FHIR not implemented. Edge/fog is server-side middleware. No comparable system with this specific LMIC-PHEM tier mapping is cited.

*Required revision:* Narrow to 2–3 contributions. The defensible novelty is: (a) selective-anchoring design explicitly mapped to the Ethiopian PHEM administrative hierarchy; (b) honest threat model for the IPFS/blockchain hybrid including revocation limitations; (c) implemented and tested Solidity contracts with gas characterization.

*Needs implementation:* Yes (contracts, tests, gas data). *Needs experiment:* No (design novelty is analytical).

---

**RQ-N2. How does this differ from FHIRChain (Zhang et al. 2018)?**

*Why:* FHIRChain also combines blockchain coordination with off-chain storage and FHIR interoperability. Reviewers familiar with the field will ask directly.

*Current answer:* Claims "FHIR-aware interoperability workflows" as a contribution without comparing implementation depth.

*Evidence available:* None for FHIR implementation.

*Evidence missing:* FHIR implementation; honest analysis of what FHIRChain does vs. proposed.

*Required revision:* Either implement FHIR integration or remove it from claimed contributions. Compare honestly: FHIRChain uses centralized off-chain storage; the proposed design uses content-addressed IPFS with encrypted payloads — that is a defensible distinction without claiming FHIR implementation.

*Needs implementation:* If FHIR retained, yes. *Needs experiment:* No.

---

**RQ-N3. How does this differ from Hyperledger Fabric private data collections?**

*Why:* Fabric private data collections provide hybrid on/off-chain storage, access control, and endorsement policies, targeted at permissioned enterprise contexts. This is arguably the most directly competing architecture. It is not mentioned in the manuscript.

*Current answer:* Fabric not cited or compared.

*Evidence available:* None.

*Evidence missing:* Analysis of Fabric private data collections.

*Required revision:* Add Fabric to related work table. Argue for Ethereum-based public-permissioned model vs. Fabric private model — the public auditability argument is legitimate.

*Needs implementation:* No. *Needs experiment:* No (analytical comparison sufficient).

---

**RQ-N4. The "selective anchoring" term — is this a novel concept or a renaming of existing hash-pointer hybrid storage?**

*Why:* Hash-pointer hybrid storage (store hash on-chain, data off-chain) is a well-known pattern. The term "selective anchoring" does not appear in the literature provided.

*Current answer:* The term is presented as a new architectural concept.

*Evidence available:* The term is applied to a specific context (LMIC disease surveillance hierarchy) that adds design-level specificity.

*Evidence missing:* Systematic comparison to existing hybrid storage terms.

*Required revision:* Acknowledge the hash-pointer pattern as the technical basis; argue novelty in the application context and the explicit treatment of IPFS enforcement limitations.

*Needs implementation:* No. *Needs experiment:* No.

---

**RQ-N5. The related work table marks all prior work "No" on most dimensions and the proposed work "Yes" on all. This seems self-serving. What is the justification?**

*Why:* Comparison tables that conveniently favor the proposed work are a red flag for reviewers.

*Current answer:* Table compares against Omar, Griggs, Quaini, Zhang, Bollig, Harvey, Rubio-Solis. All lack one or more dimensions.

*Evidence available:* The selected comparators genuinely lack several claimed dimensions.

*Evidence missing:* Hyperledger Fabric, MedRec, OmniPHR, and other LMIC-specific health blockchain systems are not included.

*Required revision:* Include stronger comparators. Acknowledge partial overlaps honestly.

*Needs implementation:* No. *Needs experiment:* No.

---

## Theme 2: Privacy

---

**RQ-P1. What happens if an attacker obtains an IPFS CID?**

*Why:* IPFS is a public network. Anyone with a CID can retrieve the content from any IPFS gateway. The privacy model must address this.

*Current answer:* "Content is encrypted; attacker obtains only ciphertext."

*Evidence available:* Design claim.

*Evidence missing:* Key management system. How are keys distributed? Who holds them? How are they rotated?

*Required revision:* Add key management design section. Explicitly state that privacy depends on key confidentiality, not on CID confidentiality.

*Needs implementation:* Yes (key distribution mechanism). *Needs experiment:* No.

---

**RQ-P2. What exactly does revoking access revoke?**

*Why:* `revokeAccess()` sets a flag in contract storage. IPFS content remains accessible to anyone with the CID. Keys already distributed are not recalled.

*Current answer:* Claims "revoking access" as a privacy mechanism; discussion states "IPFS content is accessible only when the `AccessControl` contract confirms the requester's authorization."

*Evidence available:* Contract listing showing flag-setting only.

*Evidence missing:* Key revocation mechanism; re-encryption scheme; proxy re-encryption.

*Required revision:* Acknowledge the semantic gap. State that blockchain revocation prevents future authorized API calls; it does not revoke already-distributed keys or prevent direct IPFS retrieval. Propose key rotation as a mitigating design.

*Needs implementation:* Yes (key rotation or proxy re-encryption). *Needs experiment:* No.

---

**RQ-P3. What prevents a revoked party from directly querying IPFS for content they were previously authorized to access?**

*Why:* This is the same as RQ-P2 but from an attacker model perspective.

*Current answer:* Nothing explicitly.

*Evidence available:* None.

*Evidence missing:* Key revocation mechanism.

*Required revision:* Address directly. Options: (a) IPFS pinning control + key rotation (most practical); (b) proxy re-encryption; (c) explicit statement that revocation is application-layer and does not prevent direct CID access.

*Needs implementation:* Yes. *Needs experiment:* No.

---

**RQ-P4. SHA-256 of plaintext is stored on-chain as an integrity anchor. For low-entropy health records, this enables hash-cracking re-identification. How is this addressed?**

*Why:* Disease codes from a set of 20, Boolean flags, geographic codes — an adversary can enumerate likely record contents and compare hashes. SHA-256 is not a keyed function.

*Current answer:* Not addressed. The manuscript presents this as an integrity feature.

*Evidence available:* The vulnerability is derivable from the contract listing.

*Evidence missing:* HMAC or keyed commitment scheme.

*Required revision:* Replace `SHA-256(plaintext)` with `HMAC-SHA-256(key, plaintext)` or a commitment scheme with a secret nonce. Acknowledge re-identification risk.

*Needs implementation:* Yes (modify contract). *Needs experiment:* No.

---

**RQ-P5. What metadata is visible on-chain, and what can traffic analysis reveal?**

*Why:* Even if payload content is off-chain, on-chain metadata (reporter address, timestamp, tier level, CID, completeness counts) reveals surveillance patterns.

*Current answer:* "No epidemiological content — patient identifiers, disease codes, geographic coordinates — appears on the public ledger." Also claims "metadata leakage mitigation."

*Evidence available:* Accurate that raw patient data is not on-chain.

*Evidence missing:* Analysis of what metadata analysis can reveal.

*Required revision:* Acknowledge metadata exposure. Note that reporter address pseudonymity, not anonymity; tier identifiers reveal organizational structure; timing patterns reveal outbreak timing. Propose countermeasures or acknowledge as limitation.

*Needs implementation:* No. *Needs experiment:* No (analytical).

---

**RQ-P6. Is key rotation implemented?**

*Why:* Standard question for any system claiming revocable encryption-based access control.

*Current answer:* Not mentioned.

*Evidence available:* None.

*Evidence missing:* Key management design.

*Required revision:* Add key rotation design. Even a design-level specification is better than silence.

*Needs implementation:* Yes for the claim; No if reframed as future work.

---

**RQ-P7. How is the encryption key distributed to authorized parties?**

*Why:* AES-256-GCM is specified for encryption; the key distribution mechanism is absent from the design.

*Current answer:* Not addressed.

*Evidence available:* None.

*Evidence missing:* Key distribution protocol (ECIES, asymmetric wrapping, KEM, or similar).

*Required revision:* Design and describe a key distribution mechanism. This is architecturally essential, not optional.

*Needs implementation:* Design level minimum; implementation for full prototype. *Needs experiment:* No.

---

**RQ-P8. Does plaintext hashing create any linkage between records that could enable deanonymization?**

*Why:* If two reports have the same hash, an observer knows they have identical content.

*Current answer:* Not addressed.

*Evidence available:* Derivable from contract design.

*Evidence missing:* Analysis of hash collision/linkage risk.

*Required revision:* Add nonce to commitment scheme to prevent linkage.

*Needs implementation:* Minor contract change. *Needs experiment:* No.

---

## Theme 3: Security

---

**RQ-S1. `grantAccess()` and `revokeAccess()` have no `onlyOwner` or role-based modifier. Any Ethereum address can grant or revoke permissions on any report. How is this not a critical access control vulnerability?**

*Why:* This is directly visible in the contract listing. Any reviewer examining the Solidity code will see it.

*Current answer:* Claims "hierarchical access control enforced programmatically." This is false for the listed code.

*Evidence available:* The listing confirms the vulnerability.

*Evidence missing:* Access control modifiers.

*Required revision:* Add `onlyReporter(reportId)` or `onlyGranter` modifier. Fix before submission.

*Needs implementation:* Yes (trivial fix). *Needs experiment:* No.

---

**RQ-S2. Can unauthorized actors call `anchorReport()`?**

*Why:* The `ReportCoordinator.anchorReport()` also has no access restriction — any address can anchor a report.

*Current answer:* Claims "reporters operate as lightweight clients" implying role restriction; no code enforces this.

*Evidence available:* Listing shows no modifier.

*Evidence missing:* Reporter registration and function-level access control.

*Required revision:* Add reporter registry and `onlyRegisteredReporter` modifier.

*Needs implementation:* Yes. *Needs experiment:* No.

---

**RQ-S3. What happens if a facility is compromised?**

*Why:* Standard threat model question for any distributed system.

*Current answer:* Threat model section mentions "facility compromise" but treatment is superficial.

*Evidence available:* Threat table exists.

*Evidence missing:* Threat model depth (what specifically does an attacker gain from a compromised Tier 1 node?).

*Required revision:* Expand threat model: compromised facility can submit false anchors under its own address, poison completeness counts, and leak CIDs to external parties.

*Needs implementation:* No. *Needs experiment:* No.

---

**RQ-S4. What is the validator trust model? Who appoints validators and how are they removed?**

*Why:* PoA requires validator keys to be trusted. Key compromise or validator collusion breaks consensus finality.

*Current answer:* "Validators drawn from Tier 3+ institutional actors" — one sentence.

*Evidence available:* Design specification.

*Evidence missing:* Validator onboarding/removal ceremony; key management for validators.

*Required revision:* Expand governance model for validator lifecycle.

*Needs implementation:* No. *Needs experiment:* No.

---

**RQ-S5. Are replay attacks addressed?**

*Why:* If an attacker captures an anchoring transaction and replays it, a duplicate anchor is created.

*Current answer:* Not mentioned.

*Evidence available:* Ethereum transaction nonces provide natural replay protection.

*Evidence missing:* Explicit acknowledgment.

*Required revision:* Add note that Ethereum transaction nonces prevent replay at the network layer; address application-layer duplicate submission separately.

*Needs implementation:* No. *Needs experiment:* No.

---

**RQ-S6. Who can upgrade or redeploy the smart contracts?**

*Why:* If a contract owner can upgrade the access control contract, historical permission audits become untrustworthy.

*Current answer:* "Factory pattern prevents post-deployment tampering" — this is misleading; factory deployment does not prevent contract upgrades or redeployment.

*Evidence available:* Listing shows no upgrade mechanism (but also no explicit immutability commitment).

*Evidence missing:* Contract governance model (immutable vs. upgradeable; who has admin key?).

*Required revision:* Explicitly state whether contracts are intended to be immutable. If upgradeable, describe governance of upgrade keys.

*Needs implementation:* No (design decision). *Needs experiment:* No.

---

**RQ-S7. What validator collusion fraction breaks the PoA consensus?**

*Why:* In PoA, a majority of validators colluding can fork the chain or censor transactions.

*Current answer:* "Expanded threat model addresses validator collusion" — included as a claim but not analyzed.

*Evidence available:* Threat table mentions collusion.

*Evidence missing:* Collusion threshold analysis; Byzantine fraction; mitigation.

*Required revision:* Provide collusion threshold analysis (e.g., f < n/2 for PoA clique consensus).

*Needs implementation:* No. *Needs experiment:* No.

---

## Theme 4: Architecture

---

**RQ-A1. Why blockchain and not a signed append-only transparency log (e.g., Certificate Transparency-style)?**

*Why:* A signed append-only log (Merkle tree with public audit) provides similar integrity and auditability properties with much lower overhead and without permissioned network complexity.

*Current answer:* Not addressed. The architecture is motivated by blockchain properties but does not argue against simpler alternatives.

*Evidence available:* None.

*Evidence missing:* Explicit comparison to transparency log alternatives.

*Required revision:* Add comparison to transparency log model. Argue where smart contract programmability adds value (access control automation, completeness verification, provenance lifecycle) beyond what a simple log provides.

*Needs implementation:* No. *Needs experiment:* No.

---

**RQ-A2. Why not a conventional centralized database with signed audit records?**

*Why:* This is the standard "why blockchain?" challenge. Must be answered directly.

*Current answer:* Brief mention that centralization creates single-point-of-failure risk. 

*Evidence available:* Motivation is reasonable but underdeveloped.

*Evidence missing:* Analysis of specific LMIC governance failures that blockchain addresses.

*Required revision:* Articulate the trust model. The key argument should be: multiple independent institutional actors must share data without trusting a common operator; this is the genuine use case for blockchain.

*Needs implementation:* No. *Needs experiment:* No.

---

**RQ-A3. Why Ethereum-compatible infrastructure rather than Hyperledger Fabric for a permissioned health consortium?**

*Why:* Fabric is widely used for exactly this type of multi-institutional permissioned use case.

*Current answer:* Ethereum comparison table compares against Bitcoin and XRPL — not against Fabric.

*Evidence available:* None.

*Evidence missing:* Fabric comparison.

*Required revision:* Add Fabric to platform comparison table. Argue for Ethereum: public ledger auditability, wider developer ecosystem, IPFS compatibility, existing tooling.

*Needs implementation:* No. *Needs experiment:* No.

---

**RQ-A4. What role does "selective anchoring" actually play beyond being a named pattern for hash-pointer off-chain storage?**

*Why:* The term is used as if it is a technical contribution, but the underlying mechanism is standard.

*Current answer:* "Selective anchoring" is framed as an architectural contribution; the pattern itself is standard.

*Evidence available:* The application to LMIC PHEM tiers adds context-specific design.

*Evidence missing:* Analysis of how the design choices differ from a generic hash-pointer hybrid.

*Required revision:* Argue novelty in the specific anchoring policy (what triggers anchoring, what is included in the on-chain record, how the tier structure shapes anchoring decisions).

*Needs implementation:* No. *Needs experiment:* No.

---

**RQ-A5. The CompletenessVerifier is described as verifying field-level completeness of encrypted payloads. How does a smart contract inspect encrypted content?**

*Why:* Encrypted ciphertext is opaque to all parties including the contract.

*Current answer:* Discussion states "CompletenessVerifier ensures that only reports meeting mandatory field requirements enter the provenance chain." The listing shows event counting only.

*Evidence available:* Listing contradicts the discussion.

*Evidence missing:* Correct description of what the contract actually does.

*Required revision:* Correct the description. The verifier counts anchor events per period. Field-level completeness, if desired, would require either (a) trusted off-chain pre-verification with on-chain attestation, or (b) ZKP circuits (not implemented). Both require explicit design.

*Needs implementation:* Yes if field-level verification is claimed. *Needs experiment:* No.

---

## Theme 5: Resource Constraints and LMIC Deployment

---

**RQ-R1. What does "resource-constrained" actually mean in this context? What specific device specifications, bandwidth limits, and connectivity patterns were assumed?**

*Why:* "Resource-constrained" is used throughout without quantification.

*Current answer:* Mentions "intermittent connectivity, underpowered devices, limited technical capacity" without numbers.

*Evidence available:* General Ethiopia PHEM context.

*Evidence missing:* Specific connectivity assumptions (e.g., 2G/3G, 50 kbps), device specifications (RAM, CPU), and offline duration assumptions.

*Required revision:* Provide quantified resource assumptions. This is essential for evaluating whether the proposed architecture actually satisfies them.

*Needs implementation:* No. *Needs experiment:* Ideally yes for LMIC network conditions.

---

**RQ-R2. The IPFS local-daemon latency is 340 ms. What is the expected latency over a 2G network with a distributed IPFS gateway?**

*Why:* Local daemon is essentially localhost. Over real LMIC networks, retrieval latency could be orders of magnitude higher.

*Current answer:* "Production conditions with remote IPFS gateways expected to increase by 3–8 seconds (architectural analysis; not yet empirically validated at scale)."

*Evidence available:* Honest qualification in the text.

*Evidence missing:* Empirical measurement.

*Required revision:* Retain qualification; ideally conduct measurements over a simulated degraded network.

*Needs implementation:* No. *Needs experiment:* Yes (future work or included evaluation).

---

**RQ-R3. What happens during offline operation? How does a Tier 1 node submit when disconnected from the blockchain?**

*Why:* If offline operation is a design requirement, the submission pathway must work without blockchain connectivity.

*Current answer:* "Local buffer queue retransmits upon reconnection."

*Evidence available:* Design description only.

*Evidence missing:* Queue persistence design; conflict resolution if submission is delayed; time-stamping under offline conditions.

*Required revision:* Expand offline operation design. Note that MetaMask + Ethereum node connectivity is required for anchoring; IPFS may be available through local gateway; describe the hybrid offline pathway.

*Needs implementation:* Yes for production. *Needs experiment:* Yes.

---

**RQ-R4. Are the Rinkeby performance measurements representative of production LMIC network conditions?**

*Why:* Rinkeby is a public test network run from well-connected nodes in data centers. LMIC production conditions involve constrained bandwidth, variable latency, and intermittent connectivity.

*Current answer:* States evaluation conducted on Rinkeby test network without noting representativeness limitations.

*Evidence available:* Limitations section acknowledges PoA scope.

*Evidence missing:* Acknowledgment of Rinkeby non-representativeness for LMIC; Rinkeby is deprecated.

*Required revision:* Replace Rinkeby with local Hardhat/Ganache measurements. Acknowledge that on-chain transaction costs depend on network gas prices; LMIC deployments would use a private PoA chain with negligible gas costs.

*Needs implementation:* Yes (re-measure). *Needs experiment:* Yes.

---

## Theme 6: Evaluation

---

**RQ-E1. What produced each specific number in the results? Provide the script, the raw output, and the analysis procedure.**

*Why:* This is the standard reproducibility check. Without it, results cannot be verified.

*Current answer:* Results presented as fact without supporting artefacts.

*Evidence available:* None.

*Evidence missing:* Scripts, logs, data files.

*Required revision:* For every quantitative result, provide: the script used to produce it, the raw output, the analysis step that derived the reported value.

*Needs implementation:* Yes. *Needs experiment:* Yes.

---

**RQ-E2. What is the baseline for the storage reduction calculation?**

*Why:* "12:1 to 217:1 reduction" requires a comparison. What is the baseline architecture?

*Current answer:* "Relative to direct on-chain storage of equivalent payloads."

*Evidence available:* Analytically computable from payload size and calldata size figures.

*Evidence missing:* Explicit calculation, not just assertion.

*Required revision:* Provide the calculation transparently, ideally as a script. Label as analytical comparison.

*Needs implementation:* No (analytical). *Needs experiment:* No.

---

**RQ-E3. The outlier block time of 114 seconds is excluded from the analysis. Why? Is this exclusion statistically justified?**

*Why:* Selectively excluding outliers without justification inflates the appearance of stability.

*Current answer:* "Attributable to network congestion on the shared test network."

*Evidence available:* Assertion only.

*Evidence missing:* Statistical justification; full dataset.

*Required revision:* Report all data. If outlier exclusion is retained, apply a pre-specified criterion (e.g., Grubbs' test) and document it.

*Needs implementation:* No. *Needs experiment:* Yes (new measurements on reproducible chain).

---

**RQ-E4. The SUMI evaluation uses 26 participants from a single institute. Is this sample sufficient for statistical significance claims?**

*Why:* SUMI norms are based on larger samples. CIs with n=26 will be wide.

*Current answer:* Claims "statistically significant above-average usability at the 95% confidence level."

*Evidence available:* No raw data to verify.

*Evidence missing:* Raw SUMI responses; statistical analysis code.

*Required revision:* If re-conducted, report raw data (aggregated), confidence intervals, effect sizes. Do not overstate significance for a single-institute sample.

*Needs implementation:* Yes (actual prototype needed). *Needs experiment:* Yes.

---

**RQ-E5. Are the synthetic test payloads described as representative of real Ethiopian disease surveillance reports?**

*Why:* "Ten representative report types" are described but no actual PHEM reporting templates are referenced.

*Current answer:* Lists "single case notification to weekly aggregate with laboratory attachments."

*Evidence available:* Ethiopia PHEM reporting format is a known public document.

*Evidence missing:* Reference to PHEM reporting templates; confirmation that test payloads reflect real reporting structure.

*Required revision:* Reference actual PHEM reporting templates. Use them to construct realistic test payloads.

*Needs implementation:* No (documentation). *Needs experiment:* Yes (test with realistic payloads).

---

**RQ-E6. Where is the specification of the 24 test scenarios for the AccessControl contract?**

*Why:* "24 test cases" is claimed with no test file, no scenario description beyond two sentences.

*Current answer:* "Permission matrix covering authorized access, cross-boundary attempts, grant/revoke sequences, expired-permission rejection."

*Evidence available:* None.

*Evidence missing:* Test code, test output.

*Required revision:* Write and run the test suite. Publish the test file.

*Needs implementation:* Yes. *Needs experiment:* Yes (run tests).

---

## Theme 7: Healthcare and Governance

---

**RQ-H1. Blockchain ledgers are immutable. How does this interact with health data deletion and right-to-erasure requirements (GDPR Article 17; Ethiopian data protection law)?**

*Why:* Any paper deploying blockchain in a healthcare context must address erasure rights.

*Current answer:* Not addressed.

*Evidence available:* None.

*Evidence missing:* Analysis of immutability vs. erasure requirements.

*Required revision:* Address explicitly. Key argument: personal data is off-chain and encrypted; on-chain data contains only hashes and metadata (pseudonymous). Erasure can be accomplished by destroying the encryption key ("cryptographic deletion"). Document this design choice.

*Needs implementation:* Key deletion pathway. *Needs experiment:* No.

---

**RQ-H2. What are the data sovereignty implications of IPFS storage for Ethiopian national health data?**

*Why:* IPFS content is replicated across nodes worldwide. Ethiopian health data may be subject to national data sovereignty requirements.

*Current answer:* Not addressed.

*Evidence available:* None.

*Evidence missing:* Analysis of IPFS content propagation and data sovereignty.

*Required revision:* Specify that IPFS nodes are operated by health system actors within Ethiopia; private IPFS cluster configuration for data sovereignty.

*Needs implementation:* Deployment design. *Needs experiment:* No.

---

**RQ-H3. How are health-data governance regulations in Ethiopia addressed?**

*Why:* Ethiopia has a Data Protection Act (enacted 2024). Any health data system must comply.

*Current answer:* Not addressed.

*Evidence available:* None.

*Evidence missing:* Discussion of Ethiopian data protection requirements.

*Required revision:* Add a section or paragraph on governance compliance. Reference relevant Ethiopian law.

*Needs implementation:* No. *Needs experiment:* No.

---

**RQ-H4. Is DHIS2/eIDSR integration addressed? Ethiopia uses DHIS2 for national disease reporting. How does the proposed system interact with existing infrastructure?**

*Why:* Any practical surveillance system in Ethiopia must account for DHIS2.

*Current answer:* DHIS2 not mentioned.

*Evidence available:* None.

*Evidence missing:* Integration pathway analysis.

*Required revision:* Acknowledge DHIS2. Describe whether the proposed system supplements, replaces, or integrates with DHIS2. A selective-anchoring layer for existing DHIS2 submissions would be a more practically grounded contribution.

*Needs implementation:* No (design level). *Needs experiment:* No.

---

## Theme 8: Ethics and Usability

---

**RQ-U1. Was the usability study approved by an institutional ethics committee?**

*Why:* Any study involving human participants requires ethics review, even for minimal-risk studies.

*Current answer:* "Participants briefed on research objectives and provided voluntary agreement." No ethics committee mentioned.

*Evidence available:* Ethics section in conclusion is minimal.

*Evidence missing:* IRB/ethics committee approval number or documented exemption.

*Required revision:* Document ethics approval or minimal-risk exemption from Bahir Dar University or Amhara Public Health Institute.

*Needs implementation:* Administrative. *Needs experiment:* No.

---

**RQ-U2. What exactly did the 26 participants interact with? The implementation section states the prototype does not implement edge/fog or Layer-2 components. Was the usability study conducted on an earlier, simpler version?**

*Why:* The manuscript now describes a "five-tier architecture" but the SUMI study pre-dates (apparently) the current manuscript's complexity. The relationship is unclear.

*Current answer:* "Interacted with the system for 15–20 minutes."

*Evidence available:* No prototype exists in repository.

*Evidence missing:* Description of what was actually evaluated; version of the prototype used.

*Required revision:* Describe precisely what system participants interacted with. If it was a simpler earlier version, say so and qualify the SUMI interpretation accordingly.

*Needs implementation:* Yes (current prototype). *Needs experiment:* Yes (re-evaluate).

---

**RQ-U3. Is a 15–20 minute interaction sufficient to evaluate usability of a complex multi-layer blockchain surveillance system?**

*Why:* SUMI requires genuine task experience. 15–20 minutes is very short for a complex system.

*Current answer:* Standard SUMI procedure followed.

*Evidence available:* None.

*Evidence missing:* Task list given to participants; what specific tasks were completed.

*Required revision:* Document tasks. Acknowledge that participants evaluated the interface, not the full backend complexity.

*Needs implementation:* No. *Needs experiment:* Yes (re-conduct with defined task list).

---

**RQ-U4. No control group or baseline usability comparison is provided. How are SUMI scores interpreted relative to comparable health information systems?**

*Why:* Scores above the population mean of 50 demonstrate above-average usability relative to all software, not specifically health information systems.

*Current answer:* Compared only to SUMI population mean (all software types).

*Evidence available:* None.

*Evidence missing:* Comparison to SUMI scores from DHIS2, eIDSR, or similar systems.

*Required revision:* Note the comparison basis. Ideally find published SUMI scores for comparable LMIC health IT systems.

*Needs implementation:* No. *Needs experiment:* Ideally yes.

---

*Total reviewer questions: 35. All represent realistic challenges from competent peer reviewers specializing in blockchain systems, healthcare IT, LMIC digital health, or distributed systems security.*
