## ADDED Requirements

### Requirement: Submarine collision bounds match scaled sprite size
The submarine's collision detection area SHALL correspond to its rendered sprite dimensions
at 150% of the baseline asset size. The hitbox SHALL not use the unscaled asset dimensions.

#### Scenario: Collision bounds reflect scaled width
- **WHEN** the submarine is rendered at 150% of its baseline width
- **THEN** the collision detection area spans the same horizontal extent as the rendered sprite

#### Scenario: Collision bounds reflect scaled height
- **WHEN** the submarine is rendered at 150% of its baseline height
- **THEN** the collision detection area spans the same vertical extent as the rendered sprite
