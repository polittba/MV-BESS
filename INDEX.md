# MV-BESS — Reading Priority Index

> Purpose: a single, ordered entry point for humans (and AI assistants) into the
> MV-BESS knowledge base. Items are grouped from **most foundational** to
> **most project- or reference-specific**. Inside each tier, read top-to-bottom.
>
> This index is curated. If a document does not appear here, treat it as
> supporting material. Delete or reorder any entry below that you disagree with.

---

## Tier 0 — Start Here (anchor document)

1. [docs-MiscRef/BESS_IT-Control System Architecture Vision Document.md](docs-MiscRef/BESS_IT-Control%20System%20Architecture%20Vision%20Document.md)
   The enterprise-level architecture vision. Establishes the **dispatch vs. operate**
   two-layer control concept, the SCE Presentation & Integration (P&I) Layer, and
   the vendor-neutral functional model. Every other document in this repo should
   be read in the context of this one.

## Tier 1 — Mountainview Site Baselines and Project Briefs

2. [docs-MVOverallControls/MV-Baseline-Mark VIe TC.md](docs-MVOverallControls/MV-Baseline-Mark%20VIe%20TC.md)
   As-built baseline of the GE Mark VIe Turbine Control system at MVGS.
3. [docs-MVOverallControls/MV-Baseline-Ovation-BOP.md](docs-MVOverallControls/MV-Baseline-Ovation-BOP.md)
   As-built baseline of the Emerson Ovation DCS (Balance of Plant) at MVGS.
4. [docs-MVOverallControls/MV-100MW-Augmentation.md](docs-MVOverallControls/MV-100MW-Augmentation.md)
   Planning brief for the 100 MW battery augmentation at the Mountain View
   site, anchored to the Architecture Vision Document.

## Tier 2 — Core BESS Concepts (read before any vendor work)

5. [docs-BESSCore/Battery Component Hierarchy.MD](docs-BESSCore/Battery%20Component%20Hierarchy.MD)
   Physical and logical hierarchy of a reference 1 MW / 4 MWh BESS. Defines the
   shared vocabulary (PCS, BMS, rack, module, cell, etc.) used everywhere else.
6. [docs-BESSCore/Battery Component Hierarchy - Alarm Philosophy.MD](docs-BESSCore/Battery%20Component%20Hierarchy%20-%20Alarm%20Philosophy.MD)
   Alarm philosophy mapped onto the component hierarchy — overview.
7. [docs-BESSCore/Battery Component Hierarchy - Alarm Philosophy - Detail.MD](docs-BESSCore/Battery%20Component%20Hierarchy%20-%20Alarm%20Philosophy%20-%20Detail.MD)
   Same philosophy at the working-level of detail. Read after the overview.
8. [docs-AssetManagement/Asset Performance Management for ESS.MD](docs-AssetManagement/Asset%20Performance%20Management%20for%20ESS.MD)
   APM strategy for a scaled, multi-site, multi-vendor ESS fleet (SoH/SoC,
   degradation, thermal, KPIs, data architecture, end-of-life).

## Tier 3 — Protocols, Standards, and Vendor Landscape

9. [61850/61850 For ESS - Reference Model.md](61850/61850%20For%20ESS%20-%20Reference%20Model.md)
   IEC 61850 reference model for large-scale ESS. Separates **process control**
   from **electrical protection** and explains the MMS / GOOSE / SV split.
10. [docs-MiscRef/MODBUS Challenges.MD](docs-MiscRef/MODBUS%20Challenges.MD)
    Why Modbus is fine at the device layer but the wrong choice as the
    aggregation backbone for a multi-block BESS. Companion to the 61850 piece.
11. [docs-MiscRef/Vendor Tiers v1.0.md](docs-MiscRef/Vendor%20Tiers%20v1.0.md)
    Unified tiering model (Tier 1–4 × Type A–E) for BESS integrators and EMS
    platforms. Used when evaluating vendors against the architecture vision.

## Tier 4 — Diagrams and Media (visual reference)

- [img-Architecture/MVGS_Baseline Architecture_GE_V1.png](img-Architecture/MVGS_Baseline%20Architecture_GE_V1.png) — source for the Mark VIe TC baseline.
- [img-Architecture/MVGS_Baseline Architecture_Ovation_V1.png](img-Architecture/MVGS_Baseline%20Architecture_Ovation_V1.png) — source for the Ovation BoP baseline.
- [img-Architecture/MV-100MW-Augmentation.drawio](img-Architecture/MV-100MW-Augmentation.drawio) / [.svg](img-Architecture/MV-100MW-Augmentation.svg) — editable + exported view of the 100 MW augmentation.
- [img-Architecture/MV-BESS High Level Design.png](img-Architecture/MV-BESS%20High%20Level%20Design.png)
- [img-Architecture/MV-BESS Elec Connectivity View.png](img-Architecture/MV-BESS%20Elec%20Connectivity%20View.png)

## Tier 5 — Working Areas and Source Material (not yet primary reading)

- [docs-Requirements/](docs-Requirements/) — placeholder for IT business and
  system requirements derived from the documents above. **Currently empty.**
- [docs-Word/](docs-Word/) — original `.docx` source for the Architecture
  Vision Document. The Markdown version in `docs-MiscRef/` is authoritative;
  the Word file is kept for traceability only.

---

## How to use this index

- **Humans:** read Tier 0 → Tier 1 in order. Branch into Tier 2 or Tier 3 as
  the topic demands.
- **AI assistants (Copilot):** when answering architecture questions, prefer
  Tier 0 first; cite Tier 1 for Mountainview-specific questions; cite Tier 2
  for general BESS concepts and alarm/APM topics; cite Tier 3 for protocol
  or vendor questions.
- **Maintainers:** when adding a new document, place it in the appropriate
  tier and renumber. If a document spans tiers, list it in the highest
  (lowest-numbered) tier it belongs to and cross-reference from the others.
