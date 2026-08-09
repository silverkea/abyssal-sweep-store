## Context

The Cargo Barge entity exists and handles spawning, traversal, hit detection, and scoring. It currently has no defined visual — this change wires up `assets/images/barge_*.png` as its animated display. The project uses PixiJS v8 and Vite; asset loading goes through PixiJS's `Assets` API.

## Goals / Non-Goals

**Goals:**
- Load the `barge_*.png` frames and drive them as a looping `PIXI.AnimatedSprite` on the `CargoBarge` entity.
- Keep the animation speed as a named constant for easy tuning.

**Non-Goals:**
- Changing any barge behavior (spawn interval, movement, scoring).
- Adding directional variants or flip logic based on spawn side (left vs. right).
- Introducing a new asset-loading pipeline — use the existing PixiJS `Assets` pattern already established in the project.

## Decisions

### 1. `PIXI.AnimatedSprite` over manual frame swapping
PixiJS v8's `AnimatedSprite` handles frame cycling, loop control, and `animationSpeed` natively. Manual frame swapping adds equivalent logic with no benefit.

**Alternatives considered:**
- Manual `tick`-driven frame swap on a plain `Sprite`: more code, same result.

### 2. Frame enumeration via sorted glob import (Vite)
Vite supports `import.meta.glob('assets/images/barge_*.png')` to enumerate all matching files at build time. Sort the resulting keys alphabetically (which gives `barge_0.png`, `barge_1.png`, … order) before constructing the texture array. This avoids hard-coding individual frame filenames.

**Alternatives considered:**
- Hard-code each frame path: breaks if frames are added or renamed.
- PixiJS spritesheet JSON: overkill for a small linear sequence; requires a separate build step.

### 3. Animation speed as a named constant
`BARGE_ANIM_SPEED` (in `animationSpeed` units — fractions of 60 FPS per tick) lives alongside other barge constants. The value can be tuned without touching entity logic.

## Risks / Trade-offs

- **[Frame sort order]** Alphabetic sort works correctly only if filenames use zero-padded numbers (e.g., `barge_00.png` not `barge_0.png` mixed with `barge_10.png`). → Verify naming convention of the actual PNG files before implementation; adjust sort or pad names if needed.
- **[Asset preloading]** If `barge_*.png` frames are not included in the initial asset bundle, the first barge spawn may show a blank sprite until textures resolve. → Register the barge frames in the preload list alongside other game assets.
