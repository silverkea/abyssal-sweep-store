## Context

See proposal.md for motivation. All barge dimensions are hardcoded in `src/entities/CargoBarge.ts` — there are no shared constants for barge width/height.

Current values:
- Sprite: `width = 72`, `height = 24`
- Hitbox (`getBounds`): `Rectangle(x - 36, y - 12, 72, 24)`
- Entry/exit offsets: `±40`

## Goals / Non-Goals

**Goals:**
- Double the rendered sprite and collision bounds to `144×48`
- Keep sprite and hitbox in sync (spec requirement)
- Keep the barge fully off-screen at spawn/despawn (adjust offsets)

**Non-Goals:**
- Extracting dimensions to constants (out of scope)
- Changing speed, spawn timing, or any other behavior

## Decisions

**Hardcode the new values directly** rather than computing them as `2 * old`. The values are already hardcoded; adding a multiplication layer would obscure the intent without adding flexibility. If constants are ever extracted, that's a separate refactor.

**Update entry/exit offsets from `±40` to `±72`** (half of new width) so the barge slides fully off-screen before the `alive` flag flips — same behaviour as today, scaled proportionally.

## Risks / Trade-offs

- None. Single-file change, no shared state, no external dependencies.
