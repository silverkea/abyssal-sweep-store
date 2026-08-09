## Why

Debris enemies in the main gameplay screen are rendered as plain coloured squares, missing the purpose-built pixel-art assets (barrel, bottle, can, cup, oil) that already ship with the game. Switching to sprites adds visual identity, communicates the environmental theme, and brings the game in line with the animated submarine and cargo barge.

## What Changes

- Each row of the 5×11 debris grid renders with a distinct sprite type instead of a coloured square
- Each enemy type cycles between its two animation frames (idle shimmer effect)
- Sprite dimensions replace the current square placeholder as the logical hitbox for each debris entity

## Capabilities

### New Capabilities
- `enemy-sprites`: Visual rendering of debris grid entities using per-row sprite images with 2-frame animation cycling

### Modified Capabilities
- `debris-grid`: Add visual appearance requirements — row-to-sprite-type mapping and per-entity animation behaviour

## Impact

- `src/` — debris entity rendering component(s) and grid logic
- `src/assets/images/` — barrel, bottle, can, cup, oil frame PNGs (already present, no new assets needed)
- No changes to scoring, collision logic, sludge firing, or game state transitions
