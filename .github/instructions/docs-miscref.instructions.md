---
applyTo: "docs-MiscRef/**/*.md"
description: "Editing rules for reference material under docs-MiscRef/"
---

# Rules for `docs-MiscRef/` Markdown files

These files are **reference material** describing vendor specs, protocol
challenges, and architectural context. Treat them with care.

## General

- Treat content as **reference-only**. Do not rewrite, restructure, or
  "improve" prose unless the user explicitly asks.
- Preserve existing heading hierarchy, numbering, and section order.
- Preserve vendor names, product names, and acronyms **verbatim**
  (e.g., MODBUS, IEC 61850, Tier 1).

## Media

- Image and diagram assets live in `docs-MiscRef/media/`.
- Reference assets with relative paths; do not duplicate or move them.
- When adding new images, place them in `docs-MiscRef/media/` and use
  Markdown image syntax: `![alt text](media/filename.png)`.

## Edits

- For typo fixes or small corrections, make the minimal change.
- For substantive edits, surface a plan first and wait for confirmation.
- Never delete sections without explicit approval.

## Citations

- When citing these docs elsewhere, use workspace-relative Markdown links,
  e.g. `[MODBUS Challenges](docs-MiscRef/MODBUS%20Challenges.MD)`.

## Project context (MV-BESS)

When **using** these documents as reference (for analysis, summaries,
or generating derived content elsewhere in the workspace):

- Treat the contents as **vendor- and utility-neutral reference material**.
- **Platform assignment for MV-BESS:** The
  **Site Control & Monitoring (Native BESS Control System)** — as defined
  in §6.2 of
  [BESS_IT-Control System Architecture Vision Document.md](docs-MiscRef/BESS_IT-Control%20System%20Architecture%20Vision%20Document.md)
  — **is Emerson Ovation** for the MV-BESS project. Apply the §6.2
  requirements (SCM-01 through SCM-18) to Ovation in this role.
- **Ignore use-case specifics in the source documents that frame Ovation
  as the Presentation & Integration (P&I) Layer.** That framing is
  SCE-specific and does not apply to MV-BESS. In MV-BESS, Ovation occupies
  the Operate layer (§6.2), not the P&I layer (§6.1).
- Apply all other concepts (P&I Layer responsibilities, control authority
  boundaries, interface contracts, etc.) generically. The MV-BESS P&I
  Layer **may or may not be Ovation** — this depends on the chosen
  topology and is not fixed for the project. Do not assume Ovation
  fills the P&I role unless the topology under discussion explicitly
  says so.
- If a question can only be answered by relying on the Ovation-as-P&I
  specifics, surface that fact and ask how the P&I Layer should be
  treated for MV-BESS before proceeding.

> Note: the editing rules above still apply to the source documents
> themselves — do **not** strip or rewrite the SCE/Ovation content in
> `docs-MiscRef/` to enforce this. Only apply these mappings when *using*
> the docs.
