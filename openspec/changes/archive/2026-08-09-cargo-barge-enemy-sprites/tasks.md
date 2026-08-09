## 1. Asset Verification

- [x] 1.1 Confirm all `assets/images/barge_*.png` files are present and check their naming convention (zero-padded vs. non-padded numbers)
- [x] 1.2 Verify the files load correctly in the browser via PixiJS `Assets.load` in isolation (quick smoke test or console check)

## 2. Asset Loading

- [x] 2.1 Register the `barge_*.png` frames in the project's asset preload list so they are available before the first barge spawns
- [x] 2.2 Enumerate the barge frame paths using `import.meta.glob` (Vite), sort alphabetically, and build a `Texture[]` array from the resolved paths

## 3. CargoBarge Entity Update

- [x] 3.1 Add `BARGE_ANIM_SPEED` named constant alongside existing barge constants
- [x] 3.2 Replace the placeholder graphic in `CargoBarge` with a `PIXI.AnimatedSprite` constructed from the barge texture array
- [x] 3.3 Set `animationSpeed` to `BARGE_ANIM_SPEED`, enable looping, and call `play()` when the barge becomes active
- [x] 3.4 Ensure the animated sprite is sized and positioned to match the barge's AABB collision bounds

## 4. Validation

- [x] 4.1 Run the game and confirm the barge animates smoothly while traversing the screen
- [x] 4.2 Confirm the barge disappears (sprite removed) correctly when destroyed by a hit or when it exits the screen
- [x] 4.3 Confirm no blank-sprite flash occurs on the first barge spawn (asset preloading working)
