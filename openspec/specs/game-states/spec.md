## Purpose

Defines the five-state game state machine that governs application lifecycle, scene transitions, and what interactions are permitted at each stage.

### Requirement: Five distinct game states
The game SHALL maintain exactly five states: START_MENU, PLAYING, PAUSED, GAME_OVER, and LEVEL_COMPLETE. Only one state SHALL be active at a time.

#### Scenario: Single active state
- **WHEN** the game is running
- **THEN** exactly one of the five states is active and no conflicting state logic executes simultaneously

### Requirement: START_MENU state
In the START_MENU state, the game SHALL display the game title, high score, and a prompt to begin play. Debris grid, submarine, and barriers SHALL NOT be active. Pressing the primary start action SHALL transition to PLAYING.

#### Scenario: Start game from menu
- **WHEN** the game is in START_MENU and the player presses the start action
- **THEN** the game transitions to PLAYING, initializes the debris grid, submarine, and barriers, and begins the game loop

### Requirement: PLAYING state
In the PLAYING state, all game entities (submarine, debris grid, coral reefs, cargo barge) SHALL be active and accepting input. The game loop SHALL update positions, check collisions, and update the HUD each frame.

#### Scenario: Entities active during play
- **WHEN** the game is in PLAYING state
- **THEN** the submarine responds to keyboard input, the debris grid drifts, and sludge drips are fired per their respective rules

#### Scenario: Pause during play
- **WHEN** the game is in PLAYING state and the player presses the pause action
- **THEN** the game transitions to PAUSED

### Requirement: PAUSED state
In the PAUSED state, all entity movement and projectile activity SHALL be suspended. A pause overlay SHALL be displayed. Pressing the resume action SHALL return to PLAYING.

#### Scenario: Game paused
- **WHEN** the game is in PAUSED state
- **THEN** no entity positions change, no projectiles move, and a pause indication is displayed

#### Scenario: Resume from pause
- **WHEN** the game is in PAUSED state and the player presses the resume action
- **THEN** the game transitions back to PLAYING and all entity activity resumes from its suspended positions

### Requirement: GAME_OVER state
The game SHALL enter GAME_OVER when the submarine's integrity reaches zero or the debris grid reaches the seafloor. A game-over screen SHALL display the final score and high score. The player SHALL be able to restart from this state.

#### Scenario: Display game over screen
- **WHEN** the game transitions to GAME_OVER
- **THEN** entity movement stops, a game-over overlay is displayed with the final score and current high score

#### Scenario: Restart from game over
- **WHEN** the game is in GAME_OVER and the player presses the restart action
- **THEN** the game resets all entities, score, and integrity, and transitions to PLAYING

### Requirement: LEVEL_COMPLETE state
The game SHALL enter LEVEL_COMPLETE when all debris entities in the grid are neutralized. A level-complete screen SHALL be displayed briefly before advancing to the next level in PLAYING state.

#### Scenario: Level complete triggered
- **WHEN** the last debris entity is destroyed
- **THEN** the game transitions to LEVEL_COMPLETE and displays a level-complete indication

#### Scenario: Advance to next level
- **WHEN** the game is in LEVEL_COMPLETE state and the advance action or timeout is reached
- **THEN** the level counter increments, the debris grid and barriers reset, and the game transitions to PLAYING
