---
name: orcanote-whiteboard
description: Guide for creating Excalidraw based whiteboards and diagrams using Excalidraw JSON syntax for Orca Note. Use when users need to create whiteboards or diagrams, or when user mentions whiteboard or Excalidraw.
---

# Orca Note Whiteboard

Create Excalidraw based whiteboards and diagrams using text-based Excalidraw JSON syntax for Orca Note.

## Core Syntax Structure

A Orca Note whiteboard has the following basic structure:

```json
{
  "type": "excalidraw",
  "version": 2,
  "source": "https://excalidraw.com",
  "elements": [...],
  "files": {}
}
```

## Diagram Type Selection Guide

**Choose the right diagram type:**

| Type | Use Case | How to |
|------|---------|--------|
| **Flowchart** | Step-by-step procedures, workflows, task sequences | Connect steps with arrows to clearly show process direction |
| **Mind Map** | Idea expansion, topic categorization, brainstorming | Start from a central node and radiate outward with branches |
| **Hierarchy** | Organizational charts, content hierarchies, system breakdowns | Arrange nodes top-down or left-to-right to show levels |
| **Relationship** | Dependencies, influences, or interactions between elements | Use lines and arrows between shapes; add labels or annotations |
| **Comparison** | Comparing two or more options or perspectives | Use side-by-side columns or tables and label comparison dimensions |
| **Timeline** | Event progression, project schedules, model evolution | Use a time axis to mark key dates and milestones |
| **Matrix** | Two-dimensional classification, task prioritization, positioning | Create X and Y axes and place items within the coordinate plane |
| **Freeform** | Loose content, idea capture, preliminary information gathering | Place items freely without strict structure; use shapes and arrows as needed |

## Quick Start Example

```json
{
  "type": "excalidraw",
  "version": 2,
  "source": "https://excalidraw.com",
  "elements": [
    {
      "id": "3P2z4qSfdQj6cd0ahkJfC",
      "type": "rectangle",
      "x": -244.12890575,
      "y": -114.09765575,
      "width": 113.40625,
      "height": 60.93359375,
      "angle": 0,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "index": "a0",
      "roundness": null,
      "seed": 999077125,
      "version": 109,
      "versionNonce": 705510059,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "oUQQ9gFoB_w-SoZah8p1L",
          "type": "arrow"
        }
      ],
      "updated": 1770382887653,
      "link": null,
      "locked": false
    },
    {
      "id": "ctAQl2UvB6bA3MvwBTm0-",
      "type": "text",
      "x": -240.304687,
      "y": -143.6562495,
      "width": 31.167984008789062,
      "height": 21.6,
      "angle": 0,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "index": "a1",
      "roundness": null,
      "seed": 1269773957,
      "version": 46,
      "versionNonce": 1809895691,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1770382871766,
      "link": null,
      "locked": false,
      "text": "Text",
      "fontSize": 16,
      "fontFamily": 6,
      "textAlign": "left",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "Text",
      "autoResize": true,
      "lineHeight": 1.35
    },
    {
      "type": "embeddable",
      "link": "Go to",
      "customData": {
        "blockId": 430
      },
      "validated": true,
      "backgroundColor": "transparent",
      "strokeWidth": 2,
      "roughness": 0,
      "width": 300,
      "height": 260,
      "x": 12.1875,
      "y": -228.410156,
      "version": 170,
      "versionNonce": 1920519147,
      "index": "a2",
      "isDeleted": false,
      "id": "rT8agR-ZtM-CLnyzh9ydl",
      "fillStyle": "solid",
      "strokeStyle": "solid",
      "opacity": 100,
      "angle": 0,
      "strokeColor": "#1e1e1e",
      "seed": 1,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [
        {
          "id": "oUQQ9gFoB_w-SoZah8p1L",
          "type": "arrow"
        }
      ],
      "updated": 1770382887654,
      "locked": false
    },
    {
      "id": "oUQQ9gFoB_w-SoZah8p1L",
      "type": "arrow",
      "x": -125.72265575,
      "y": -83.730858875,
      "width": 129.0312495,
      "height": 0.3394536250000044,
      "angle": 0,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "index": "a3",
      "roundness": null,
      "seed": 959749797,
      "version": 104,
      "versionNonce": 1837542731,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1770382887654,
      "link": null,
      "locked": false,
      "points": [
        [
          0,
          0
        ],
        [
          129.0312495,
          -0.3394536250000044
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": {
        "elementId": "3P2z4qSfdQj6cd0ahkJfC",
        "fixedPoint": [
          1.044089280793607,
          0.4983588691582795
        ],
        "focus": 0,
        "gap": 0
      },
      "endBinding": {
        "elementId": "rT8agR-ZtM-CLnyzh9ydl",
        "fixedPoint": [
          -0.02959635416666667,
          0.5551532442307693
        ],
        "focus": 0,
        "gap": 0
      },
      "startArrowhead": null,
      "endArrowhead": "triangle",
      "elbowed": true,
      "fixedSegments": null,
      "startIsSpecial": null,
      "endIsSpecial": null
    }
  ],
  "files": {}
}
```

## Design Rules

### Layout & Design

- **Element Spacing**: Ensure appropriate spacing for a visually pleasing layout
- **Clear Hierarchy**: Use different colors and shapes to distinguish information levels
- **Graphic Elements**: Use rectangles, circles, arrows, etc. to organize information
- **No Emoji**: Do not use any Emoji symbols in diagram text; use simple shapes (circles, squares, arrows) or colors for visual markers

### Theming & Colors

Use harmonious color schemes and avoid excessive colors.

## Element Template

### Required Fields for All Elements

```json
{
  "id": "unique-identifier",
  "type": "rectangle|text|arrow|ellipse|diamond",
  "x": 100, "y": 100,
  "width": 200, "height": 50,
  "angle": 0,
  "strokeColor": "#color-hex",
  "backgroundColor": "transparent|#color-hex",
  "fillStyle": "solid",
  "strokeWidth": 2,
  "strokeStyle": "solid|dashed",
  "roughness": 1,
  "opacity": 100,
  "groupIds": [],
  "frameId": null,
  "index": "a1",
  "roundness": {"type": 3},
  "seed": 123456789,
  "version": 1,
  "versionNonce": 987654321,
  "isDeleted": false,
  "boundElements": [],
  "updated": 1751928342106,
  "link": null,
  "locked": false
}
```

### Block Elements

Elements that represent blocks has the type "embeddable" and an extra `customData` field:

```json
{
  "id": "block-1",
  "type": "embeddable",
  "customData": {
    "blockId": 123
  },
  ...
}
```

See [references/excalidraw-schema.md](references/excalidraw-schema.md) for all element types.

## Detailed References

- Excalidraw Schema: [references/excalidraw-schema.md](references/excalidraw-schema.md)

## Additional Technical Requirements

### Coordinates & Layout

- **Coordinate system**: origin at the top-left corner (0, 0)
- **Element ID**: each element must have a unique `id` (can be a string, e.g., "title", "box1")
- **Index field**: recommended to use alphanumeric values (a1, a2, a3...)
