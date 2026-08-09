## 1. Constants

- [x] 1.1 Add `SUB_WIDTH = 84` and `SUB_HEIGHT = 36` constants to `src/constants.ts`

## 2. Submarine Entity

- [x] 2.1 Replace hardcoded `width = 56` / `height = 24` sprite sizing in `Submarine.ts` with `SUB_WIDTH` / `SUB_HEIGHT`
- [x] 2.2 Update the boundary clamp in `Submarine.update()` to use `SUB_WIDTH / 2` (42) instead of the hardcoded 28
- [x] 2.3 Update `getBounds()` in `Submarine.ts` to derive offsets and dimensions from `SUB_WIDTH` / `SUB_HEIGHT`

## 3. Verification

- [x] 3.1 Run the game and confirm the submarine sprite visually appears ~50% larger than before
- [x] 3.2 Confirm the submarine cannot move past the left or right screen edge at the new size
- [x] 3.3 Confirm sludge drip collision detection still triggers correctly against the larger hitbox
