## 1. Project Scaffold

- [x] 1.1 Initialize Vite + TypeScript project with `npm create vite@latest` (vanilla-ts template)
- [x] 1.2 Install PixiJS v8 and pin to a specific minor version in `package.json`
- [x] 1.3 Configure TypeScript strict mode in `tsconfig.json`
- [x] 1.4 Create `src/` directory structure: `engine/`, `entities/`, `states/`, `systems/`, `ui/`
- [x] 1.5 Implement `Engine` class: own `PIXI.Application` instance, initialize canvas, append to DOM, start ticker

## 2. Core Systems

- [x] 2.1 Implement `InputManager`: `keydown`/`keyup` listeners populating a `Map<string, boolean>`, expose `isDown(key: string): boolean`
- [x] 2.2 Implement `StateMachine`: state registration, `transition(stateId)`, delegate `update(dt)` and scene mount/unmount to active state
- [x] 2.3 Implement `SoundManager`: wrap `AudioContext`, resume on first user gesture, implement `playSonarPing()` (sine oscillator, short envelope), implement `playAmbience()` (low-frequency filtered noise)
- [x] 2.4 Implement `ParticlePool`: pre-allocate fixed array of `PIXI.Sprite` instances, expose `emit(position, velocity, lifetime)`, `update(dt)` advances and recycles expired particles

## 3. Game State Machine

- [x] 3.1 Define `IGameState` interface: `onEnter()`, `onExit()`, `update(dt: number)`, `scene: PIXI.Container`
- [x] 3.2 Implement `StartMenuState`: render title, stored high score, start prompt; transition to PLAYING on start action
- [x] 3.3 Implement `PlayingState` shell: compose and own all game entities, delegate `update(dt)` to each
- [x] 3.4 Implement `PausedState`: suspend entity updates, display pause overlay, resume to PLAYING on resume action
- [x] 3.5 Implement `GameOverState`: stop all entity activity, display final score and high score, transition to PLAYING on restart
- [x] 3.6 Implement `LevelCompleteState`: display level-complete screen, increment level counter, reset entities, transition to PLAYING

## 4. Player Submarine

- [x] 4.1 Implement `Submarine` class with `PIXI.Container`, sprite placeholder, horizontal movement driven by `InputManager`; clamp to screen boundaries
- [x] 4.2 Implement `BubblePulse` class: upward travel at `BUBBLE_SPEED` constant, destroy on reaching top boundary
- [x] 4.3 Integrate single-shot constraint in `PlayingState`: spawn `BubblePulse` only when no instance is currently active; Spacebar otherwise ignored
- [x] 4.4 Implement submarine integrity tracking (start at 3); on zero trigger `StateMachine.transition('GAME_OVER')`

## 5. Debris Grid

- [x] 5.1 Implement `DebrisGrid` class: build 5×11 entity array, assign tiers (Tier 1 top rows = 30 pts, Tier 2 middle = 20 pts, Tier 3 bottom rows = 10 pts), lay out initial positions
- [x] 5.2 Implement synchronized horizontal drift: move entire grid as one unit each frame at current speed
- [x] 5.3 Implement edge detection: when rightmost/leftmost living entity hits boundary, drop grid by `GRID_DROP_STEP` constant and reverse direction
- [x] 5.4 Implement speed scaling: `speed = Math.min(BASE_GRID_SPEED + SPEED_SCALE_K * (TOTAL_DEBRIS - remaining), MAX_GRID_SPEED)` — define all three as named constants
- [x] 5.5 Implement `SludgeDrip` class: downward travel, destroy on reaching bottom boundary
- [x] 5.6 Implement periodic sludge drip firing: each Chemical Drum in the bottom occupied row fires on a random per-entity interval during PLAYING state
- [x] 5.7 Implement seafloor boundary check: if any debris entity reaches seafloor Y, trigger `StateMachine.transition('GAME_OVER')`
- [x] 5.8 Implement last-debris check: when `remaining === 0`, trigger `StateMachine.transition('LEVEL_COMPLETE')`

## 6. Coral Reef Barriers

- [x] 6.1 Implement `CoralReefBarrier` class: draw initial sprite onto a `PIXI.RenderTexture`; use that texture as the barrier's display object
- [x] 6.2 Implement hit erosion: on each projectile impact, render a circle with `blendMode = 'erase'` at the impact coordinate on the barrier's `RenderTexture`
- [x] 6.3 Implement visual bleaching: increase the barrier container's tint toward white in proportion to cumulative hit count
- [x] 6.4 Implement barrier destruction: sample a damage counter per barrier; when it exceeds `BARRIER_MAX_HITS` constant, remove the container from the scene
- [x] 6.5 Place four `CoralReefBarrier` instances evenly spaced across screen width in `PlayingState.onEnter()`

## 7. Cargo Barge

- [x] 7.1 Implement `CargoBarge` class: spawn at left or right screen edge at ocean-surface Y, drift horizontally at constant speed, destroy on reaching opposite edge
- [x] 7.2 Implement random spawn timer in `PlayingState`: schedule next spawn between 15–30 seconds after previous barge exits or is destroyed; enforce single-barge limit
- [x] 7.3 On barge exit (no hit): remove instance, reset spawn timer
- [x] 7.4 On barge hit: destroy barge, award `CARGO_BARGE_POINTS` constant to score, reset spawn timer

## 8. HUD & Scoring

- [x] 8.1 Implement `HUD` class: `PIXI.Container` with semi-transparent background panel added as top layer
- [x] 8.2 Add score `PIXI.Text` label to HUD; expose `setScore(n: number)` — call from `PlayingState` whenever points are awarded
- [x] 8.3 Add high score `PIXI.Text` label; read initial value via `localStorage.getItem('abyssal-sweep:high-score')`; update via `localStorage.setItem(...)` in `GameOverState` when current score exceeds stored value
- [x] 8.4 Add Reef Health indicator: derive percentage from `(sum of remaining barrier hit budgets) / (4 * BARRIER_MAX_HITS)`; update after each barrier erosion event
- [x] 8.5 Add level `PIXI.Text` label; update in `LevelCompleteState` before advancing to PLAYING

## 9. Collision Detection

- [x] 9.1 Implement `aabbOverlap(a: PIXI.Rectangle, b: PIXI.Rectangle): boolean` utility function
- [x] 9.2 Wire `BubblePulse` vs `DebrisGrid` in `PlayingState.update`: on overlap, remove debris entity, award tier points, destroy pulse
- [x] 9.3 Wire `BubblePulse` vs `CoralReefBarrier` in `PlayingState.update`: on overlap, call barrier erase at impact point, destroy pulse
- [x] 9.4 Wire `BubblePulse` vs `CargoBarge` in `PlayingState.update`: on overlap, destroy barge, award bonus, destroy pulse
- [x] 9.5 Wire `SludgeDrip` vs `Submarine` in `PlayingState.update`: on overlap, decrement submarine integrity, destroy drip
- [x] 9.6 Wire `SludgeDrip` vs `CoralReefBarrier` in `PlayingState.update`: on overlap, call barrier erase at impact point, destroy drip

## 10. Particle Effects

- [x] 10.1 Emit bubble trail particles from active `BubblePulse` position each frame via `ParticlePool`
- [x] 10.2 Emit debris fragment particles at debris entity position on neutralization via `ParticlePool`
- [x] 10.3 Call `ParticlePool.update(dt)` each frame to advance lifetime, position, and fade alpha of active particles

## 11. Container Layer Setup

- [x] 11.1 In `PlayingState.onEnter()`, assemble PixiJS container stack in order: Background → Coral Reef Barriers → Debris Grid → Projectiles → Eco-Submarine → Cargo Barge → Particles → HUD

## 12. Integration & Verification

- [x] 12.1 Wire `SoundManager`: call `playSonarPing()` on bubble fire, play impact tone on hit, play destruction tone on debris neutralized
- [x] 12.2 Verify `AudioContext` is resumed inside the first `keydown` handler before any sound is triggered
- [x] 12.3 Validate single-shot constraint: rapid Spacebar presses never produce more than one active `BubblePulse`
- [x] 12.4 Validate localStorage high score round-trip: score persists across page refreshes and updates correctly when beaten
- [x] 12.5 Profile frame rate under full 55-debris grid with active particles on Chrome, Firefox, and Safari — confirm 60 FPS target is met
