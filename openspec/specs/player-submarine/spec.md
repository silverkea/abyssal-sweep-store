## Purpose

Defines the behavior of the player-controlled Eco-Submarine entity: its movement, single-shot sonar firing, and integrity-based life system.

### Requirement: Horizontal keyboard movement
The Eco-Submarine SHALL move horizontally across the seafloor in response to keyboard input. Left Arrow or A key SHALL move the submarine left; Right Arrow or D key SHALL move the submarine right. The submarine SHALL not move beyond the left or right screen boundary.

#### Scenario: Move left within bounds
- **WHEN** the player holds the Left Arrow or A key and the submarine is not at the left boundary
- **THEN** the submarine moves left at the defined movement speed

#### Scenario: Move right within bounds
- **WHEN** the player holds the Right Arrow or D key and the submarine is not at the right boundary
- **THEN** the submarine moves right at the defined movement speed

#### Scenario: Boundary clamp left
- **WHEN** the player holds the Left Arrow or A key and the submarine is at the left boundary
- **THEN** the submarine stops and does not move further left

#### Scenario: Boundary clamp right
- **WHEN** the player holds the Right Arrow or D key and the submarine is at the right boundary
- **THEN** the submarine stops and does not move further right

### Requirement: Single-shot sonar bubble pulse
The Eco-Submarine SHALL fire a sonar bubble pulse upward when the player presses Spacebar. A maximum of one bubble pulse SHALL be active on screen at any time. If a bubble pulse is already active, pressing Spacebar SHALL have no effect. The bubble pulse SHALL travel upward until it collides with a debris entity or exits the top of the screen.

#### Scenario: Fire when no active pulse
- **WHEN** the player presses Spacebar and no bubble pulse is currently active
- **THEN** a new bubble pulse is spawned at the submarine's current position and travels upward

#### Scenario: Fire blocked by active pulse
- **WHEN** the player presses Spacebar and a bubble pulse is already active on screen
- **THEN** no new bubble pulse is spawned

#### Scenario: Pulse exits screen
- **WHEN** an active bubble pulse reaches the top boundary of the screen without hitting any debris
- **THEN** the pulse is destroyed and the player may fire again

### Requirement: Submarine integrity points (lives)
The submarine SHALL start each game session with 3 Submarine Integrity points. The submarine SHALL lose one integrity point when struck by a toxic sludge drip projectile. When integrity points reach zero, the game SHALL transition to the GAME_OVER state.

#### Scenario: Hit by sludge drip
- **WHEN** a toxic sludge drip projectile collides with the submarine
- **THEN** the submarine loses one integrity point and the sludge drip is destroyed

#### Scenario: Integrity depleted
- **WHEN** the submarine's integrity reaches zero
- **THEN** the game transitions to the GAME_OVER state

#### Scenario: Initial integrity
- **WHEN** a new game session begins
- **THEN** the submarine starts with exactly 3 integrity points
