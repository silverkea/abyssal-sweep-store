## Why

The submarine hero is currently rendered using Pixi.js Graphics primitives (an ellipse body and rectangle conning tower), while `sub_frame*.png` sprite assets already exist in the repo. Replacing the placeholder with an animated sprite brings the hero in line with the CargoBarge, which already uses this pattern.

## What Changes

- Remove the Graphics-based submarine drawing (ellipse + rectangle)
- Load `sub_frame*.png` frames via `import.meta.glob` (same pattern as `CargoBarge`)
- Render the submarine as a Pixi.js `AnimatedSprite` that loops through all frames
- Adjust collision bounds if the sprite dimensions differ from the current 56×24 px shape

## Capabilities

### New Capabilities
- `submarine-animated-sprite`: Submarine hero renders as a cycling `AnimatedSprite` using `sub_frame*.png` assets instead of Graphics primitives

### Modified Capabilities
<!-- No existing spec-level behavior changes — collision bounds and movement remain the same -->

## Impact

- `src/entities/Submarine.ts` — replace Graphics with AnimatedSprite; export frame URLs
- `src/assets/images/sub_frame*.png` — consumed as sprite frames (already present: sub_frame1.png, sub_frame2.png)
- No API, dependency, or state machine changes required
