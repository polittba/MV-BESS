# MV-BESS — IT Business and System Requirements

> **Status:** Shell / skeleton. This document is the working location for the
> IT business and system requirements that support the Mountain View Battery
> Energy Storage System (MV-BESS) project. Sections below are placeholders to
> be filled in as requirements are elicited, reviewed, and approved.

Owner: IT Enterprise Architecture
Classification: INTERNAL
Version: 0.1 (Draft — TBD)

---

## 1. Purpose

State, in one or two paragraphs, why this document exists and which decisions
it is intended to support. Make explicit that requirements here are derived
from and traceable to the
[BESS IT & Control System Architecture Vision Document](../docs-MiscRef/BESS_IT-Control%20System%20Architecture%20Vision%20Document.md).

## 2. Scope

- **In scope:** TBD (e.g., IT/OT systems supporting MV-BESS dispatch and
  operate functions, P&I Layer integration, monitoring, cybersecurity,
  asset management interfaces).
- **Out of scope:** TBD (e.g., detailed electrical/protection design,
  market-strategy specifications, project-level engineering specs).

## 3. References

- [BESS IT & Control System Architecture Vision Document](../docs-MiscRef/BESS_IT-Control%20System%20Architecture%20Vision%20Document.md) — anchor document.
- [MV-Baseline-Mark VIe TC](../docs-MVOverallControls/MV-Baseline-Mark%20VIe%20TC.md) — GE Mark VIe TC baseline at MVGS.
- [MV-Baseline-Ovation-BOP](../docs-MVOverallControls/MV-Baseline-Ovation-BOP.md) — Ovation DCS (BoP) baseline at MVGS.
- [MV-100MW-Augmentation](../docs-MVOverallControls/MV-100MW-Augmentation.md) — 100 MW augmentation planning brief.
- [Battery Component Hierarchy](../docs-BESSCore/Battery%20Component%20Hierarchy.MD) — shared vocabulary.
- [Asset Performance Management for ESS](../docs-AssetManagement/Asset%20Performance%20Management%20for%20ESS.MD) — APM strategy.
- [IEC 61850 for ESS — Reference Model](../61850/61850%20For%20ESS%20-%20Reference%20Model.md)
- [MODBUS Challenges](../docs-MiscRef/MODBUS%20Challenges.MD)
- [Vendor Tiers v1.0](../docs-MiscRef/Vendor%20Tiers%20v1.0.md)

## 4. Definitions, Acronyms, and Conventions

- **BESS** — Battery Energy Storage System
- **MVGS / MV** — Mountainview Generation Station / Mountain View site
- **P&I Layer** — SCE Presentation & Integration Layer
- **Dispatch vs. Operate** — see Architecture Vision Document
- *(Add project-specific terms as they are introduced.)*

### Requirement ID convention

- `BR-###` — Business Requirement
- `FR-###` — Functional (System) Requirement
- `NFR-###` — Non-Functional Requirement
- `IR-###` — Interface Requirement
- `SR-###` — Security Requirement
- `DR-###` — Data Requirement

Each requirement should carry: **ID**, **Statement**, **Rationale**,
**Source** (link to anchor doc section), **Priority** (Must / Should / Could),
and **Verification** (Inspection / Demonstration / Test / Analysis).

## 5. Stakeholders

| Role | Group | Interest |
| --- | --- | --- |
| Operations | TBD | TBD |
| Engineering / Controls | TBD | TBD |
| IT / OT | TBD | TBD |
| Cybersecurity | TBD | TBD |
| Asset Management | TBD | TBD |
| Vendors | TBD | TBD |

## 6. Business Requirements (BR)

> High-level business outcomes the IT/control systems must enable. Tied back
> to the dispatch/operate model and SCE’s fleet-level objectives.

| ID | Statement | Source | Priority |
| --- | --- | --- | --- |
| BR-001 | *TBD* | *TBD* | *TBD* |

## 7. Functional / System Requirements (FR)

> What the system must **do**. Organize by functional area as the requirements
> mature.

### 7.1 Dispatch (external intent)

| ID | Statement | Source | Priority | Verification |
| --- | --- | --- | --- | --- |
| FR-001 | *TBD* | *TBD* | *TBD* | *TBD* |

### 7.2 Operate (BESS execution and safety)

| ID | Statement | Source | Priority | Verification |
| --- | --- | --- | --- | --- |
| FR-101 | *TBD* | *TBD* | *TBD* | *TBD* |

### 7.3 Monitoring, Alarming, and Historization

*(Anchor to the Alarm Philosophy and APM docs.)*

### 7.4 Asset Performance Management Integration

*(Anchor to the APM doc — SoH/SoC, degradation, KPIs, warranty/compliance.)*

## 8. Non-Functional Requirements (NFR)

> Performance, availability, scalability, maintainability, usability, etc.

| ID | Category | Statement | Target / Metric | Priority |
| --- | --- | --- | --- | --- |
| NFR-001 | *TBD* | *TBD* | *TBD* | *TBD* |

## 9. Interface Requirements (IR)

> External and internal interfaces. Include protocol, direction, latency,
> reliability, and ownership for each.

| ID | Interface | Protocol | Direction | Notes |
| --- | --- | --- | --- | --- |
| IR-001 | BESS ↔ P&I Layer | *TBD* | *TBD* | *TBD* |
| IR-002 | P&I Layer ↔ DERMS / ADMS / GenMS / Market | *TBD* | *TBD* | *TBD* |
| IR-003 | BESS ↔ MVGS GE Mark VIe TC | *TBD* | *TBD* | *TBD* |
| IR-004 | BESS ↔ MVGS Ovation BoP | *TBD* | *TBD* | *TBD* |

## 10. Security Requirements (SR)

> Cybersecurity expectations for SCE-owned and vendor-supplied components.
> Anchor to the Architecture Vision Document's stance that all networking —
> SCE-provided or vendor-supplied — is part of the logical SCE grid
> environment and must meet minimum monitoring, management, and cybersecurity
> expectations.

| ID | Statement | Standard / Control | Priority |
| --- | --- | --- | --- |
| SR-001 | *TBD* | *TBD* | *TBD* |

## 11. Data Requirements (DR)

> Data points, sampling rates, retention, quality, and lineage.

| ID | Data Item | Source | Rate | Retention | Notes |
| --- | --- | --- | --- | --- | --- |
| DR-001 | *TBD* | *TBD* | *TBD* | *TBD* | *TBD* |

## 12. Assumptions, Constraints, and Dependencies

- **Assumptions:** TBD
- **Constraints:** TBD
- **Dependencies:** TBD

## 13. Open Issues / Decisions Pending

| # | Topic | Owner | Target Date | Status |
| --- | --- | --- | --- | --- |
| 1 | *TBD* | *TBD* | *TBD* | Open |

## 14. Traceability Matrix

> Map each requirement to its source (Architecture Vision Document section,
> baseline doc, regulatory driver, etc.) and to its verification artifact.

| Requirement ID | Source | Verification Artifact | Status |
| --- | --- | --- | --- |
| *TBD* | *TBD* | *TBD* | *TBD* |

## 15. Revision History

| Version | Date | Author | Changes |
| --- | --- | --- | --- |
| 0.1 | TBD | TBD | Initial shell document |
