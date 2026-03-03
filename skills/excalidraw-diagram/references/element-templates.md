# Element Templates

## Fixed Fields (apply to every element — never change these)

```json
"fillStyle": "solid",
"roughness": 0,
"opacity": 100,
"angle": 0,
"version": 1,
"versionNonce": 12345,
"isDeleted": false,
"groupIds": [],
"link": null,
"locked": false
```

Use any integer for `seed` and `versionNonce`. Only the fields shown in each template below vary.

---

## Free-Floating Text

```json
{
  "type": "text",
  "id": "section-title",
  "x": 100, "y": 100,
  "width": 200, "height": 25,
  "text": "Section Title",
  "originalText": "Section Title",
  "fontSize": 20,
  "fontFamily": 3,
  "textAlign": "left",
  "verticalAlign": "top",
  "strokeColor": "<title color from color-palette.md>",
  "backgroundColor": "transparent",
  "strokeWidth": 1,
  "strokeStyle": "solid",
  "containerId": null,
  "lineHeight": 1.25,
  "boundElements": null
}
```

## Rectangle (process / component box)

```json
{
  "type": "rectangle",
  "id": "process-name",
  "x": 100, "y": 100, "width": 180, "height": 90,
  "strokeColor": "<stroke from color-palette.md>",
  "backgroundColor": "<fill from color-palette.md>",
  "strokeWidth": 2,
  "strokeStyle": "solid",
  "roundness": {"type": 3},
  "boundElements": [{"id": "process-name-label", "type": "text"}]
}
```

## Text Inside Shape

```json
{
  "type": "text",
  "id": "process-name-label",
  "x": 130, "y": 132,
  "width": 120, "height": 25,
  "text": "Process",
  "originalText": "Process",
  "fontSize": 16,
  "fontFamily": 3,
  "textAlign": "center",
  "verticalAlign": "middle",
  "strokeColor": "<on light/dark fills color from color-palette.md>",
  "backgroundColor": "transparent",
  "strokeWidth": 1,
  "strokeStyle": "solid",
  "containerId": "process-name",
  "lineHeight": 1.25,
  "boundElements": null
}
```

## Ellipse (entry/exit point)

```json
{
  "type": "ellipse",
  "id": "start-node",
  "x": 100, "y": 100, "width": 160, "height": 80,
  "strokeColor": "<stroke from color-palette.md>",
  "backgroundColor": "<fill from color-palette.md>",
  "strokeWidth": 2,
  "strokeStyle": "solid",
  "boundElements": [{"id": "start-node-label", "type": "text"}]
}
```

## Diamond (decision)

```json
{
  "type": "diamond",
  "id": "decision-name",
  "x": 100, "y": 100, "width": 180, "height": 100,
  "strokeColor": "<decision stroke from color-palette.md>",
  "backgroundColor": "<decision fill from color-palette.md>",
  "strokeWidth": 2,
  "strokeStyle": "solid",
  "boundElements": [{"id": "decision-name-label", "type": "text"}]
}
```

## Arrow

```json
{
  "type": "arrow",
  "id": "source-to-target",
  "x": 282, "y": 145, "width": 118, "height": 0,
  "strokeColor": "<source element's stroke color from color-palette.md>",
  "backgroundColor": "transparent",
  "strokeWidth": 2,
  "strokeStyle": "solid",
  "points": [[0, 0], [118, 0]],
  "startBinding": {"elementId": "source-id", "focus": 0, "gap": 2},
  "endBinding": {"elementId": "target-id", "focus": 0, "gap": 2},
  "startArrowhead": null,
  "endArrowhead": "arrow",
  "boundElements": null
}
```

For curves, add a midpoint: `[[0,0],[60,-40],[118,0]]`.

## Structural Line (timeline / tree)

```json
{
  "type": "line",
  "id": "timeline-line",
  "x": 100, "y": 200,
  "width": 0, "height": 300,
  "strokeColor": "<structural line color from color-palette.md>",
  "backgroundColor": "transparent",
  "strokeWidth": 2,
  "strokeStyle": "solid",
  "points": [[0, 0], [0, 300]],
  "boundElements": null
}
```

## Marker Dot (timeline node)

```json
{
  "type": "ellipse",
  "id": "timeline-dot-1",
  "x": 94, "y": 294,
  "width": 12, "height": 12,
  "strokeColor": "<marker dot color from color-palette.md>",
  "backgroundColor": "<marker dot color from color-palette.md>",
  "strokeWidth": 1,
  "strokeStyle": "solid",
  "boundElements": null
}
```

## Section Container

```json
{
  "type": "rectangle",
  "id": "section-name",
  "x": 0, "y": 0, "width": 800, "height": 600,
  "strokeColor": "#1e3a5f",
  "backgroundColor": "#f0f7ff",
  "strokeWidth": 2,
  "strokeStyle": "dashed",
  "opacity": 30,
  "roundness": {"type": 3},
  "boundElements": null
}
```

Note: `opacity: 30` overrides the fixed default for section containers only.

## Evidence Artifact (code / data block)

```json
{
  "type": "rectangle",
  "id": "evidence-name",
  "x": 100, "y": 200, "width": 300, "height": 150,
  "strokeColor": "#334155",
  "backgroundColor": "#1e293b",
  "strokeWidth": 1,
  "strokeStyle": "solid",
  "roundness": {"type": 3},
  "boundElements": [{"id": "evidence-name-text", "type": "text"}]
}
```

Text inside: `fontSize: 13`, `strokeColor` = green (`#22c55e`) for JSON, or language-appropriate for code.
