# Quality Checklist

## Before Drawing

- [ ] Depth assessed: simple/conceptual or comprehensive/technical?
- [ ] For technical diagrams: specs, event names, and data formats researched
- [ ] Coordinate regions planned — sections don't overlap
- [ ] Reading path decided: top→bottom or left→right

## After Generating JSON

- [ ] All colors sourced from `color-palette.md`
- [ ] Meaningful element IDs — no `elem1`, `elem2`
- [ ] Every shape with text has `boundElements` declaring the text ID
- [ ] Every text inside a shape has `containerId` set
- [ ] `roughness: 0`, `opacity: 100` on all elements (except section containers: `opacity: 30`)

## After Rendering to PNG

- [ ] **Isomorphism**: structure still communicates without text
- [ ] **Education**: teaches concrete details, not just labels
- [ ] All text fully visible — no clipping
- [ ] No elements overlap unintentionally
- [ ] Arrows route cleanly, no unnecessary crossings
- [ ] Evidence artifacts present in technical diagrams
- [ ] Composition feels balanced
