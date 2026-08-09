## Context

`DebrisEntity` currently renders via a PixiJS `Graphics` rect filled with a tier-based colour. `Submarine` and `CargoBarge` already use `AnimatedSprite` with Vite's `import.meta.glob` (eager, as URL) to resolve frame paths at build time and create textures with `Texture.from(url)`. There is no explicit asset preload step — PixiJS handles texture loading on first access through its internal cache. All 10 enemy frame PNGs already exist in `src/assets/images/`.

## Goals / Non-Goals

**Goals:**
- Replace the `Graphics` square in `DebrisEntity` with an `AnimatedSprite` using the 2-frame assets for its assigned type
- Map each of the 5 rows to a distinct enemy sprite type (bottle → cup → can → barrel → oil, top to bottom)
- Cycle frames at a consistent rate so entities shimmer without any external coordination

**Non-Goals:**
- No hitbox dimension changes — the existing 36×24 bounds are preserved to keep gameplay balance unchanged
- No changes to tier assignment, scoring, sludge firing, or any other grid logic
- No new image assets

## Decisions

### Follow the existing `import.meta.glob` + `AnimatedSprite` pattern
`Submarine` and `CargoBarge` already demonstrate this pattern cleanly in the same codebase. Repeating it for debris entities keeps the approach consistent and avoids introducing a second asset-loading mechanism.

A single broad glob (`*/{barrel,bottle,can,cup,oil}_frame*.png`) loads all 10 frames. The resulting URL map is split into per-type arrays by matching the type name in each URL's filename, then stored in a module-level lookup keyed by type string. `DebrisEntity` looks up its type at construction time.

Alternative considered: one glob per type (five separate `import.meta.glob` calls). Rejected as repetitive; the single-glob-plus-grouping approach is concise and equally type-safe.

### Row-to-type mapping lives in `DebrisEntity`
A module-level constant array `['bottle', 'cup', 'can', 'barrel', 'oil']` indexed by row (0–4) maps each row to its sprite type. `DebrisGrid` already passes `row` into entity construction (via `tierForRow`); the constructor signature becomes `(tier: number, row: number)` so `DebrisEntity` can self-select its frames.

Alternative considered: pass the type string from `DebrisGrid`. Rejected — the mapping is presentation logic that belongs alongside the sprite assets in `DebrisEntity`, not in the grid.

### Sprite scaled to fit within existing 36×24 hitbox
Sprites are set to `width = 36, height = 24` on the `AnimatedSprite` (same dimensions as the current `Graphics` rect) so `getBounds()` needs no change. This keeps collision behaviour identical.

### `DEBRIS_ANIM_SPEED` constant — separate from barge and sub
The existing `BARGE_ANIM_SPEED = 0.15` and `SUB_ANIM_SPEED = 0.08` are tuned independently. Debris needs its own constant (`DEBRIS_ANIM_SPEED`) in `constants.ts` so the idle shimmer rate can be adjusted without coupling it to the barge or submarine.

### Natural sync via shared start time
All `AnimatedSprite` instances call `play()` at construction. Because they all start at frame 0 with the same `animationSpeed`, they stay in phase automatically — no shared clock or explicit sync mechanism is needed. PixiJS ticks them all from the same application ticker.

## Risks / Trade-offs

- **10 additional texture loads at grid construction** → not a practical concern; the images are small pixel-art PNGs and PixiJS caches them after first load. If the grid is reset mid-game, `Texture.from(url)` returns the cached texture immediately.
- **Sprites constrained to 36×24 may look small relative to the cell (48×32)** → this leaves breathing room between entities and is consistent with the current placeholder size. Visual spacing can be re-evaluated in a follow-up without any spec or hitbox changes.
