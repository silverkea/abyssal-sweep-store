## Context

The submarine sprite dimensions are hardcoded directly in `Submarine.ts` (`width = 56`, `height = 24`). The same literal values appear three times: sprite sizing (lines 26–27), the boundary clamp (`Math.max(28, ...)` uses half-width 28), and `getBounds()` which returns `Rectangle(x - 28, y - 12, 56, 24)`. There are no constants in `constants.ts` for these dimensions today.

See proposal.md for motivation.

## Goals / Non-Goals

**Goals:**
- Submarine sprite renders at 84×36 px (150% of 56×24)
- `getBounds()` hitbox reflects the new dimensions (84×36)
- Boundary clamp uses updated half-width (42)

**Non-Goals:**
- Repositioning the submarine's Y offset (`SEAFLOOR_Y - 16`) — visual tuning only if needed
- Changing the sprite assets themselves

## Decisions

**Extract dimensions to named constants in `constants.ts`**

The three occurrences of the magic numbers `56`, `24`, `28`, `12` in `Submarine.ts` are easy to miss when they need to stay in sync. Extracting `SUB_WIDTH` and `SUB_HEIGHT` constants gives a single source of truth for both the sprite size and the derived hitbox/clamp values.

Alternative considered: update the three inline literals directly without adding constants. Rejected — the half-width and half-height are derivatives of the sprite size; keeping them as inline literals makes the coupling invisible and fragile for future resizes.

## Risks / Trade-offs

- **Y-offset may feel off visually** at the larger size: the sub sits at `SEAFLOOR_Y - 16`, which places its center 16 px above the floor. At 36 px tall the bottom edge sits 2 px above the floor (18 − 16). This is acceptable but may warrant a follow-up visual tweak. → No mitigation needed for this change; note it for QA.
- **Boundary clamp half-width increases from 28 → 42**: the effective play area narrows by 14 px on each side. At 800 px wide this is ~3.5% per side — negligible.

## Migration Plan

No migration needed — pure constant/rendering change with no saved state or external API impact. Deploy replaces the previous build.
