---
name: orcanote-whiteboard
description: Guide for creating Excalidraw-based whiteboards in Orca Note. Use when user mentions whiteboard or Excalidraw.
---

# Orca Note Whiteboard (Excalidraw)

This skill provides the syntax, structural rules, and design principles for creating semantically rich and visually appealing whiteboards in Orca Note using the Excalidraw JSON format.

## Excalidraw JSON Schema

Whiteboards are defined within `whiteboard` code blocks.

### Core Structure

```json
{
  "elements": [],
  "files": {}
}
```

### Common Element Types

| Type | Description | Key Fields |
| :--- | :--- | :--- |
| `rectangle` | Standard rectangle | `x, y, width, height, strokeColor, backgroundColor, roundness, boundElements` |
| `ellipse` | Circle or oval | `x, y, width, height, strokeColor, backgroundColor, boundElements` |
| `arrow` | Line or connector | `points` (relative to `x,y`), `startArrowhead`, `endArrowhead`, `startBinding`, `endBinding`, `elbowed` (true for elbow arrows) |
| `line` | Simple line | `points` |
| `text` | Text label | `text`, `fontSize`, `fontFamily` (5: Hand-drawn, 6: Sans, 8: Monospace), `containerId` (ID of the shape this text belongs to), `x, y, width, height, strokeColor, backgroundColor`, `textAlign`, `verticalAlign` |
| `embeddable` | Embedded Orca Note block | `customData: { "blockId": "ID" }`, `x, y, width, height, strokeColor, backgroundColor, roundness` |

### Labeling Shapes with Text

To place text inside a shape (like a rectangle or ellipse):
1.  **Create the Shape**: Give it a unique `id`.
2.  **Create a Text Element**:
    - Set its `containerId` to the shape's `id`.
    - Set `x` and `y` to the container's `x` and `y`.
    - Set `width` and `height` to fit within the shape's dimensions.
    - Set `textAlign` and `verticalAlign` to center the text.
3.  **Link back**: Add the text element's ID to the shape's `boundElements` array: `{ "type": "text", "id": "text_id" }`.

### Arrow Types
- **Straight Arrow**: Standard line. No `elbowed` field.
- **Elbow Arrow**: Set `elbowed: true`. This creates a right-angled connector.

#### Arrowheads (`startArrowhead` / `endArrowhead`)
- `null`: No arrowhead.
- `"arrow"`, `"bar"`, `"dot"`, `"triangle"`.

### Arrow Connection / Connecting Two Elements
To link two shapes (e.g., A and B) via an arrow:
1.  **On the Arrow**: Define `startBinding: { "elementId": "A", "fixedPoint": [1, 0.5] }` and `endBinding: { "elementId": "B", "fixedPoint": [0, 0.5] }`.
2.  **On the Shapes**: Add `{ "id": "arrow_id", "type": "arrow" }` to the `boundElements` array of both Shape A and Shape B.

`fixedPoint` is a tuple of [x, y] where x and y are between 0 and 1, representing the relative position on the shape's boundary (0,0 is top-left, 1,1 is bottom-right). For example:
- `fixedPoint: [1, 0.5]` means the arrow starts at the middle of the right edge of Shape A.
- `fixedPoint: [0, 0.5]` means the arrow ends at the middle of the left edge of Shape B.

**Example Connection with Text labels:**
```json
{
  "elements": [
    {
      "id": "shape-a",
      "type": "rectangle",
      "x": 100, "y": 100, "width": 100, "height": 50,
      "boundElements": [
        { "id": "text-a", "type": "text" },
        { "id": "link-1", "type": "arrow" }
      ]
    },
    {
      "id": "text-a",
      "type": "text",
      "x": 100, "y": 100, "width": 50, "height": 20,
      "fontFamily": 6,
      "fontSize": 16,
      "text": "Start",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "shape-a"
    },
    {
      "id": "shape-b",
      "type": "ellipse",
      "x": 400, "y": 100, "width": 100, "height": 50,
      "boundElements": [
        { "id": "text-b", "type": "text" },
        { "id": "link-1", "type": "arrow" }
      ]
    },
    {
      "id": "text-b",
      "type": "text",
      "x": 400, "y": 100, "width": 50, "height": 20,
      "fontFamily": 6,
      "fontSize": 16,
      "text": "End",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "shape-b"
    },
    {
      "id": "link-1",
      "type": "arrow",
      "x": 200, "y": 125,
      "points": [[0, 0], [200, 0]],
      "startBinding": { "elementId": "shape-a", "fixedPoint": [1, 0.5] },
      "endBinding": { "elementId": "shape-b", "fixedPoint": [0, 0.5] },
      "endArrowhead": "triangle",
      "elbowed": true
    }
  ]
}
```

## Embedding Orca Note Blocks

To reference and display an existing Orca Note block inside a whiteboard, use the `embeddable` type. Note that embedded blocks have no associated text element for labeling.

### Implementation
- **type**: `embeddable`
- **customData**: Must contain `blockId` mapping to the target block's ID (number).
- **validated**: Must be true.
- **link**: String of value "go to".

### Example
```json
{
  "id": "block-1",
  "type": "embeddable",
  "x": 100,
  "y": 100,
  "width": 300,
  "height": 200,
  "customData": {
    "blockId": 88234
  },
  "validated": true,
  "link": "go to"
}
```

## Layout Guidance

Good layout uses **Proximity** to imply relationship and **White Space** to denote separation.

### 1. Mind Map
- **Structure**: A central "Root" node with radial or hierarchical branching.
- **Visual Grouping**:
  - Keep sub-topics within 50-100 pixels of their parent.
  - Leave at least 200 pixels between major branches to avoid overlap.
  - **Hierarchy**: Use larger font sizes (e.g., `fontSize: 28`) for the root and smaller (e.g., `fontSize: 16`) for leaf nodes.
- **Example Pattern**:
  - Center: Rectangle (`backgroundColor: "#1d3557"`, `text: "Goal"`, `strokeColor: "#ffffff"`).
  - Branches: Curved arrows linking to smaller rectangles with thinner strokes.

### 2. Relationship Map (Clue Wall)
- **Structure**: A non-linear web of interconnected entities (blocks, notes, images).
- **Visual Grouping**:
  - **Cluster Mapping**: Place semantically similar elements (e.g., all "Evidence" blocks) in a tight spatial cluster.
  - **Guttering**: Ensure unrelated clusters are separated by a "dead zone" of at least 300 pixels.
  - **Connector Contrast**: Use `strokeStyle: "solid"` for direct evidence and `"dashed"` for speculative links.
- **Visual Hierarchy**: Highlight key "anchor" blocks by giving them a `strokeWidth: 4` or a distinct `backgroundColor`.

## Styling & Color Themes

Color and style should be used semantically to reduce cognitive load.

### 1. Font Sizes
Choose ONLY from the following 4 font sizes:
- `fontSize: 16` (Small)
- `fontSize: 20` (Medium)
- `fontSize: 28` (Large)
- `fontSize: 36` (Extra Large)

### 1. Style Variations
- **Professional**:
  - `roughness: 0`, `fontFamily: 6`, `roundness: null` (sharp corners) or `type: 2`.
  - Best for: Technical diagrams, architecture, formal reports.
- **Hand-drawn**:
  - `roughness: 1.5`, `fontFamily: 5`, `roundness: { type: 3 }`.
  - Best for: Brainstorming, user journey maps, creative storytelling.

### 2. Semantic Color Palettes
Assign colors based on the role of the element:

| Role | Professional Palette | Hand-drawn Palette |
| :--- | :--- | :--- |
| **Core Concept** | Deep Blue (#003049) | Vibrant Red (#e63946) |
| **Support Info** | Light Gray (#e0e0e0) | Pale Blue (#a8dadc) |
| **Positive/Success** | Forest Green (#2a9d8f) | Mint Green (#f1faee) |
| **Warning/Risk** | Amber (#ffb703) | Orange (#f4a261) |
| **Action Items** | Indigo (#4cc9f0) | Purple (#7209b7) |

## Examples

### 1. Mind Map (Hand-drawn Hierarchical)
```whiteboard
{
  "elements": [
    {
      "id": "m1", "type": "rectangle", "x": 500, "y": 300, "width": 150, "height": 60,
      "backgroundColor": "#e63946", "roughness": 1.5, "roundness": { "type": 3 },
      "boundElements": [{ "id": "t1", "type": "text" }, { "id": "a1", "type": "arrow" }]
    },
    { "id": "t1", "type": "text", "x": 500, "y": 300, "width": 150, "height": 60, "text": "Project Goal", "fontFamily": 5, "fontSize": 20, "textAlign": "center", "verticalAlign": "middle", "containerId": "m1", "strokeColor": "#ffffff" },
    {
      "id": "c1", "type": "rectangle", "x": 750, "y": 200, "width": 120, "height": 50,
      "backgroundColor": "#a8dadc", "roughness": 1.5, "roundness": { "type": 3 },
      "boundElements": [{ "id": "t2", "type": "text" }, { "id": "a1", "type": "arrow" }]
    },
    { "id": "t2", "type": "text", "x": 750, "y": 200, "width": 120, "height": 50, "text": "Task A", "fontFamily": 5, "fontSize": 16, "textAlign": "center", "verticalAlign": "middle", "containerId": "c1" },
    {
      "id": "a1", "type": "arrow", "x": 650, "y": 330, "points": [[0, 0], [100, -105]],
      "startBinding": { "elementId": "m1", "fixedPoint": [1, 0.5] },
      "endBinding": { "elementId": "c1", "fixedPoint": [0, 0.5] },
      "elbowed": true, "roughness": 1.5
    }
  ]
}
```

### 2. Relationship Map (Professional Clue Wall with Embeds)
```whiteboard
{
  "elements": [
    { "id": "clue-1", "type": "embeddable", "x": 100, "y": 100, "width": 250, "height": 180, "customData": { "blockId": 5501 }, "strokeColor": "#003049", "strokeWidth": 2, "validated": true, "link": "go to" },
    { "id": "clue-2", "type": "embeddable", "x": 100, "y": 350, "width": 250, "height": 180, "customData": { "blockId": 5502 }, "strokeColor": "#003049", "validated": true, "link": "go to" },
    { "id": "label-cat1", "type": "text", "x": 100, "y": 50, "text": "CATEGORY: SOURCES", "fontFamily": 6, "fontSize": 20, "strokeColor": "#003049" },

    { "id": "clue-3", "type": "embeddable", "x": 600, "y": 100, "width": 250, "height": 180, "customData": { "blockId": 7701 }, "strokeColor": "#2a9d8f", "validated": true, "link": "go to" },
    { "id": "label-cat2", "type": "text", "x": 600, "y": 50, "text": "CATEGORY: FINDINGS", "fontFamily": 6, "fontSize": 20, "strokeColor": "#2a9d8f" },

    { "id": "link-1", "type": "arrow", "x": 350, "y": 190, "points": [[0,0], [250, 0]], "strokeStyle": "dashed", "strokeColor": "#ffb703" }
  ]
}
```
