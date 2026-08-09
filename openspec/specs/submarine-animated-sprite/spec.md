## Purpose

Defines the behavior of the submarine hero's visual representation as an animated sprite that cycles through frames, replacing the placeholder Graphics-based drawing.

## Requirements

### Requirement: Submarine renders as an animated sprite
The submarine hero SHALL be rendered using sprite frames loaded from `sub_frame*.png` assets rather than programmatic Graphics primitives.

#### Scenario: Submarine visible during play
- **WHEN** a game round is in progress
- **THEN** the submarine is displayed using its sprite frames, not a geometric shape

### Requirement: Submarine sprite animates continuously
The submarine sprite SHALL cycle through all available frames in a looping animation while the game is in the playing state.

#### Scenario: Animation loops without stopping
- **WHEN** the submarine is on screen
- **THEN** the sprite cycles through all frames repeatedly without pausing

#### Scenario: Animation plays from game start
- **WHEN** the playing state is entered
- **THEN** the submarine sprite animation is already playing

### Requirement: Submarine sprite aligns with collision bounds
The submarine sprite SHALL be sized and anchored so that its visual center matches the entity's logical position used for collision detection.

#### Scenario: Sprite center matches position
- **WHEN** the submarine is at position (x, y)
- **THEN** the sprite's visual center is rendered at (x, y)
