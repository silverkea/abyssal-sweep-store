## Why

The submarine sprite appears too small relative to other game entities, reducing its visual
presence and making it harder to perceive during gameplay. Increasing its rendered size by
50% improves readability and makes the player entity feel more substantial in the game world.

## What Changes

- Submarine sprite rendered at 150% of its current width and height
- Submarine hitbox/collision bounds scaled to match the new visual size

## Capabilities

### New Capabilities
<!-- none -->

### Modified Capabilities
- `player-submarine`: Submarine display dimensions (sprite width and height) increase by 50%; hitbox updated to match
- `submarine-animated-sprite`: Sprite render size scaled up by 50% in both axes

## Impact

- Submarine entity rendering/sizing constants in the game code
- Collision detection bounds for the player submarine
