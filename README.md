# VPAT Generator — Chrome Extension

A Chrome extension that scans web pages for WCAG 2.2 accessibility issues and auto-drafts a VPAT® 2.5 Accessibility Conformance Report (ACR) in `.docx` or `.pdf` format.

Built as a tool for accessibility practitioners and procurement reviewers who need defensible, audit-ready documentation without manually compiling results from multiple scanners.

---

## What it does

- Runs three scanning engines in parallel: **axe-core 4.14.0**, **IBM Equal Access 4.0.34**, and **HTML CodeSniffer 2.5.1**
- Simulates keyboard navigation to detect focus traps and missing skip links
- Maps violations to WCAG 2.2 Level A and AA success criteria
- Applies conformance threshold logic automatically:
  - 0 violations → **Supports**
  - 1–4 violations → **Partially Supports**
  - 5+ violations → **Does Not Support**
  - 2.4.1 (bypass blocks) → binary **Does Not Support**
- Generates a structured VPAT 2.5Rev document with per-criterion findings, violation counts, and affected URLs
- Exports to `.docx` (editable) or `.pdf`

---

## Why three engines

No single scanner catches everything. axe-core, IBM Equal Access, and HTML CodeSniffer each have different rule sets and heuristics. Running all three and deduplicating gives broader coverage and reduces false negatives — especially important when producing documentation that may be reviewed by a client's procurement or legal team.

---

## Tech

| Layer | Stack |
|---|---|
| Extension platform | Chrome Manifest V3, Side Panel API |
| Scanning | axe-core 4.14.0, IBM Equal Access 4.0.34, HTML CodeSniffer 2.5.1 |
| Keyboard simulation | Custom content script |
| Document generation | docx.js, jsPDF |
| State persistence | `chrome.storage.local` |

---

## Install (developer mode)

1. Clone or download this repo
2. Open Chrome → `chrome://extensions`
3. Enable **Developer mode** (top right toggle)
4. Click **Load unpacked** → select the `dist/chrome` folder
5. Click the extension icon to open the side panel

---

## Usage

1. Open the side panel on any web page
2. Enter a product name (required — appears in the VPAT header)
3. Click **Scan Page** — results appear per WCAG criterion
4. Scan additional pages as needed (each adds to the report)
5. Click **Generate VPAT** → choose `.docx` or `.pdf`

---

## Sample output

- [`hint-astrology-app-VPAT-2026-10-14.pdf`](./samples/hint-astrology-app-VPAT-2026-10-14.pdf) — scan of Hint Astrology App
- [`vanta-web-VPAT-2026-10-14.pdf`](./samples/vanta-web-VPAT-2026-10-14.pdf) — scan of Vanta web app

---

## Status

Version 0.5.0 — functional, actively developed. Known issues:

- **Session bleed**: accumulated scan data persists if the extension is not reset between unrelated scans. Reset via the domain-change dialog when scanning a new product.

---

## Background

Built to solve a real workflow problem: accessibility consultants and in-house teams spend hours manually correlating scanner output into VPAT format. This extension automates the structural work so practitioners can focus on remediation guidance and edge cases that automated scanners miss.

The conformance thresholds are calibrated for enterprise SaaS review contexts, where a "Partially Supports" rating with documented violations is more credible than an unsupported "Supports."

---

## License

MIT. Third-party library licenses: see [THIRD-PARTY-NOTICES.md](./THIRD-PARTY-NOTICES.md).
