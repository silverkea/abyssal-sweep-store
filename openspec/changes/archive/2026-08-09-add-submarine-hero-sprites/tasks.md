## 1. Constants

- [x] 1.1 Add `SUB_ANIM_SPEED` constant to `src/constants.ts`

## 2. Submarine Entity

- [x] 2.1 Add `import.meta.glob` frame loading for `sub_frame*.png` and export `subFrameUrls` from `src/entities/Submarine.ts`
- [x] 2.2 Replace the `Graphics` body and conning tower with a `AnimatedSprite` constructed from `subFrameUrls`
- [x] 2.3 Set sprite `width = 56`, `height = 24`, `anchor.set(0.5)`, `loop = true`, and call `.play()`
- [x] 2.4 Remove any remaining `Graphics`-related imports and declarations from `Submarine.ts`

## 3. Verification

- [x] 3.1 Run the game and confirm the submarine renders as an animated sprite (not a green ellipse)
- [x] 3.2 Confirm the animation loops continuously during play
- [x] 3.3 Confirm collision behavior is unchanged (submarine still takes damage on hits)
