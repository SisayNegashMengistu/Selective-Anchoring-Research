# Literature Gap Plan

**Manuscript:** Selective Anchoring Architecture for LMIC Disease Surveillance  
**Prepared:** 2026-08-12  
**Purpose:** Audit existing references; identify required literature domains; guide bibliography rebuild  

---

## Part 1: Audit of Existing references.bib

The bibliography file (`original/references.bib`) contains 14 entries. Each is assessed below.

| Key | Author(s) | Title | Year | Status | Issues |
|---|---|---|---|---|---|
| yaga2018blockchain | Yaga, Dylan; Mell, Peter; Roby, Nik; Scarfone, Karen | Blockchain Technology Overview | 2018 | **Likely verifiable** — NISTIR 8202 is a real NIST document | Missing report number (NISTIR 8202); incomplete metadata |
| kelly2019key | "E. Kelly and others" | "A Survey of Blockchain Scalability" | 2019 | **PLACEHOLDER** — `journal={Survey placeholder}`, `author={E. Kelly and others}` | Fabricated/incomplete bibliographic record |
| finlayson2019adversarial | Finlayson, Samuel G and others | Adversarial attacks on medical machine learning | 2019 | **Likely verifiable** — Finlayson et al., *Science* 2019 is a real paper | Author list incomplete; citation context is questionable (paper is about ML attacks, not blockchain metadata privacy) |
| bohme2015building | Böhme, Rainer and others | "Building a Better Bitcoin" | 2015 | **Unverifiable as titled** — No known paper titled "Building a Better Bitcoin" by Böhme | Likely fabricated or misremembered title; may be a mashup of other papers |
| omar2019privacy | "Omar, others" | Privacy Preserving Medical Record Sharing with Blockchain | 2019 | **PLACEHOLDER** — `journal={Journal placeholder}`, `author={Omar, others}` | Fabricated/incomplete |
| griggs2018healthcare | "Griggs, others" | Healthcare blockchains: rationale, architectures, and applications | 2018 | **PLACEHOLDER** — `journal={Journal placeholder}` | Possibly references a real survey; metadata fabricated |
| quaini2018model | "Quaini, others" | UniRec: A distributed electronic health record architecture | 2018 | **PLACEHOLDER** | Fabricated/incomplete |
| zhang2018fhirchain | Zhang, Peng and others | FHIRChain: Applying blockchain to securely and scalably share clinical data | 2018 | **Likely verifiable** — FHIRChain is a known paper; Zhang et al., *Computational and Structural Biotechnology Journal* 2018 | Author list incomplete (`and others`) |
| bollig2020machine | "Bollig, others" | Machine learning for syndromic surveillance | 2020 | **PLACEHOLDER** — `journal={Journal placeholder}` | Fabricated/incomplete |
| harvey2021predicting | "Harvey, others" | Predicting malaria early warning | 2021 | **PLACEHOLDER** — `journal={Journal placeholder}` | Fabricated/incomplete |
| rubiosolis2019zika | "Rubio-Solis, others" | Neural network-based predictive surveillance for dengue | 2019 | **PLACEHOLDER** — `journal={Journal placeholder}` | Fabricated/incomplete |
| nunamaker1990systems | Nunamaker, J.F.; Chen, M.; Purdin, T.D.M. | Systems development in information systems research | 1990 | **Verifiable** — *Journal of Management Information Systems* 1989/1990 — real paper | Year may be off by one (1989 vs 1990) |
| peck2017blockchain | Peck, Morgan | How the blockchain could revolutionize cybersecurity | 2017 | **Likely verifiable** — Peck has written blockchain articles for IEEE Spectrum | Cited in bib but appears not cited in manuscript text (unused reference) |
| cdc2012principles | Centers for Disease Control and Prevention | Principles of Epidemiology in Public Health Practice | 2012 | **Verifiable** — CDC self-study course publication exists | Not cited in manuscript text; incomplete URL/edition info |
| bevan1995measuring | Bevan, Nigel | Measuring usability as quality of use | 1995 | **Likely verifiable** — Bevan is the SUMI developer; *Software Quality Journal* | Incomplete metadata (no volume/pages) |

**Summary of existing bibliography:**
- Verifiable, adequate: 3–4 entries (yaga2018blockchain, zhang2018fhirchain, nunamaker1990systems, bevan1995measuring)
- Likely verifiable but incomplete: 2–3 entries (finlayson2019adversarial, peck2017blockchain, cdc2012principles)
- Placeholder/fabricated: 7 entries (kelly2019key, omar2019privacy, griggs2018healthcare, quaini2018model, bollig2020machine, harvey2021predicting, rubiosolis2019zika)
- Unverifiable as titled: 1 entry (bohme2015building)
- Unused in manuscript: peck2017blockchain, cdc2012principles

**Critical finding:** The majority of references cited in the Related Work section are placeholder records with `journal={Journal placeholder}` and `author={others}`. These must be replaced with verified bibliographic information before submission.

---

## Part 2: Required Literature Domains

The following domains must be covered in the revised bibliography. Priorities are marked H (High — essential), M (Medium — strongly recommended), L (Low — useful if available).

For each domain, candidate real publications are suggested where verifiable titles are known. **Unverified candidates are marked [VERIFY BEFORE USE].** Do not add any reference without verifying authors, title, journal/conference, year, volume/pages/DOI.

---

### Domain 1: Blockchain in Healthcare — General (H)

Essential foundational and review papers:

- Yaga et al. (2018) NISTIR 8202 — *Blockchain Technology Overview* [VERIFY: NISTIR 8202, NIST, 2018]
- Linn & Koo (2016) — "Blockchain for Health Data and Its Potential Use in Health IT and Healthcare Related Research" [VERIFY: ONC/RWC White Paper, 2016]
- Gordon & Catalini (2018) — "Blockchain Technology for Healthcare: Facilitating the Transition to Patient-Driven Interoperability" [VERIFY: *Computational and Structural Biotechnology Journal*, 2018]
- Agbo et al. (2019) — "Blockchain Technology in Healthcare: A Systematic Review" [VERIFY: *Healthcare*, MDPI, 2019]
- Mayer et al. (2020) — survey/systematic review of blockchain healthcare [VERIFY: search Google Scholar]

---

### Domain 2: Blockchain Disease Surveillance (H)

Directly relevant to the proposed system:

- Mackey et al. (2019/2020) — Use of blockchain for COVID/infectious disease surveillance [VERIFY: *npj Digital Medicine*, Lancet Digital Health, or similar]
- Sylim et al. (2018) — "Blockchain Technology for Detecting Falsified and Substandard Drugs in Distribution" — relevant to surveillance supply chain [VERIFY: *JMIR Research Protocols*, 2018]
- [VERIFY] Search: "blockchain disease surveillance" in IEEE Transactions on Biomedical Engineering, *Journal of Medical Internet Research*, *Epidemiology & Infection* 2018–2024

---

### Domain 3: Privacy-Preserving Health Data Sharing (H)

- Dubovitskaya et al. (2017/2019) — "Secure and Trustable Electronic Medical Records Sharing using Blockchain" [VERIFY: *AMIA Annual Symposium*, 2017, or *npj Digital Medicine* 2019]
- Azaria et al. (2016) — MedRec: "Using Blockchain for Medical Data Access and Permission Management" [VERIFY: *IEEE International Conference on Open and Big Data*, 2016]
- Xia et al. (2017) — "MeDShare: Trust-Less Medical Data Sharing Among Cloud Service Providers via Blockchain" [VERIFY: *IEEE Access*, 2017]

---

### Domain 4: Blockchain Access Control (H)

- Ouaddah et al. (2016) — "FairAccess: A New Blockchain-Based Access Control Framework for IoT" [VERIFY: *Security and Communication Networks*, Wiley, 2016]
- Zhang & Poslad (2018) — "Blockchain Based Enhanced Edge Intelligence and Secure Video Surveillance for IoT" — access control patterns [VERIFY]
- Maesa et al. (2019) — "Blockchain Based Access Control" [VERIFY: *IFIP International Conference on Distributed Applications and Interoperable Systems*, 2017/2019]

---

### Domain 5: Ciphertext-Policy Attribute-Based Encryption (CP-ABE) and Attribute Revocation (M)

If the revised paper claims fine-grained attribute-based access control:

- Bethencourt et al. (2007) — "Ciphertext-Policy Attribute-Based Encryption" [VERIFY: *IEEE S&P*, 2007] — foundational
- Waters (2011) — "Ciphertext-Policy Attribute-Based Encryption: An Expressive, Efficient, and Provably Secure Realization" [VERIFY: *PKC*, 2011]
- Yang et al. (2013) — "Attribute-Based Encryption for Fine-Grained Access Control of Encrypted Data" — revocation [VERIFY: *ACM CCS*, 2006 or similar]
- Search: "CP-ABE attribute revocation" for papers 2018–2024

---

### Domain 6: Proxy Re-Encryption (M)

If re-encryption is used for key revocation:

- Blaze, Bleumer & Strauss (1998) — "Divertible Protocols and Atomic Proxy Cryptography" — foundational PRE [VERIFY: *Eurocrypt*, 1998]
- Nunez et al. (2017) — "NuCypher KMS: Decentralized Key Management System" [VERIFY: *arXiv*, 2017]

---

### Domain 7: Encrypted Decentralized Storage and IPFS (H)

- Benet (2014) — "IPFS — Content Addressed, Versioned, P2P File System" [VERIFY: *arXiv*, 2014]
- Henningsen et al. (2020) — "Mapping the Interplanetary File System" [VERIFY: *IEEE IFIP Networking*, 2020]
- Confais et al. (2017) — "Performance Analysis of Object Store Systems in a Fog and Edge Computing Infrastructure" — IPFS in edge contexts [VERIFY]
- Search: "IPFS blockchain healthcare" 2019–2024 in IEEE/ACM

---

### Domain 8: Hyperledger Fabric and Private Data Collections (H)

This domain is conspicuously absent from the current manuscript. It must be added:

- Androulaki et al. (2018) — "Hyperledger Fabric: A Distributed Operating System for Permissioned Blockchains" [VERIFY: *EuroSys*, 2018] — foundational
- Hyperledger Fabric Private Data documentation [cite as technical documentation with URL]
- Search: "Hyperledger Fabric healthcare private data" 2019–2024

---

### Domain 9: Permissioned Ethereum and Besu (M)

- ConsenSys Quorum/Besu technical documentation [cite as technical documentation]
- Search: "enterprise Ethereum permissioned healthcare" 2019–2024

---

### Domain 10: DHIS2 and eIDSR (H)

- WHO/DHIS2 documentation — DHIS2 in disease surveillance [VERIFY: dhis2.org documentation and peer-reviewed papers]
- Seebregts et al. (2009) / Braa et al. — DHIS2 in LMIC health systems [VERIFY: various papers in *Health Information Management Journal*]
- WHO EWARS/eIDSR documentation [VERIFY: WHO technical reports]
- Search: "DHIS2 disease surveillance Ethiopia" 2015–2024

---

### Domain 11: Transparency Logs and Hash-Chain Alternatives (M)

- Laurie et al. (2013) — "Certificate Transparency" [VERIFY: *RFC 6962*, IETF, 2013]
- Al-Bassam et al. (2017) — "SCPKI: A Smart Contract-Based PKI and Identity System" — relevance to signed logs [VERIFY]
- Search: "append-only transparency log audit" 2018–2024

---

### Domain 12: Metadata Privacy and Traffic Analysis (M)

- Meiklejohn et al. (2013) — "A Fistful of Bitcoins: Characterizing Payments Among Men with No Names" — blockchain de-anonymization [VERIFY: *IMC*, 2013]
- Narayanan & Shmatikoff — privacy in blockchain systems [VERIFY]
- Search: "blockchain metadata privacy healthcare" 2018–2024

---

### Domain 13: Health Data Governance, GDPR, and Immutable Ledgers (H)

- Politou et al. (2018) — "Blockchain Mutability: Challenges and Proposed Solutions" [VERIFY: *IEEE Transactions on Emerging Topics in Computing*, 2019/2020]
- Search: "blockchain GDPR right to erasure" in *Computer Law & Security Review* or *Journal of Cybersecurity* 2018–2024
- Search: "blockchain health data governance" 2019–2024

---

### Domain 14: Ethiopian and LMIC Data Protection Law (H)

- Ethiopia Personal Data Protection Proclamation (2024) — official government document [VERIFY: Ethiopian Federal Gazette]
- Search: "Ethiopia health data privacy law" in *Global Health Action*, *Health Policy and Planning*, *BMC Health Services Research* 2020–2024

---

### Domain 15: LMIC Digital Health Infrastructure (H)

- Blaya et al. (2010) — "E-Health Technologies Show Promise In Developing Countries" [VERIFY: *Health Affairs*, 2010] — foundational LMIC digital health
- Mehl et al. (2017) — WHO SMART Guidelines framework [VERIFY: WHO technical report]
- Search: "digital health LMIC infrastructure Ethiopia" in *The Lancet Digital Health*, *PLOS Digital Health*, *BMC Medical Informatics and Decision Making* 2018–2024

---

### Domain 16: Offline-First and Disconnected Health Systems (H)

- Vital Wave Consulting — "Health Information Systems in Developing Countries" [VERIFY]
- Search: "offline-first health information system LMIC" 2015–2024
- OpenMRS offline capabilities documentation [VERIFY]

---

### Domain 17: Zero-Knowledge Proofs in Healthcare (M — if ZKP retained as future work)

- Groth (2016) — "On the Size of Pairing-Based Non-interactive Arguments" — foundational Groth16 zk-SNARK [VERIFY: *EUROCRYPT*, 2016]
- Bowe, Gabizon & Miers (2018) — Sapling/Zcash protocol [VERIFY]
- Search: "zero knowledge proof healthcare privacy" in IEEE/ACM 2019–2024

---

### Domain 18: Consensus Mechanisms for Permissioned Blockchains (M)

- Castro & Liskov (1999) — "Practical Byzantine Fault Tolerance" [VERIFY: *OSDI*, 1999] — foundational PBFT
- De Angelis et al. (2018) — "PBFT vs Proof-of-Authority: Applying the CAP Theorem to Permissioned Blockchain" [VERIFY: *ITASEC*, 2018]
- Search: "consensus mechanism comparison permissioned blockchain performance" 2018–2024

---

### Domain 19: SUMI and Usability Measurement (M)

- Kirakowski & Corbett (1993) — SUMI original validation paper [VERIFY: *Behaviour & Information Technology*, 1993]
- Bevan (1995) — "Measuring usability as quality of use" [cited; needs complete metadata]

---

## Part 3: Priority Action List for Literature Rebuild

### Immediate (P0 — before submission):

1. Replace all 7 placeholder bibliography entries with verified references or remove them
2. Resolve `bohme2015building` — find the actual paper intended or remove
3. Complete metadata for `zhang2018fhirchain` (full author list, volume, pages, DOI)
4. Complete metadata for `yaga2018blockchain` (NISTIR 8202, NIST, 2018, URL)
5. Complete metadata for `nunamaker1990systems` (volume, pages)
6. Complete metadata for `bevan1995measuring` (volume, pages, DOI)

### High Priority (P1 — strongly recommended):

7. Add Androulaki et al. (2018) Hyperledger Fabric paper
8. Add Azaria et al. (2016) MedRec paper
9. Add Benet (2014) IPFS paper
10. Add at least one verified DHIS2 surveillance paper
11. Add Agbo et al. or similar systematic review of blockchain healthcare
12. Add a post-2020 paper on blockchain-health privacy or hybrid storage
13. Add Ethiopia data protection regulation reference

### Medium Priority (P2):

14. Add transparency log / Certificate Transparency comparison
15. Add CP-ABE or proxy re-encryption reference if key management design includes these
16. Add LMIC digital health infrastructure survey
17. Add metadata privacy / de-anonymization reference
18. Add PBFT/consensus comparison paper

### Low Priority (P3 — future work):

19. ZKP healthcare privacy papers (when ZKP is implemented)
20. FHIR/HL7 interoperability papers (when FHIR is implemented)
21. Edge computing in health systems (when edge nodes are implemented)

---

## Part 4: References to Verify Before Use

The following are suggested real publications that should be verified before inclusion. None should be added without confirming: authors, exact title, venue, year, volume/issue/pages, DOI.

```
Androulaki, E., et al. (2018). Hyperledger Fabric: A Distributed Operating System for Permissioned Blockchains. EuroSys 2018.

Azaria, A., et al. (2016). MedRec: Using Blockchain for Medical Data Access and Permission Management. 2nd International Conference on Open and Big Data.

Benet, J. (2014). IPFS - Content Addressed, Versioned, P2P File System. arXiv:1407.3561.

Bethencourt, J., Sahai, A., & Waters, B. (2007). Ciphertext-Policy Attribute-Based Encryption. IEEE Symposium on Security and Privacy.

Castro, M., & Liskov, B. (1999). Practical Byzantine Fault Tolerance. OSDI 1999.

Dubovitskaya, A., et al. (2017). Secure and Trustable Electronic Medical Records Sharing Using Blockchain. AMIA Annual Symposium.

Gordon, W.J., & Catalini, C. (2018). Blockchain Technology for Healthcare. Computational and Structural Biotechnology Journal.

Groth, J. (2016). On the Size of Pairing-Based Non-interactive Arguments. EUROCRYPT.

Kirakowski, J., & Corbett, M. (1993). SUMI: The Software Usability Measurement Inventory. British Journal of Educational Technology.

Nunamaker, J.F., Chen, M., & Purdin, T.D.M. (1991). Systems Development in Information Systems Research. Journal of Management Information Systems, 7(3).

Politou, E., et al. (2019). Blockchain Mutability: Challenges and Proposed Solutions. IEEE Transactions on Emerging Topics in Computing.

Zhang, P., et al. (2018). FHIRChain: Applying Blockchain to Securely and Scalably Share Clinical Data. Computational and Structural Biotechnology Journal, 16, 267–278.
```

---

*No references have been fabricated. All candidates above are suggested for verification only. Do not add any entry to references.bib without confirming it against a primary source (publisher website, DOI resolver, or library database).*
