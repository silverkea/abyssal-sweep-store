## Purpose

Defines the behavior of the four Coral Reef Barrier structures that shield the seafloor, including their dynamic erosion mechanics when struck by hostile or friendly projectiles.

### Requirement: Four barrier placement
The game SHALL render four Coral Reef Barrier structures positioned horizontally between the Eco-Submarine and the bottom row of the debris grid. The barriers SHALL be evenly distributed across the screen width.

#### Scenario: Initial barrier layout
- **WHEN** a new game session or level begins
- **THEN** four intact Coral Reef Barrier structures are visible, evenly spaced above the submarine

### Requirement: Dynamic erosion on projectile impact
Each Coral Reef Barrier SHALL erode at the precise point of projectile impact when struck by a toxic sludge drip or a stray sonar bubble pulse. The erosion SHALL be visualized as localized destruction of the barrier's rendered texture. The barrier SHALL remain present and partially functional until it is fully eroded.

#### Scenario: Sludge drip hits barrier
- **WHEN** a toxic sludge drip collides with a Coral Reef Barrier
- **THEN** the sludge drip is destroyed and the barrier's texture is eroded at the impact coordinate

#### Scenario: Bubble pulse hits barrier
- **WHEN** a sonar bubble pulse collides with a Coral Reef Barrier
- **THEN** the bubble pulse is destroyed and the barrier's texture is eroded at the impact coordinate

#### Scenario: Partial barrier remains passable
- **WHEN** a barrier has been partially eroded such that a gap exists at a specific horizontal position
- **THEN** projectiles passing through that gap are not blocked by the barrier

### Requirement: Bleached visual degradation
As a Coral Reef Barrier accumulates erosion damage, its visual appearance SHALL shift from vibrant coral coloring toward a bleached, degraded state to communicate structural decay.

#### Scenario: Undamaged barrier appearance
- **WHEN** a Coral Reef Barrier has received no hits
- **THEN** the barrier is rendered with full color and intact form

#### Scenario: Heavily damaged barrier appearance
- **WHEN** a Coral Reef Barrier has been struck multiple times and is mostly eroded
- **THEN** the barrier is rendered with a bleached or faded visual state and visibly fragmented form

### Requirement: Barrier destruction
A Coral Reef Barrier SHALL be fully destroyed and removed from play when its entire rendered area has been eroded, leaving no remaining cover.

#### Scenario: Fully eroded barrier removed
- **WHEN** a Coral Reef Barrier's remaining solid texture area reaches zero
- **THEN** the barrier is removed from the scene and no longer blocks projectiles
