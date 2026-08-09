## 1. Update CargoBarge dimensions

- [x] 1.1 Set sprite `width` to `144` and `height` to `48` in `CargoBarge` constructor
- [x] 1.2 Update `getBounds()` to return `Rectangle(x - 72, y - 24, 144, 48)`
- [x] 1.3 Update entry/exit offsets from `±40` to `±72` in the constructor and `update()` method

## 2. Verify

- [x] 2.1 Run the game and confirm the barge appears at double its previous size
- [x] 2.2 Confirm collision detection still works (sonar bubble destroys the barge on hit)
- [x] 2.3 Confirm the barge slides fully off-screen before disappearing on both sides
