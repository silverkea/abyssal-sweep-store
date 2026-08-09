## Context

`Submarine.ts` currently draws the hero using Pixi.js `Graphics` (an ellipse body + rectangle conning tower). Two sprite frames (`sub_frame1.png`, `sub_frame2.png`) already exist in `src/assets/images/`. The `CargoBarge` entity uses an identical load-and-animate pattern with `import.meta.glob` + `AnimatedSprite` and serves as the reference implementation.

See proposal.md for motivation.

## Goals / Non-Goals

**Goals:**
- Replace Graphics drawing with a looping `AnimatedSprite` in `Submarine.ts`
- Match the barge's frame-loading pattern for consistency

**Non-Goals:**
- Adding new sprite frames (use the two that exist)
- Changing submarine movement, input handling, or collision bounds
- Adjusting animation speed post-ship (a constant can be tuned later)

## Decisions

**Follow the CargoBarge pattern exactly.**
Load frames with `import.meta.glob('../assets/images/sub_frame*.png', { eager: true, as: 'url' })`, sort keys, map to `Texture.from()`, construct `AnimatedSprite`, set `loop = true`, `anchor.set(0.5)`, and call `.play()`. Alternative considered: manual `PIXI.Assets.load` calls — rejected because glob is already the project convention and handles any future frame additions automatically.

**Sprite size: match current collision rect (56×24 px).**
The playing state computes collision as `Rectangle(x - 28, y - 12, 56, 24)`. Setting `sprite.width = 56` and `sprite.height = 24` keeps collision and visuals consistent without any changes to `PlayingState.ts`. If the sprite art requires a different aspect ratio this can be adjusted, but collision bounds would need to change in sync.

**Animation speed: introduce `SUB_ANIM_SPEED` constant in `constants.ts`.**
With only two frames a low speed (e.g. `0.08`) gives a slow propeller-style pulse. Can be tuned without touching entity code.

## Risks / Trade-offs

- **Only 2 frames** — animation will be a simple two-frame toggle. Visually minimal but correct; more frames can be added later without code changes.
- **Texture preloading** — `Texture.from()` with eager glob URLs works at runtime the same way the barge does; no additional preload step needed.
