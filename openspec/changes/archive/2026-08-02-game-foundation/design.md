## Context

Greenfield browser game — no existing codebase. See `proposal.md` for motivation. The tech stack is fixed: TypeScript, PixiJS v8, Vite. Target is 60 FPS on modern browsers via WebGL (WebGPU where available). The game is a single-page application with no server component.

## Goals / Non-Goals

**Goals:**
- Establish the Vite + TypeScript + PixiJS v8 project scaffold.
- Implement all six capabilities (player-submarine, debris-grid, coral-reef-barriers, cargo-barge, game-states, hud-and-scoring) to a fully playable first build.
- Keep rendering within a single PixiJS Application with layered Container hierarchy.
- Target 60 FPS via WebGL; WebGPU support via PixiJS auto-detection.

**Non-Goals:**
- Mobile / touch input support (keyboard-only for this phase).
- Online leaderboards or backend persistence (localStorage only).
- Full audio asset pipeline — programmatic Web Audio API tones acceptable.
- Advanced bioluminescent shaders (defer to a future visual polish change).
- Multiplayer or co-op.

## Decisions

### 1. PixiJS v8 as the rendering engine
PixiJS v8 targets WebGPU-first with automatic WebGL fallback, giving GPU-accelerated 2D at zero install cost. The ERASE blend mode for reef erosion is supported natively in v8's render pipeline.

**Alternatives considered:**
- Canvas 2D API: lower performance ceiling, no GPU acceleration for particles.
- Three.js: designed for 3D; overhead for a 2D arcade game is unjustified.

### 2. Vite as the build tool
Vite provides instant HMR, native ESM output, and TypeScript out of the box with minimal config. No custom webpack setup needed.

**Alternatives considered:**
- Webpack: slower cold start and HMR; more config to maintain.
- esbuild standalone: lacks HMR and plugin ecosystem.

### 3. Module architecture
`Engine` owns the single `PIXI.Application` instance and drives the ticker. `StateMachine` holds the active state object and delegates `update(dt)` and scene mounting/unmounting to state objects. Each state (e.g., `PlayingState`) composes the entities it needs. `InputManager` wraps `keydown`/`keyup` into a `Map<string, boolean>` polled each frame — no event callbacks scattered across entities.

Container layer order (bottom → top):
1. Background (ocean gradient / parallax)
2. Coral Reef Barriers
3. Debris Grid
4. Projectiles (bubble pulses, sludge drips)
5. Eco-Submarine
6. Cargo Barge
7. Particles
8. HUD

### 4. Entity design
Each entity class owns a `PIXI.Container` added to its layer. Entities expose `update(dt: number)` called by the active state each tick. Collision detection is AABB (axis-aligned bounding box) checked in the `PlayingState.update` loop — no physics engine needed.

### 5. Coral Reef erosion via RenderTexture + ERASE blend mode
Each barrier holds a `PIXI.RenderTexture`. The initial barrier sprite is drawn onto it once. On each hit, a circle sprite with `blendMode = 'erase'` is rendered at the impact point, punching a hole in the texture. This gives per-pixel granular erosion without sprite-sheet complexity.

**Alternatives considered:**
- Sprite sheet damage frames: coarser damage granularity; fixed hit positions.
- HTML Canvas overlay: loses PixiJS batch rendering benefits.

### 6. Particle system: manual object pool
A `ParticlePool` class pre-allocates a fixed array of `PIXI.Sprite` instances. On emission, a pooled sprite is activated at the spawn position with velocity and lifetime; on expiry it is returned to the pool. This avoids GC pressure during intensive debris destruction moments.

**Alternatives considered:**
- `PIXI.ParticleContainer`: optimized for uniform sprites; inflexible for mixed bubble/debris fragment types.
- Dynamic allocation: GC pauses risk dropped frames during large explosions.

### 7. Sound: Web Audio API with programmatic tones
`SoundManager` wraps `AudioContext` directly. Sonar pings are synthesized oscillator pulses (sine wave, short envelope). Underwater ambience is low-frequency filtered noise. No external audio files needed for the initial build, eliminating asset pipeline complexity.

**Alternatives considered:**
- Howler.js: adds a dependency for needs that the native API handles cleanly.
- HTML5 `<audio>`: no programmatic synthesis; requires asset files.

### 8. High score storage: localStorage
`localStorage.getItem('abyssal-sweep:high-score')` / `setItem(...)`. Simple, synchronous, requires no backend. Sufficient for a single-player browser game.

## Risks / Trade-offs

- **[PixiJS v8 API instability]** v8 was a major rewrite; some APIs differ from v7 documentation found online. → Pin to a specific v8 minor in `package.json`; read v8 migration docs before writing entity code.
- **[RenderTexture ERASE blend mode compatibility]** ERASE may behave differently across WebGL implementations or degrade on older GPUs. → Test on Chrome, Firefox, and Safari early in development; prepare a fallback (opaque damage-frame swap) if needed.
- **[Single-shot constraint tuning]** One active bubble forces players to be deliberate; feel depends heavily on bubble travel speed. → Expose `BUBBLE_SPEED` as a named constant for rapid tuning without touching entity logic.
- **[Debris grid speed scaling balance]** Linear speed scaling from 55→1 debris can produce an unplayable final entity. → Use a capped curve (e.g., `Math.min(baseSpeed + k * (55 - remaining), maxSpeed)`) with `maxSpeed` as a named constant.
- **[Audio context autoplay policy]** Browsers block `AudioContext` until a user gesture. → Resume `AudioContext` inside the first `pointerdown` or `keydown` handler; no audio before that gesture.

## Open Questions

- Should the Cargo Barge award a fixed bonus or a randomly selected value from a tiered range? The spec leaves the exact point value open — confirm before implementing `CargoBarge`.
- What is the exact vertical drop distance per edge bounce for the debris grid? This affects pacing significantly — define as a named constant and tune during playtesting.
