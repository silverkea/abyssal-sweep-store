## Purpose

Defines the behavior of the Sinking Debris Grid — the primary adversary fleet of marine waste that sinks toward the reef, scores points when neutralized, and actively threatens the player with toxic sludge.

### Requirement: Grid formation and debris tiers
The debris grid SHALL consist of 5 rows and 11 columns of debris entities (55 total). Rows SHALL be assigned to scoring tiers: Tier 1 (top rows — Light Plastic Bottles / Microplastic Swarms) awards 30 points per piece, Tier 2 (middle rows — Ghost Fishing Nets) awards 20 points per piece, and Tier 3 (bottom rows — Heavy Chemical and Oil Drums) awards 10 points per piece.

#### Scenario: Initial grid population
- **WHEN** a new level begins
- **THEN** the grid is populated with 55 debris entities arranged in 5 rows × 11 columns with correct tier assignments

#### Scenario: Tier 1 scoring
- **WHEN** a bubble pulse collides with a Tier 1 debris entity
- **THEN** that entity is removed and 30 points are added to the player's score

#### Scenario: Tier 2 scoring
- **WHEN** a bubble pulse collides with a Tier 2 debris entity
- **THEN** that entity is removed and 20 points are added to the player's score

#### Scenario: Tier 3 scoring
- **WHEN** a bubble pulse collides with a Tier 3 debris entity
- **THEN** that entity is removed and 10 points are added to the player's score

### Requirement: Synchronized horizontal drift with edge-bounce drops
All remaining debris entities SHALL move as a single synchronized grid. The grid SHALL drift horizontally in one direction. When any debris entity in the grid reaches the left or right screen boundary, the entire grid SHALL shift downward by one step and reverse horizontal direction.

#### Scenario: Right boundary reached
- **WHEN** the rightmost debris entity in the grid reaches the right screen boundary
- **THEN** the entire grid drops downward one step and begins moving left

#### Scenario: Left boundary reached
- **WHEN** the leftmost debris entity in the grid reaches the left screen boundary
- **THEN** the entire grid drops downward one step and begins moving right

### Requirement: Speed scaling by remaining debris count
The horizontal movement speed of the debris grid SHALL increase as the number of remaining debris entities decreases. The speed SHALL be at its minimum when all 55 entities are present and SHALL reach its maximum when only one entity remains.

#### Scenario: Full grid speed
- **WHEN** 55 debris entities remain
- **THEN** the grid moves at minimum drift speed

#### Scenario: Single entity speed
- **WHEN** only 1 debris entity remains
- **THEN** the grid moves at maximum drift speed

#### Scenario: Intermediate speed scaling
- **WHEN** debris entities are progressively destroyed
- **THEN** grid speed increases continuously with each entity removed

### Requirement: Toxic sludge drip projectiles
Chemical Drum entities in the bottom occupied row of the grid SHALL periodically fire toxic sludge drip projectiles downward toward the seafloor. Sludge drips SHALL travel downward and SHALL collide with Coral Reef Barrier structures or the Eco-Submarine.

#### Scenario: Sludge drip fired
- **WHEN** a periodic firing interval elapses for a Chemical Drum in the bottom occupied row
- **THEN** that Chemical Drum fires a toxic sludge drip projectile downward from its position

#### Scenario: Sludge hits reef
- **WHEN** a sludge drip collides with a Coral Reef Barrier
- **THEN** the sludge drip is destroyed and the reef barrier takes erosion damage at the impact point

#### Scenario: Sludge hits submarine
- **WHEN** a sludge drip collides with the Eco-Submarine
- **THEN** the sludge drip is destroyed and the submarine loses one integrity point

#### Scenario: Sludge exits screen
- **WHEN** a sludge drip reaches the bottom boundary of the screen without hitting anything
- **THEN** the sludge drip is destroyed

### Requirement: Grid reaching the seafloor triggers game over
If the debris grid descends far enough that any surviving debris entity reaches the seafloor level, the game SHALL immediately transition to the GAME_OVER state.

#### Scenario: Grid reaches seafloor
- **WHEN** any debris entity's position reaches or crosses the seafloor boundary
- **THEN** the game transitions to the GAME_OVER state regardless of remaining player integrity

### Requirement: Level complete when grid is cleared
When all 55 debris entities have been neutralized, the game SHALL transition to the LEVEL_COMPLETE state.

#### Scenario: Last debris destroyed
- **WHEN** the final debris entity is neutralized by a bubble pulse
- **THEN** the game transitions to the LEVEL_COMPLETE state

### Requirement: Visual appearance of debris entities
Each debris entity in the grid SHALL be rendered using the sprite type assigned to its row as defined by the `enemy-sprites` capability. Entities SHALL NOT appear as coloured rectangles or other placeholder shapes during normal gameplay.

#### Scenario: Entities visible as sprites in gameplay
- **WHEN** the PLAYING game state is active and at least one debris entity remains
- **THEN** every surviving entity is drawn using its row-assigned sprite image rather than a geometric placeholder
