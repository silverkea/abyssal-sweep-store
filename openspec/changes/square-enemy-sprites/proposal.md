## Why

Enemy source images have been updated to square dimensions, but the display size remains rectangular (36×24px), causing the sprites to render as stretched/squashed versions of the new artwork. Adjusting the display size to square preserves the intended proportions of the updated images.

## What Changes

- Enemy sprite display dimensions change from 36×24px to 32×32px (square, fits within the 48×32px grid cell)
- Enemy hitbox updated from 36×24px to 32×32px to match the new display size

## Capabilities

### New Capabilities

_(none)_

### Modified Capabilities

- `enemy-sprites`: Enemy display dimensions change from rectangular (36×24) to square (32×32) to match updated source artwork
- `debris-grid`: Grid cell spacing may need review to ensure 32×32 enemies have appropriate visual padding within 48×32 cells

## Impact

- `src/entities/DebrisEntity.ts` — `sprite.width`, `sprite.height`, and `getBounds()` hitbox values
- Visual layout of the debris grid — enemies will appear taller (24→32px) within their cells; vertical padding within cells reduces from 4px to 0px, so grid row height or cell size may need adjustment
