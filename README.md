> **Zenodo DOI:** [10.5281/zenodo.21379780](https://doi.org/10.5281/zenodo.21379780) — Published 2026-07-15

# PP-SPEC-010 · Legal Attestation Format

**Document ID:** PP-SPEC-010  
**Version:** 0.1 - Draft  
**Status:** Draft  
**License:** CC BY 4.0  
**Maintained by:** Proof Economy™ Standards Alliance (PESA)  
**Repository:** https://github.com/proofprotocol/legal-proof-format  
**Published:** 2026-07-13  

---

## Abstract

This specification defines the Legal Attestation Format: the structured package by which a ProofBundle™ and its associated ProofRegister™ record are assembled for submission to regulators, auditors, legal proceedings, and compliance frameworks.

A ProofBundle™ is a technical artifact. A Legal Attestation Package is the same artifact formatted and contextualized for non-technical consumers - regulators, counsel, auditors, boards - who need to act on it without running a verifier binary.

---

## Status of This Document

Draft. Subject to change before v1.0.

---

## Table of Contents

1. [Motivation](#1-motivation)
2. [Legal Attestation Package Structure](#2-legal-attestation-package-structure)
3. [Cover Document](#3-cover-document)
4. [Regulatory Mappings](#4-regulatory-mappings)
5. [Chain of Custody](#5-chain-of-custody)
6. [Submission Formats](#6-submission-formats)
7. [Conformance](#7-conformance)
8. [Authors](#8-authors)

---

## 1. Motivation

Cryptographic proof is technically sound but legally inert without context. A regulator receiving a JSONL file of Ed25519-signed receipts cannot act on it without understanding what it proves, how it was produced, who witnessed it, and what standards it satisfies.

The Legal Attestation Format bridges the technical and legal layers. It wraps a conformant ProofBundle™ in a structured package that answers the questions a non-technical decision-maker needs answered:

- What was tested?
- Who ran the test?
- Who witnessed it?
- What did it prove?
- How can I verify it independently?
- Which regulations does it satisfy?

---

## 2. Legal Attestation Package Structure

```
legal-attestation/
  cover.pdf              # Human-readable cover document
  packet.json            # ProofBundle™ envelope
  evidence.jsonl         # Signed receipt chain
  verifier.txt           # Verifier output
  nist-pulse.json        # NIST Beacon anchors
  witness.json           # Witness attestation
  proofregister.json     # ProofRegister™ anchor record
  proofstamp.json        # ProofStamp™ token (if certified)
  regulatory-mapping.json # Framework compliance mapping
  chain-of-custody.json  # Submission and handling record
```

---

## 3. Cover Document

The cover document is a human-readable PDF that summarizes:

- Product or agent name and version
- Benchmark name and date
- PES score and key metrics
- Witness identity and affiliation
- ProofRegister™ Record ID and URI
- ProofStamp™ ID and URI (if certified)
- NIST Beacon pulse references
- Verification instructions in plain language
- Regulatory framework mappings
- Contact information for HACKERverse

The cover document is not the proof. The evidence.jsonl and verifier.txt are the proof. The cover document is the translation layer for non-technical consumers.

---

## 4. Regulatory Mappings

```json
{
  "frameworks": [
    {
      "name": "NIST AI RMF",
      "version": "1.0",
      "controls_satisfied": ["GOVERN 1.1", "MAP 1.1", "MEASURE 2.5"],
      "evidence_references": ["packet.json", "witness.json"]
    },
    {
      "name": "EU AI Act",
      "article": "Article 9 - Risk Management",
      "evidence_references": ["packet.json", "proofregister.json"]
    },
    {
      "name": "SOC 2",
      "trust_service_criteria": ["CC7.1", "CC7.2"],
      "evidence_references": ["evidence.jsonl", "verifier.txt"]
    },
    {
      "name": "HIPAA",
      "safeguard": "Technical Safeguards - Access Control",
      "evidence_references": ["packet.json"]
    }
  ]
}
```

Regulatory mappings are informational. They document which framework controls the ProofBundle™ evidence supports. They do not constitute legal advice or formal compliance certification.

---

## 5. Chain of Custody

```json
{
  "chain_of_custody_version": "1.0",
  "proof_record_id": "PR-YYYY-NNNNN",
  "events": [
    {
      "event": "bundle_assembled",
      "timestamp": "RFC 3339 UTC",
      "actor": "string",
      "nist_pulse_index": 0
    },
    {
      "event": "submitted_to_proofregister",
      "timestamp": "RFC 3339 UTC",
      "actor": "HACKERverse ProofRegister™ API",
      "anchor_pulse_index": 0
    },
    {
      "event": "proofstamp_issued",
      "timestamp": "RFC 3339 UTC",
      "actor": "HACKERverse",
      "proofstamp_id": "PS-YYYY-NNNNN"
    },
    {
      "event": "legal_package_assembled",
      "timestamp": "RFC 3339 UTC",
      "actor": "string",
      "nist_pulse_index": 0
    }
  ]
}
```

---

## 6. Submission Formats

The Legal Attestation Package may be submitted as:

- A zip archive containing all files
- A PDF portfolio embedding all files as attachments
- Direct API submission to a regulator's evidence portal where supported

ProofRegister™ provides a Legal Attestation Package download endpoint:

```
GET https://proofregister.com/api/v1/records/:id/legal-package
```

Returns a zip archive conformant with this specification.

---

## 7. Conformance

A Legal Attestation Package is conformant with this specification if:

- It contains all required files from Section 2
- The underlying ProofBundle™ is conformant with PP-SPEC-003
- The cover document addresses all items in Section 3
- The chain of custody records all events from bundle assembly through package delivery

---

## 8. Authors

Craig Ellrod, Founder & CEO, Nebulonium, Inc. (d/b/a HACKERverse)  
Castle Rock, Colorado  
2026-07-13

---

*CC BY 4.0 - Attribution to Craig Ellrod / Nebulonium, Inc. required.*
