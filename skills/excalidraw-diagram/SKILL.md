---
name: excalidraw-diagram
description: Create Excalidraw diagram JSON files that make visual arguments. Use when the user wants to visualize workflows, architectures, or concepts as beautiful, structured diagrams.
---

# Excalidraw Diagram Creator

Generate `.excalidraw` JSON files that **argue visually** — shapes that mirror their meaning, not just labelled boxes.

**Two tests every diagram must pass:**
- **Isomorphism**: remove all text — does the structure still communicate the concept?
- **Education**: does it teach concrete details, or just label things?

---

## Step 0 — Assess Depth

- **Simple/Conceptual**: abstract shapes and labels (mental models, overviews)
- **Comprehensive/Technical**: real data formats, actual API/event names, code snippets

For technical diagrams: **research the actual specs first** (real event names, JSON formats, method names — not generic placeholders).

## Step 1 — Map Concepts to Patterns

See `references/diagram-patterns.md` for the full pattern library (fan-out, convergence, timeline, assembly line, etc.) and evidence artifact types.

## Step 2 — Plan Layout

Before writing JSON, sketch mentally:
- Entry and exit points
- Major sections (assign non-overlapping coordinate regions, e.g. Section A: x 0–800, Section B: x 900–1700)
- Where evidence artifacts live
- Arrow flow and reading direction (top→bottom or left→right)

## Step 3 — Generate JSON

**Wrapper:**
```json
{
  "type": "excalidraw",
  "version": 2,
  "source": "https://excalidraw.com",
  "elements": [...],
  "appState": { "viewBackgroundColor": "#ffffff", "gridSize": null },
  "files": {}
}
```

- Element templates → `references/element-templates.md`
- JSON schema & sizing → `references/json-schema.md`
- Colors → `references/color-palette.md` (single source of truth)
- Use meaningful IDs (`auth-box`, `api-arrow-1`), never `elem1`
- For large diagrams: build section-by-section, not all at once

**Key defaults:** `roughness: 0`, `opacity: 100`, `fontFamily: 3` (monospace) on all elements.

## Step 4 — Render

```bash
cd .claude/skills/excalidraw-diagram/references && uv run python render_excalidraw.py <file.excalidraw>
```

First-time setup: `uv sync && uv run playwright install chromium`

## Step 5 — Validate & Iterate

Check the PNG against `references/checklist.md`. Fix issues and re-render. Expect 2–4 iterations.
