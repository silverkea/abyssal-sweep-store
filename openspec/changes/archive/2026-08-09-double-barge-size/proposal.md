## Why

The cargo barge sprite is rendered at 72×24 px, which makes it feel too small and hard to notice as it crosses the screen. Doubling the visual size makes the barge a more prominent and satisfying target.

## What Changes

- The `CargoBarge` sprite dimensions are changed from `72×24` to `144×48` (2× in both axes)
- The collision bounds returned by `getBounds()` are updated from `72×24` to `144×48` to stay in sync with the new sprite size
- Entry/exit offsets in the constructor (`-40` / `SCREEN_WIDTH + 40`) and in `update()` are increased to `(-72` / `SCREEN_WIDTH + 72`) so the larger barge still slides fully on- and off-screen before appearing or disappearing

## Capabilities

### New Capabilities
<!-- None -->

### Modified Capabilities
<!-- No spec-level behavioral requirements are changing. The existing cargo-barge spec
     requires the sprite to match the hitbox extent, which still holds after this
     change. Absolute pixel dimensions are not mandated by any spec. -->

## Impact

- `src/entities/CargoBarge.ts` — sprite width/height and `getBounds()` hardcoded values
- No constants, no other files affected
