## Purpose

Defines how debris grid entities are visually rendered using per-row sprite images with 2-frame animation cycling, replacing the plain coloured square placeholders used during early development.

## ADDED Requirements

### Requirement: Per-row sprite type assignment
Each row of the debris grid SHALL be assigned a distinct sprite type according to the following mapping, from top row to bottom row: bottle, cup, can, barrel, oil. All entities within a given row SHALL use the same sprite type.

#### Scenario: Top row renders as bottle
- **WHEN** the debris grid is rendered
- **THEN** all entities in row 1 (top) display the bottle sprite

#### Scenario: Second row renders as cup
- **WHEN** the debris grid is rendered
- **THEN** all entities in row 2 display the cup sprite

#### Scenario: Third row renders as can
- **WHEN** the debris grid is rendered
- **THEN** all entities in row 3 display the can sprite

#### Scenario: Fourth row renders as barrel
- **WHEN** the debris grid is rendered
- **THEN** all entities in row 4 display the barrel sprite

#### Scenario: Fifth row renders as oil
- **WHEN** the debris grid is rendered
- **THEN** all entities in row 5 (bottom) display the oil sprite

### Requirement: Two-frame idle animation
Each debris entity SHALL cycle between its two sprite frames to produce a continuous idle shimmer effect. The two frames SHALL alternate at a consistent interval shared across all entities.

#### Scenario: Frame alternates over time
- **WHEN** the game is running and the debris grid is visible
- **THEN** each entity alternates between frame 1 and frame 2 of its assigned sprite type at a regular interval

#### Scenario: All entities share the same animation phase
- **WHEN** the debris grid is rendered
- **THEN** all entities of the same sprite type display the same frame at the same time

### Requirement: Sprite replaces square placeholder
Debris entities SHALL NOT render as coloured squares. Each entity SHALL be drawn using its assigned sprite image at the entity's current grid position.

#### Scenario: No square visible during play
- **WHEN** the gameplay screen is active and debris entities are present
- **THEN** no plain coloured rectangles are visible in place of debris entities
