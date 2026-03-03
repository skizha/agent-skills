# Excalidraw JSON Schema

## Element Types

| Type | Use For |
|------|---------|
| `rectangle` | Processes, components, section containers, evidence artifacts |
| `ellipse` | Entry/exit points, external systems, marker dots |
| `diamond` | Decisions, conditionals |
| `arrow` | Directed connections between shapes |
| `text` | Labels — free-floating or inside shapes |
| `line` | Structural connections (timelines, tree lines) |

## Always-Same Defaults

Set these on every element and never vary them:

```
fillStyle: "solid"    roughness: 0    opacity: 100
angle: 0              version: 1      isDeleted: false
groupIds: []          link: null      locked: false
```

Use any integer for `seed` and `versionNonce`.
**Exception**: Section containers use `opacity: 30`.

## Variable Properties

| Property | Notes |
|----------|-------|
| `id` | Meaningful string — `auth-box`, `api-arrow`. Never `elem1` |
| `x`, `y`, `width`, `height` | Integers. Plan coordinates before writing JSON |
| `strokeColor`, `backgroundColor` | Always from `color-palette.md` |
| `strokeWidth` | `2` for shapes, `1` for text and evidence artifacts |
| `strokeStyle` | `"solid"` default; `"dashed"` for section containers |
| `roundness` | `{"type": 3}` for rounded corners on boxes |

## Text Properties

| Property | Value |
|----------|-------|
| `fontFamily` | Always `3` (monospace) |
| `lineHeight` | Always `1.25` |
| `containerId` | Parent shape ID, or `null` for free-floating |
| `textAlign` | `"center"` inside shapes, `"left"` for free-floating |
| `verticalAlign` | `"middle"` inside shapes, `"top"` for free-floating |

## Arrow Properties

| Property | Value |
|----------|-------|
| `points` | `[[0,0],[dx,dy]]` — relative to arrow origin. Add midpoint for curves |
| `startBinding` / `endBinding` | `{"elementId": "id", "focus": 0, "gap": 2}` |
| `startArrowhead` | `null` (no tail) or `"arrow"`, `"bar"`, `"dot"`, `"triangle"` |
| `endArrowhead` | Usually `"arrow"` |

## BoundElements

Shapes must declare their bound text and arrows:
```json
"boundElements": [
  {"id": "my-label", "type": "text"},
  {"id": "my-arrow", "type": "arrow"}
]
```
Text elements always set `"boundElements": null`.

## Coordinate System

- `(0, 0)` is top-left. X → right, Y → down.
- Allocate non-overlapping regions per section with 100px buffers:
  Section A: x `0`–`800` | Section B: x `900`–`1700` | Section C: x `1800`–`2600`

## Typography Scale

| Context | fontSize |
|---------|----------|
| Diagram title | `32` |
| Section header | `24`–`28` |
| Component label | `16`–`20` |
| Annotation / detail | `13`–`16` |
| Evidence artifact | `12`–`13` |

## Sizing Guidelines

| Element | Width | Height |
|---------|-------|--------|
| Section container | `600`–`1000` | `400`–`800` |
| Process / component box | `160`–`240` | `60`–`100` |
| Evidence artifact | `200`–`400` | `100`–`250` |
| Decision diamond | `160`–`200` | `90`–`120` |
| Entry / exit ellipse | `120`–`160` | `60`–`80` |
| Marker dot | `12` | `12` |
