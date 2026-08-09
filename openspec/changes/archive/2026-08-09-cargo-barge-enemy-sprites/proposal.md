## Why

The Cargo Barge currently has no defined visual representation — it renders as a placeholder while traversing the ocean surface. The `assets/images/barge_*.png` sprite sheet frames exist and should be wired up as the animated sprite for the barge, giving the bonus entity a polished, animated appearance that matches the game's aesthetic.

## What Changes

- Load the `assets/images/barge_*.png` frame sequence as a PixiJS `AnimatedSprite` for the Cargo Barge entity.
- Cycle through all barge frames in order at a fixed animation speed while the barge is on screen.
- Replace any placeholder graphic currently used for the barge with the animated sprite.

## Capabilities

### New Capabilities

*(none)*

### Modified Capabilities

- `cargo-barge`: Add a visual animation requirement — the barge MUST render as an animated sprite using the `assets/images/barge_*.png` frame sequence while it traverses the screen.

## Impact

- `src/entities/CargoBarge.ts` (or equivalent): swap placeholder graphic for `PIXI.AnimatedSprite` driven by the barge frame textures.
- `assets/images/barge_*.png`: these image files must be present and loadable via PixiJS asset loading.
- No changes to barge behavior, scoring, or spawn logic.
