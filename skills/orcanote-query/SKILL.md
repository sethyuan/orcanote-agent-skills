---
name: orcanote-query
description: Guide for querying/searching data (blocks) in Orca Note.
---

# Orca Note Querying

This skill provides guidance on how to query blocks in Orca Note using the `QueryDescription2` schema. This is used for advanced filtering, search, and data retrieval within repositories.

## Query Structure

A query consists of a `QueryDescription2` object, which contains a main query group `q`, sorting options, and pagination settings.

```typescript
{
  q: QueryGroup2;          // The main query criteria
  sort?: [string, string][]; // e.g., [["_created", "DESC"]]
  page?: number;           // 1-based page number
  pageSize?: number;       // Number of items per page
  randomSeed?: number;     // For stable random sorting
}
```

## Query Groups (Logical Operators)

Groups combine multiple conditions. Every group has a `kind` and a `conditions` array.

| Kind | Name | Description |
| :--- | :--- | :--- |
| **100** | `SELF_AND` | All conditions in the array must match the block. |
| **101** | `SELF_OR` | At least one condition in the array must match the block. |
| **106** | `CHAIN_AND`| Matches blocks that have a match in their ancestors, descendants, or themselves. Often used within a `SELF_AND` group to filter results by hierarchy. |

### Hierarchy Groups
- **102**: `ANCESTOR_AND` - Matches ancestors of blocks that match conditions.
- **104**: `DESCENDANT_AND` - Matches descendants of blocks that match conditions.

## Condition Types

Each condition object must have a `kind` field.

| Kind | Type | Key Fields | description |
| :--- | :--- | :--- | :--- |
| **3** | **Journal** | `start`, `end` | Journal blocks within a specific journal date range. |
| **4** | **Tag** | `name`, `properties` | Blocks with a specific tag. Can filter by tag properties. |
| **6** | **Reference** | `blockId` | Blocks referencing a specific block ID. |
| **8** | **Text** | `text`, `raw` | Blocks containing specified text. |
| **9** | **Block** | `types`, `hasParent`, `hasChild`, `hasTags`, `created`, `modified` | Filter by block metadata and properties. |
| **11** | **Task** | `completed` | Specifically for task blocks. |
| **12** | **Block Match**| `blockId` | Match a specific block by its unique ID. |
| **13** | **Format** | `f`, `fa` | Match content fragments with specific markdown formats (e.g., bold, italic). |

## Dates (Relative & Absolute)

Used in Journal queries and Block property filters (created/modified).

- **Relative Date**: `{"t": 1, "v": -7, "u": "d"}` (e.g., 7 days ago).
    - `t`: 1 (Relative)
    - `v`: Numeric value (positive or negative)
    - `u`: Unit (`s`, `m`, `h`, `d`, `w`, `M`, `y`)
- **Absolute Date**: `{"t": 2, "v": 1640995200000}` (Unix timestamp in milliseconds).
    - `t`: 2 (Absolute)
    - `v`: Timestamp

## Operators (`op`)

Used for property comparisons (tag properties, block metadata, etc.).

| Value | Operator | Description |
| :--- | :--- | :--- |
| **1** | `eq` | Equals |
| **2** | `noteq` | Not equals |
| **3** | `includes` | Includes (for strings or lists) |
| **4** | `notincludes` | Does not include |
| **5** | `has` | Has property/tag |
| **6** | `nothas` | Does not have |
| **7** | `gt` | Greater than |
| **8** | `lt` | Less than |
| **9** | `ge` | Greater than or equal |
| **10** | `le` | Less than or equal |
| **11** | `null` | Is null / empty |
| **12** | `notnull` | Is not null / not empty |

## Sorting & Pagination

### Sorting
Specified as an array of tuples: `[field, direction]`.
- **Fields**: `_created`, `_modified`, `_text`, `_journal`, `_refcount`.
- **Direction**: `"ASC"` or `"DESC"`.

Example: `[["_created", "DESC"], ["_text", "ASC"]]`

### Pagination
- `page`: Default is `1`.
- `pageSize`: Default is `20`.

## Example Queries

### 1. Incomplete tasks tagged with "Project A"
```json
{
  "q": {
    "kind": 100,
    "conditions": [
      { "kind": 4, "name": "Project A" },
      { "kind": 11, "completed": false }
    ]
  }
}
```

### 2. Blocks containing "deadline" created in the last 7 days
```json
{
  "q": {
    "kind": 100,
    "conditions": [
      { "kind": 8, "text": "deadline" },
      {
        "kind": 9,
        "created": { "op": 9, "value": { "t": 1, "v": -7, "u": "d" } }
      }
    ]
  },
  "sort": [["_created", "DESC"]]
}
```

### 3. Hierarchy Search (Chain AND)
Find blocks containing "setup" that are children (or descendants, or parent, or ancestors) of blocks containing "environment".
```json
{
  "q": {
    "kind": 100,
    "conditions": [
      { "kind": 8, "text": "setup" },
      {
        "kind": 106,
        "conditions": [{ "kind": 8, "text": "environment" }]
      }
    ]
  }
}
```
