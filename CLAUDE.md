# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A single-page web application that generates DOCX referral forms for Gynae Oncology (GONC) Multidisciplinary Meetings (MDM) at National Women's Health (NZ). Users paste structured text in Heidi format; the app parses it, shows a live preview, and exports a Word document.

## Running the App

No build step required. Open `index.html` directly in a browser or serve it via any HTTP server:

```sh
python3 -m http.server 8080
# or
npx serve .
```

There are no automated tests, no linting config, and no package manager.

## Architecture

The entire application lives in **`index.html`** (~961 lines, ~33KB) with three embedded sections:

1. **CSS** (lines ~1–340) — design system with CSS custom properties (`--accent`, `--bg`, etc.), responsive at 860px breakpoint
2. **HTML** (lines ~341–449) — layout with two sidebars (stats + field preview) and a textarea input
3. **JavaScript** (lines ~450–960) — all application logic

### JavaScript Structure

| Section | Location | Purpose |
|---|---|---|
| Field definitions | `CORE_FIELDS`, `RAD_FIELDS` arrays | 27 core fields + 4 fields × 5 radiology entries |
| `parseHeidi()` | ~464 | Regex-based parser for `KEY: value` format |
| `updatePreview()` | ~487 | Real-time validation, stats, field sidebar |
| `generateDocx()` | ~569 | Builds Office Open XML and downloads via JSZip |
| XML builder fns | ~785 | Pure functions returning OOXML string fragments |
| Sample data | ~889 | Example endometrial cancer case |

### Input Format (Heidi)

```
PATIENT_NAME: Jane Smith
NHI_NUMBER: ABC1234
DIAGNOSIS: Endometrial adenocarcinoma
```

Keys must match entries in `CORE_FIELDS` or radiology fields (`RAD1_TYPE`, `RAD2_DATE`, etc.).

### DOCX Generation

Uses `jszip@3.10.1` (CDN) for ZIP assembly. OOXML is built by string concatenation — no template library. The generated `.docx` filename is derived from patient name and referral date.

### External Dependencies (CDN only)

- `jszip@3.10.1` — ZIP/DOCX assembly
- Google Fonts — typography

## Code Conventions

- **Constants:** `UPPERCASE_WITH_UNDERSCORES`
- **Functions:** `camelCase`
- **CSS classes:** `kebab-case`
- **Section separators:** `// ─── Section Name ───`
- Functional style throughout; no classes
