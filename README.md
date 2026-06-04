# Strain Lineage Viewer

A browser-based tool for exploring yeast strain lineages, genomic integrations, and construct searches. Built for the Erg Bio EBY/EBB strain collection. No server required — runs entirely on GitHub Pages.

---

## Features

| Feature | Description |
|---|---|
| **Lineage chart** | Interactive D3.js tree showing ancestor–descendant relationships back to EBY11. Pan, zoom, click nodes for details. |
| **Strain info panel** | Clicking any node shows description, parent, uncured plasmid, notes, and all cumulative integrations across the full lineage. |
| **Search by EBB** | Enter one or more EBB construct names to find every EBY strain that carries them (anywhere in its lineage). Results split into complete matches (all queried EBBs present) and partial matches. |
| **Client-side only** | CSV files are parsed in your browser. Nothing leaves your machine. |

---

## Setup

### 1 — Fork / clone

```bash
git clone https://github.com/<your-org>/strain-lineage-viewer.git
cd strain-lineage-viewer
```

### 2 — Files

The entire app is a single `index.html`. No build step, no dependencies to install.

```
strain-lineage-viewer/
└── index.html          # entire app
└── README.md
```

### 3 — Enable GitHub Pages

Go to **Settings → Pages → Source** and set it to the `main` branch, root (`/`). The app will be live at:

```
https://<your-org>.github.io/strain-lineage-viewer/
```

---

## Usage

### Upload CSVs

The app accepts two CSV files, uploaded each session via drag-and-drop or the file picker:

**EBY yeast file** — must contain these columns (extras are ignored):

| Column | Required | Notes |
|---|---|---|
| `Erg Bio name` | ✓ | `EBY###` format |
| `Parent Strain` | ✓ | `EBY###` or blank for root |
| `Transformed donor; locus` | ✓ | e.g. `EBB161; @FUI1` |
| `Description` | ✓ | free text |
| `Last uncured plasmid present` | ✓ | EBB name or blank |
| `Notes` | ✓ | free text |

**EBB bacteria file** — must contain:

| Column | Required |
|---|---|
| `Erg Bio name` | ✓ |
| `Description` | ✓ |

> Column order doesn't matter. Extra columns are ignored. UTF-8 encoding recommended.

### Lineage mode

1. Select the **Lineage** tab
2. Enter one or more strain names, comma-separated: `EBY112, EBY88, Y45`
3. Click **Show lineage**

The chart renders a left-to-right tree. All ancestor strains back to EBY11 are shown automatically. Click any node to open the detail panel showing integrations, plasmids, and notes.

**Input format**: accepts `EBY###`, `Y###` (any case). Internally normalised to `EBY###`.

### Search by EBB mode

1. Select the **Search by EBB** tab
2. Enter one or more EBB names: `EBB161, EBB224`
3. Click **Search**

Results are sorted by strain number (descending, newest first) and split into:
- **Complete matches** — strain carries *all* queried EBBs somewhere in its lineage
- **Partial matches** — strain carries at least one (only shown when searching multiple EBBs)

Each result card can be expanded to show the full lineage steps with integrations highlighted. Click **View lineage chart** to jump to the chart for that strain.

---

## Data notes

- EBY11 is the universal root ancestor. Lineage traversal stops when no parent is found (or EBY11 is reached).
- A strain's "integrations" includes only constructs recorded in the `Transformed donor; locus` column. EBB numbers are parsed from that field; locus is parsed from `@LOCUS` patterns.
- The `Last uncured plasmid present` field is displayed but not used for lineage/search logic.

---

## Updating the CSV files

The CSVs are uploaded fresh each session — just drag and drop the latest export from your strain tracking spreadsheet. No code changes needed when new strains are added.

---

## Browser support

Modern browsers (Chrome, Firefox, Safari, Edge). Requires JavaScript enabled. No internet connection required after initial page load (fonts load from Google Fonts on first visit; subsequent visits use cache).

---

## Local development

Since it's a single HTML file, open directly:

```bash
open index.html
# or
python3 -m http.server 8000   # then visit http://localhost:8000
```

Note: file:// drag-and-drop works in most browsers, but a local server avoids any CORS edge cases.

---

## Tech stack

| Library | Version | Purpose |
|---|---|---|
| [D3.js](https://d3js.org) | 7.9 | Tree layout and SVG rendering |
| [PapaParse](https://www.papaparse.com) | 5.4 | CSV parsing |

Both loaded from cdnjs — no npm, no bundler.

---

## Migrated from

Flask app originally hosted on PythonAnywhere (`flask_app.py` + Jinja templates). All parsing and lineage logic has been ported to vanilla JavaScript. The Graphviz/matplotlib chart has been replaced with a D3 interactive tree.
