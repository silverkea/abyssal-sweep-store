## ADDED Requirements

### Requirement: Visual appearance of debris entities
Each debris entity in the grid SHALL be rendered using the sprite type assigned to its row as defined by the `enemy-sprites` capability. Entities SHALL NOT appear as coloured rectangles or other placeholder shapes during normal gameplay.

#### Scenario: Entities visible as sprites in gameplay
- **WHEN** the PLAYING game state is active and at least one debris entity remains
- **THEN** every surviving entity is drawn using its row-assigned sprite image rather than a geometric placeholder
