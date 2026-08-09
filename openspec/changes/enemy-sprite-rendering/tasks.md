## 1. Constants

- [x] 1.1 Add `DEBRIS_ANIM_SPEED` constant to `src/constants.ts` alongside the existing `BARGE_ANIM_SPEED` and `SUB_ANIM_SPEED` constants

## 2. Frame Loading

- [x] 2.1 Add a single `import.meta.glob` call in `DebrisEntity.ts` using the pattern `*/{barrel,bottle,can,cup,oil}_frame*.png` (eager, as URL) to load all 10 enemy frame PNGs
- [x] 2.2 Build a module-level per-type frame URL lookup (`Record<string, string[]>`) by grouping the glob results by type name matched from each URL's filename

## 3. DebrisEntity Sprite Rendering

- [x] 3.1 Add a module-level `ROW_TYPES` constant array `['bottle', 'cup', 'can', 'barrel', 'oil']` indexed by row (0–4)
- [x] 3.2 Update the `DebrisEntity` constructor signature to accept `row: number` as a second parameter
- [x] 3.3 Replace the `Graphics` square rendering with an `AnimatedSprite` built from the per-type frame textures looked up by `ROW_TYPES[row]`
- [x] 3.4 Set the `AnimatedSprite` to `width = 36, height = 24`, assign `animationSpeed = DEBRIS_ANIM_SPEED`, and call `play()` to start the idle shimmer

## 4. DebrisGrid Integration

- [x] 4.1 Update `DebrisGrid` to pass the entity's `row` into each `DebrisEntity` constructor call

## 5. Verification

- [x] 5.1 Run the game and confirm all five debris rows display distinct sprite types (bottle, cup, can, barrel, oil top to bottom) with no coloured rectangles visible
- [x] 5.2 Confirm all entities shimmer with a 2-frame animation while the game is running
