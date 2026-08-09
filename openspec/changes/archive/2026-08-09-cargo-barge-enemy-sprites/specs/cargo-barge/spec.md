## ADDED Requirements

### Requirement: Animated sprite rendering
The Cargo Barge SHALL render using an animated sprite composed of the `assets/images/barge_*.png` frame sequence. The animation SHALL loop continuously at a fixed frame rate while the barge is active on screen.

#### Scenario: Barge animates while traversing
- **WHEN** the Cargo Barge is active and moving across the screen
- **THEN** the barge sprite cycles through all `barge_*.png` frames in order, looping at a consistent fixed frame rate

#### Scenario: Barge sprite matches barge hitbox extent
- **WHEN** the Cargo Barge is rendered
- **THEN** the animated sprite is positioned and sized to match the barge's collision bounds
