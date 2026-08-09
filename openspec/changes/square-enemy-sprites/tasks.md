## 1. Update Sprite Dimensions

- [ ] 1.1 In `src/entities/DebrisEntity.ts`, change `sprite.width` from `36` to `32`
- [ ] 1.2 In `src/entities/DebrisEntity.ts`, change `sprite.height` from `24` to `32`

## 2. Update Collision Hitbox

- [ ] 2.1 In `DebrisEntity.getBounds()`, update the `Rectangle` to use `32×32` dimensions (offset: `−16, −16` from center instead of `−18, −12`)

## 3. Visual Verification

- [ ] 3.1 Run the game and confirm debris entities render as squares with correct sprite artwork
- [ ] 3.2 Confirm no vertical overflow between adjacent grid rows (entities should be flush but not overlapping)
- [ ] 3.3 Confirm bubble pulses collide with debris entities at the expected hit zone
