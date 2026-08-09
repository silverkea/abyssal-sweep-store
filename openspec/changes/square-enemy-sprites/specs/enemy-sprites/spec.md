## ADDED Requirements

### Requirement: Square sprite display dimensions
Each debris entity sprite SHALL be rendered at equal width and height (square aspect ratio). The display size SHALL be 32×32 pixels. The collision hitbox for each entity SHALL match the square display dimensions.

#### Scenario: Sprite renders as square
- **WHEN** a debris entity is visible in the gameplay area
- **THEN** the entity's rendered width equals its rendered height

#### Scenario: Hitbox matches square sprite
- **WHEN** collision detection is performed against a debris entity
- **THEN** the entity's collision bounds form a 32×32 pixel square centered on the entity's position
