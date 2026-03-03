# Diagram Patterns & Evidence Artifacts

## Concept → Visual Pattern

| If the concept... | Use this pattern |
|-------------------|-----------------|
| Spawns multiple outputs | **Fan-out** (radial arrows from center) |
| Combines inputs into one | **Convergence** (funnel, arrows merging) |
| Has hierarchy/nesting | **Tree** (lines + free-floating text) |
| Is a sequence of steps | **Timeline** (line + dots + labels) |
| Loops or improves continuously | **Spiral/Cycle** (arrow returning to start) |
| Is an abstract state or context | **Cloud** (overlapping ellipses) |
| Transforms input to output | **Assembly line** (before → process → after) |
| Compares two things | **Side-by-side** |
| Separates into phases | **Gap/Break** |

## Multi-Zoom Architecture

Build comprehensive diagrams at three levels simultaneously:

1. **Summary flow** — simplified overview of the full pipeline at a glance
2. **Section boundaries** — labeled regions grouping related components
3. **Detail inside sections** — evidence artifacts, code snippets, real data

## Evidence Artifact Types

| Artifact | When to Use | How to Render |
|----------|-------------|---------------|
| Code snippets | APIs, integrations, implementation | Dark rect + syntax-colored text |
| JSON/data examples | Data formats, schemas, payloads | Dark rect + green text (see `color-palette.md`) |
| Event/step sequences | Protocols, workflows, lifecycles | Timeline pattern |
| UI mockups | Showing actual output | Nested rectangles |
| API/method names | Real function calls, endpoints | Use actual names from docs |

**Key principle**: show what things *actually look like*, not just what they're called.

## Container vs. Free-Floating Text

Default to free-floating text. Add a container only when:
- Arrows need to connect to it
- It represents a distinct "thing" in the system
- Visual grouping is needed

Use font size, weight, and color for hierarchy — a 28px title doesn't need a rectangle.

## Simple vs. Comprehensive

| Simple | Comprehensive |
|--------|--------------|
| Abstract labels ("Input → Process → Output") | Shows what input/output actually looks like |
| Named boxes only | Named boxes + real request/response examples |
| ~30s to explain | ~2–3 min of teaching content |

Use simple for mental models and overviews. Use comprehensive for architectures, tutorials, and real systems.
