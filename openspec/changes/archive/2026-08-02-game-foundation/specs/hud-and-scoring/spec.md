## Purpose

Defines the heads-up display and scoring system, including live score accumulation, persistent high score storage, reef health indicator, and level display.

## ADDED Requirements

### Requirement: Live score display
The HUD SHALL display the player's current Cleaned Debris Score at all times during the PLAYING and PAUSED states. The score SHALL update immediately when points are awarded.

#### Scenario: Score updates on debris neutralized
- **WHEN** a debris entity is neutralized by a bubble pulse
- **THEN** the displayed score increases by that entity's point value within the same frame

#### Scenario: Score updates on barge hit
- **WHEN** the Cargo Barge is hit
- **THEN** the displayed score increases by the barge's bonus point value

### Requirement: High score persistence via localStorage
The game SHALL persist the player's all-time high score in browser localStorage. The high score SHALL be updated at the end of a session if the current score exceeds it. The HUD SHALL display the stored high score alongside the current score.

#### Scenario: High score loaded on start
- **WHEN** the game initializes
- **THEN** the stored high score is read from localStorage and displayed in the HUD

#### Scenario: High score updated on game over
- **WHEN** the game transitions to GAME_OVER and the current score exceeds the stored high score
- **THEN** localStorage is updated with the new high score and the HUD reflects the new value

#### Scenario: High score not overwritten by lower score
- **WHEN** the game transitions to GAME_OVER and the current score is less than or equal to the stored high score
- **THEN** localStorage is not modified

### Requirement: Reef health indicator
The HUD SHALL display a Reef Health indicator that reflects the aggregate structural health of all four Coral Reef Barrier structures. The indicator SHALL decrease as barriers are eroded.

#### Scenario: Full reef health at start
- **WHEN** a new game session or level begins
- **THEN** the Reef Health indicator is at its maximum value

#### Scenario: Reef health decreases on barrier damage
- **WHEN** a Coral Reef Barrier sustains erosion damage
- **THEN** the Reef Health indicator decreases proportionally

#### Scenario: Reef health at zero when all barriers destroyed
- **WHEN** all four Coral Reef Barriers are fully destroyed
- **THEN** the Reef Health indicator reads zero

### Requirement: Level display
The HUD SHALL display the current level number throughout the PLAYING and PAUSED states.

#### Scenario: Level shown during play
- **WHEN** the game is in PLAYING state
- **THEN** the current level number is visible in the HUD

#### Scenario: Level increments on advance
- **WHEN** the game advances from LEVEL_COMPLETE to the next PLAYING session
- **THEN** the displayed level number increases by one

### Requirement: Semi-transparent HUD overlay
The HUD SHALL be rendered as a semi-transparent overlay that does not fully obscure the game scene behind it.

#### Scenario: HUD does not block scene
- **WHEN** the game is in PLAYING state
- **THEN** game entities behind the HUD elements remain visible through the semi-transparent overlay
