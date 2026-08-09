## ADDED Requirements

### Requirement: Submarine sprite rendered at 150% baseline dimensions
The submarine sprite SHALL be rendered at 150% of its baseline pixel dimensions in both
width and height. The visual size of the submarine on screen SHALL be 1.5× larger than
the original unscaled sprite dimensions.

#### Scenario: Sprite width at 150% of baseline
- **WHEN** the submarine sprite is rendered during gameplay
- **THEN** its displayed width is 1.5 times the source asset's pixel width

#### Scenario: Sprite height at 150% of baseline
- **WHEN** the submarine sprite is rendered during gameplay
- **THEN** its displayed height is 1.5 times the source asset's pixel height
