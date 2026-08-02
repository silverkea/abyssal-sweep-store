## Why

The ocean arcade genre has no modern browser-native entry that pairs classic Space Invaders mechanics with an environmental narrative. Abyssal Sweep: Deep Sea Cleanup delivers that experience — a PixiJS v8 WebGL/WebGPU game running at 60 FPS in the browser with zero install friction, establishing the full game foundation from project scaffold to playable loop.

## What Changes

- Introduce the complete Abyssal Sweep game as a new TypeScript + PixiJS v8 + Vite project.
- Implement the Eco-Submarine player entity with horizontal movement and single-shot sonar bubble firing.
- Implement the Sinking Debris Grid: a 5×11 invader fleet with tiered scoring, synchronized drift, edge-bounce drops, and speed scaling.
- Implement periodic toxic sludge drips fired by Chemical Drum debris in the bottom row.
- Implement four Coral Reef Barrier structures with dynamic erosion via PixiJS RenderTexture masking.
- Implement the Cargo Barge bonus entity that randomly traverses the ocean surface.
- Implement a five-state game state machine (START_MENU, PLAYING, PAUSED, GAME_OVER, LEVEL_COMPLETE).
- Implement a semi-transparent HUD displaying score, high score (localStorage), reef health, and level.
- Implement Web Audio API sound manager for sonar pings and underwater ambience.
- Implement a pooled particle system for bubble trails and debris fragments.

## Capabilities

### New Capabilities

- `player-submarine`: Eco-Submarine player entity — horizontal keyboard movement, single-active sonar bubble pulse firing, and three-integrity-point life system.
- `debris-grid`: Sinking Debris Grid — 5×11 tiered formation, synchronized horizontal drift with downward drops on edge collision, per-tier scoring, speed scaling by remaining count, and periodic toxic sludge drip projectiles from bottom-row Chemical Drums.
- `coral-reef-barriers`: Four Coral Reef Barrier shields with dynamic hit erosion implemented via PixiJS RenderTexture mask erasing, bleached visually as they absorb sludge drips and stray bubble charges.
- `cargo-barge`: Cargo Barge bonus entity that spawns randomly every 15–30 seconds at the ocean surface, drifts horizontally across the screen, and awards bonus points when hit.
- `game-states`: Five-state game state machine (START_MENU, PLAYING, PAUSED, GAME_OVER, LEVEL_COMPLETE) driving the PixiJS application lifecycle and scene transitions.
- `hud-and-scoring`: Semi-transparent HUD displaying Cleaned Debris Score, High Score (persisted via localStorage), Reef Health, and Level indicator.

### Modified Capabilities

*(none — this is a greenfield project)*

## Impact

- **New project:** `abyssal-sweep/` Vite + TypeScript workspace (no existing code affected).
- **Dependencies introduced:** PixiJS v8, Vite, TypeScript, Web Audio API (browser-native).
- **Architecture modules created:** `Engine`, `StateMachine`, `InputManager`, `Entities` (Submarine, DebrisGrid, CoralReef, BubblePulse, CargoBarge), `ParticlePool`, `SoundManager`.
- **Browser APIs used:** WebGL/WebGPU (via PixiJS), Web Audio API, localStorage (high score).
