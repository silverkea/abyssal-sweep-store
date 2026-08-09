## Context

Debris entities are rendered via PixiJS `AnimatedSprite`. Display size is set by assigning `sprite.width` and `sprite.height` directly after construction. The collision hitbox is returned from `getBounds()` as a manually computed `Rectangle` using the same dimensions. Both values are currently hardcoded in `DebrisEntity.ts` as 36×24 (width×height).

Grid cells are 48×32px. At 36×24, entities had 12px horizontal padding and 8px vertical padding per cell. At 32×32, horizontal padding becomes 16px and vertical padding drops to 0 — the sprite fills the full cell height.

## Goals / Non-Goals

**Goals:**
- Change sprite display size to 32×32 in `DebrisEntity.ts`
- Update `getBounds()` hitbox to 32×32 to match

**Non-Goals:**
- Changing grid cell dimensions (48×32 stays as-is)
- Adjusting row/column spacing or grid layout
- Modifying animation frame count, speed, or asset files

## Decisions

**32×32 as the target size**
The grid cell height is 32px, making 32×32 the largest square that fits without overflow. Alternatives:
- 36×36: would overflow the 32px cell height — rejected
- 24×24: fits with 4px vertical padding but reduces visual presence unnecessarily — rejected

**Keep sprite dimensions and hitbox in sync**
`getBounds()` will be updated to use the same 32×32 values. A mismatch between display and hitbox would create invisible dead zones or phantom hit areas.

## Risks / Trade-offs

- **Collision feel changes**: Horizontal hitbox shrinks 36→32px (−4px); vertical hitbox grows 24→32px (+8px). Shots that graze the side of a sprite will now miss more easily; shots that clip the top or bottom will now connect more often. Net effect is minor but noticeable if playtesting reveals it.  
  → Mitigation: accept as intended — hitbox now honestly reflects the visible sprite.

- **Zero vertical cell padding**: Entities fill the full cell height (32/32px). Adjacent rows will appear flush with no gap between sprite edges. Whether this looks correct depends on the updated artwork.  
  → Mitigation: visual review during implementation; if gaps are needed, increase cell height rather than shrinking the sprite.

## Migration Plan

Single-file change to `src/entities/DebrisEntity.ts`. No data migration, no asset changes, no API changes. Rollback by reverting the file.
