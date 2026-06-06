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

<!-- intentionally empty — to be filled in -->
