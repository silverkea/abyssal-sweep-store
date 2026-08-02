## Purpose

Defines the behavior of the Cargo Barge — a high-risk bonus entity that periodically traverses the ocean surface and awards the player bonus points when neutralized.

## ADDED Requirements

### Requirement: Random spawn interval
A Cargo Barge SHALL spawn at a random interval between 15 and 30 seconds after the previous barge departed or was destroyed. No more than one Cargo Barge SHALL be present on screen at a time.

#### Scenario: Barge spawns after interval
- **WHEN** the random spawn timer elapses during the PLAYING state
- **THEN** a Cargo Barge appears at either the left or right edge of the ocean surface level and begins traversing horizontally

#### Scenario: No duplicate barges
- **WHEN** a Cargo Barge is currently active on screen
- **THEN** the spawn timer does not trigger another barge until the current one is gone

### Requirement: Horizontal traversal and exit
The Cargo Barge SHALL drift horizontally across the screen at a constant speed. When the barge reaches the opposite screen edge, it SHALL disappear and the next spawn interval SHALL begin.

#### Scenario: Barge exits screen
- **WHEN** the Cargo Barge reaches the far screen boundary opposite its spawn side
- **THEN** the barge is removed from the scene and a new spawn interval begins

### Requirement: Bonus points on hit
When the player's sonar bubble pulse strikes the Cargo Barge, the barge SHALL be destroyed and the player SHALL receive a bonus point reward.

#### Scenario: Player hits barge
- **WHEN** a sonar bubble pulse collides with the Cargo Barge
- **THEN** the barge is destroyed, bonus points are added to the player's score, and a new spawn interval begins

### Requirement: Barge not active during non-playing states
The Cargo Barge SHALL not spawn or remain active when the game is in any state other than PLAYING.

#### Scenario: Barge inactive outside play
- **WHEN** the game transitions out of the PLAYING state
- **THEN** any active Cargo Barge is removed and the spawn timer is suspended
