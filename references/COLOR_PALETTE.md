# Color Palette Reference

## Full Ramp Table

Each ramp has 7 levels. Level meanings:
- **50**: Lightest fill — use for container backgrounds
- **100**: Light fill — use for node/box backgrounds
- **200**: Light-mid — rarely used, for subtle differentiation
- **400**: Mid-tone — use for strokes and accents
- **600**: Accent / dark stroke — use for emphasis
- **800**: Title text on light backgrounds
- **900**: Darkest text — use for maximum contrast

| Ramp | 50 | 100 | 200 | 400 | 600 | 800 | 900 |
|---|---|---|---|---|---|---|---|
| blue | #E6F1FB | #B5D4F4 | #85B7EB | #378ADD | #185FA5 | #042C53 | #0C447C |
| teal | #E1F5EE | #9FE1CB | #5DCAA5 | #1D9E75 | #0F6E56 | #04342C | #0F6E56 |
| gray | #F1EFE8 | #D3D1C7 | #B4B2A9 | #888780 | #5F5E5A | #2C2C2A | #444441 |
| amber | #FAEEDA | #FAC775 | #EF9F27 | #EF9F27 | #BA7517 | #412402 | #854F0B |
| red | #FCEBEB | #F7C1C1 | #F09595 | #E24B4A | #A32D2D | #501313 | #791F1F |
| coral | #FAECE7 | #F5C4B3 | #F0997B | #D85A30 | #993C1D | #4A1B0C | #712B13 |
| green | #EAF3DE | #C0DD97 | #97C459 | #639922 | #3B6D11 | #173404 | #27500A |
| pink | #FBEAF0 | #F4C0D1 | #ED93B1 | #D4537E | #993556 | #4B1528 | #72243E |
| purple | #EEEDFE | #CECBF6 | #AFA9EC | #7F77DD | #534AB7 | #26215C | #3C3489 |

## Usage Guidelines

### Single Diagram Constraint
Use **≤ 2 color ramps** per diagram. More than 2 creates visual noise.

### Common Combinations
- **Primary + neutral**: blue + gray (most common, professional)
- **Primary + accent**: blue + amber (highlight one element)
- **Status coding**: green (good) + amber (warning) + red (bad)
- **Hierarchy**: same ramp, different levels (800 for top, 600 for mid, 400 for bottom)

### Theme Compatibility
Since Obsidian SVGs must work in both light and dark themes:
- Use **light fills (50/100)** with **dark text (800/900)**
- This creates a "card" effect that reads well on any background
- Avoid dark fills — they will blend into dark theme backgrounds

### Accessibility
- Minimum contrast ratio: 4.5:1 for normal text
- 800/900 text on 50/100 fills typically exceeds 7:1
- Never use 400-level colors for text on 50-level fills (contrast too low)
