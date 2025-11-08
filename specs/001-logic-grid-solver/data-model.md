# Data Model: Logic Grid Puzzle Solver

## Core Entities

### Puzzle Definition

```typescript
interface Puzzle {
  id: string;               // Unique identifier for the puzzle
  title: string;           // Display name of the puzzle
  description?: string;    // Optional puzzle description
  categories: Category[];  // List of categories to match
  clues: Clue[];          // List of given clues
  created: string;        // ISO date string of creation
  modified: string;       // ISO date string of last modification
}

interface Category {
  id: string;            // Unique identifier within puzzle
  name: string;          // Display name of category
  items: Item[];         // List of items in this category
}

interface Item {
  id: string;           // Unique identifier within category
  name: string;         // Display name of item
}

interface Clue {
  id: string;          // Unique identifier within puzzle
  text: string;        // Human-readable clue text
  relationships: Relationship[]; // Encoded logical relationships
}

interface Relationship {
  type: "MUST" | "CANNOT";  // Relationship type
  fromCategory: string;     // Source category ID
  fromItem: string;         // Source item ID
  toCategory: string;       // Target category ID
  toItem: string;          // Target item ID
}
```

### Grid State

```typescript
interface GridState {
  puzzleId: string;        // Reference to puzzle definition
  cells: CellState[][];    // 2D array of cell states
  modified: string;        // ISO date string of last change
}

interface CellState {
  status: "UNKNOWN" | "CONFIRMED" | "ELIMINATED";
  userMarked: boolean;     // True if marked by user vs deduced
  deducedFrom?: string[];  // IDs of cells this was deduced from
}

interface SavedState {
  puzzleId: string;
  timestamp: string;      // ISO date string
  gridState: GridState;   // Current grid state
  history: Change[];      // List of user actions
}

interface Change {
  timestamp: string;      // When the change occurred
  cellX: number;         // X coordinate in grid
  cellY: number;         // Y coordinate in grid
  oldStatus: CellState["status"];
  newStatus: CellState["status"];
  automatic: boolean;    // True if change was automatic deduction
}
```

## Storage Schema

### File Structure

```
puzzles/
├── definitions/
│   ├── {puzzleId}.json        # Puzzle definitions
│   └── index.json             # List of available puzzles
├── samples/
│   └── {sampleId}.json       # Sample puzzles for testing
└── progress/
    └── {puzzleId}/
        ├── current.json      # Current state if unsaved
        └── saves/
            └── {timestamp}.json  # Named save points
```

### File Formats

#### puzzle-definition.json
```json
{
  "id": "puzzle-123",
  "title": "Pet Shop Mystery",
  "description": "Match pets with their owners",
  "categories": [
    {
      "id": "pets",
      "name": "Pets",
      "items": [
        {"id": "dog", "name": "Dog"},
        {"id": "cat", "name": "Cat"}
      ]
    },
    {
      "id": "owners",
      "name": "Owners",
      "items": [
        {"id": "alice", "name": "Alice"},
        {"id": "bob", "name": "Bob"}
      ]
    }
  ],
  "clues": [
    {
      "id": "clue-1",
      "text": "Alice does not own a cat",
      "relationships": [
        {
          "type": "CANNOT",
          "fromCategory": "owners",
          "fromItem": "alice",
          "toCategory": "pets",
          "toItem": "cat"
        }
      ]
    }
  ],
  "created": "2025-11-08T12:00:00Z",
  "modified": "2025-11-08T12:00:00Z"
}
```

#### saved-state.json
```json
{
  "puzzleId": "puzzle-123",
  "timestamp": "2025-11-08T14:30:00Z",
  "gridState": {
    "puzzleId": "puzzle-123",
    "cells": [
      [
        {
          "status": "CONFIRMED",
          "userMarked": true
        },
        {
          "status": "ELIMINATED",
          "userMarked": false,
          "deducedFrom": ["0,0"]
        }
      ]
    ],
    "modified": "2025-11-08T14:30:00Z"
  },
  "history": [
    {
      "timestamp": "2025-11-08T14:29:55Z",
      "cellX": 0,
      "cellY": 0,
      "oldStatus": "UNKNOWN",
      "newStatus": "CONFIRMED",
      "automatic": false
    }
  ]
}
```

## Validation Rules

1. **Puzzle Definition**:
   - All IDs must be unique within their scope
   - At least 2 categories required
   - Each category must have at least 1 item
   - All category/item references in clues must exist

2. **Grid State**:
   - Grid dimensions must match category sizes
   - Cell coordinates must be valid for grid size
   - Status changes must be valid transitions
   - Deduced relationships must be logically sound

3. **Save Files**:
   - All required fields must be present
   - Timestamps must be valid ISO strings
   - File names must match expected format
   - JSON must be well-formed