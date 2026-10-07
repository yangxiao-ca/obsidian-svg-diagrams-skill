---
name: obsidian-svg-diagrams
description: >
  Generate publication-ready SVG diagrams for Obsidian vaults. Covers layout calculation, boundary safety, visual alignment, color palette, and validation.
  Use when the user asks to create diagrams, charts, or visual illustrations for Obsidian notes, or when delivering Markdown files that contain structural/logical content worth visualizing.
  触发词（中文）：给我画、画出来、画个图、画一张图、把这部分画出来、SVG图、画个SVG、生成图、配图、画个流程图、画个结构图、画个时间线、画个对照图、画个层级图、可视化、做个图、插图、画一下、帮我把这个画成图、用图表示、图形化、画成SVG、存成SVG、落库配图、画到md里
---

# Obsidian SVG Diagrams Skill

Generate standalone SVG files that render correctly in Obsidian reading view. Unlike inline widget SVGs (which rely on host CSS variables), Obsidian SVGs must be **fully self-contained** with hardcoded colors and explicit dimensions.

## When to Create a Diagram

Create a diagram whenever the Markdown section has any of these structural features:
- **Parallel items** (并列): multiple boxes at the same level
- **Hierarchy** (层级): parent-child relationships
- **Sequence** (时序): timeline, process flow
- **Causation** (因果): A leads to B leads to C
- **Exclusion** (排除): ruling out options
- **Comparison** (对照): side-by-side comparison
- **Formula chain** (公式链): input → transform → output

Do NOT create diagrams for pure prose paragraphs with no structural pattern.

## File Location

Store SVG files in the vault's `_assets/diagrams/` directory (or the vault's configured attachment folder). Reference them in Markdown using Obsidian wikilinks:

```markdown
![[diagram-name.svg|620]]
```

Place the reference line **immediately below** the section heading it illustrates.

## Hard Requirements (Obsidian Environment)

These are **non-negotiable**. Violating any one will cause rendering failures in Obsidian.

### 1. Colors Must Be Hardcoded

Obsidian does NOT have the CSS variables available in chat environments (`--color-text-primary`, etc.). Using CSS variables will render as black or blank.

**Every** `fill`, `stroke`, and text `fill` attribute must use a hex color value.

### 2. Self-Contained Styling

- **No** `<style>` blocks
- **No** HTML comments
- **No** gradients, drop shadows, blur, or glow effects
- **No** external font imports
- Design as "light fill + dark text" so it reads in both light and dark Obsidian themes

### 3. Root SVG Tag

```svg
<svg viewBox="0 0 680 H" width="100%" xmlns="http://www.w3.org/2000/svg"
     role="img" font-family="-apple-system,BlinkMacSystemFont,'PingFang SC','Hiragino Sans GB','Microsoft YaHei',sans-serif">
  <title>Descriptive title</title>
  <desc>One-sentence description for accessibility</desc>
  ...
</svg>
```

- `viewBox` width is **always 680** (never change this)
- `viewBox` height H = bottommost element y + height + 24 (20px safe margin + 4px buffer)
- `width="100%"` for responsive scaling
- Always include `role="img"`, `<title>`, and `<desc>`

### 4. Typography

| Property | Value |
|---|---|
| Font size | ≥ 11px; body text 12-14px |
| Font weight | Only 400 (regular) or 500 (medium). Never 600 or 700 |
| Line height | Use `dominant-baseline="central"` for vertical centering |
| Color systems | ≤ 2 color ramps per diagram |

## Layout Calculation Rules

All coordinates must be **calculated before drawing**. Never eyeball positions.

### Rule 1: Boundary Safety

Safe zone within viewBox 680 × H:
- Top: content y ≥ 40
- Bottom: content y + height ≤ H - 20
- Left: content x ≥ 40
- Right: content x + width ≤ 640

After generating the SVG, **always run the boundary check script**:

```bash
python3 _scripts/svg_boundary_check.py
```

The script must report 100% pass. If any element overflows, fix coordinates or run with `--fix` to auto-adjust viewBox height.

### Rule 2: Equal Sizing for Same-Level Elements

All boxes/nodes at the same hierarchy level must have **identical width and height**.

- 4 boxes in a row → all 4 must be the same width
- 5 team cards stacked → all 5 must be the same height
- Exception: only when content difference is extreme AND explicitly intentional

### Rule 3: Uniform Container Padding

Container padding must be **equal on all four sides** (typically 20px).

```
container_height = content_total_height + padding_top + padding_bottom
where padding_top == padding_bottom
```

Never have "24px top padding, 4px bottom padding" — this is the most common visual defect.

### Rule 4: Strict Text Centering

**Single-line text:**
```
y = box_y + box_height / 2
```
(works with `dominant-baseline="central"`)

**Multi-line text (as a block):**
```
text_block_height = sum of (line_height × 1.2) + gap × (n - 1)
offset = (box_height - text_block_height) / 2
first_line_y = box_y + offset + first_line_font_size / 2
subsequent_line_y = previous_y + previous_font_size × 1.2 + gap
```

Never let a text block sit visually off-center within its box.

### Rule 5: Horizontal Alignment

- Same row: all elements share the same y coordinate
- Same column: all elements share the same x coordinate (or same center-x)

## Color Palette

Use this palette consistently. Each ramp has 7 levels:

| Level | Meaning |
|---|---|
| 50 | Lightest fill (container backgrounds) |
| 100 | Light fill (node backgrounds) |
| 200 | Light-mid (rarely used) |
| 400 | Mid-tone (strokes, accents) |
| 600 | Accent / dark stroke |
| 800 | Title text on light background |
| 900 | Darkest text |

### Ramps

| Name | 50 | 100 | 400 | 600 | 800 | 900 |
|---|---|---|---|---|---|---|
| blue | #E6F1FB | #B5D4F4 | #378ADD | #185FA5 | #042C53 | #0C447C |
| teal | #E1F5EE | #9FE1CB | #1D9E75 | #0F6E56 | #04342C | #0F6E56 |
| gray | #F1EFE8 | #D3D1C7 | #888780 | #5F5E5A | #2C2C2A | #444441 |
| amber | #FAEEDA | #FAC775 | #EF9F27 | #BA7517 | #412402 | #854F0B |
| red | #FCEBEB | #F7C1C1 | #E24B4A | #A32D2D | #501313 | #791F1F |
| coral | #FAECE7 | #F5C4B3 | #D85A30 | #993C1D | #4A1B0C | #712B13 |
| green | #EAF3DE | #C0DD97 | #639922 | #3B6D11 | #173404 | #27500A |

### Theme Quick Pick

- **Light mode**: 50 fill + 600 stroke + 800 title / 600 subtitle
- **Dark mode**: 800 fill + 200 stroke + 100 title / 200 subtitle

Since Obsidian SVGs must work in both themes, prefer the "light fill + dark text" approach (50/100 fills with 800/900 text).

## Common Diagram Patterns

### Pattern 1: Stacked Rows (Timeline, Tier List)

```
Each row: rect(x=40, y=Y, width=600, height=H, rx=10)
Spacing between rows: 12px
Row height for 2-line text: 64px
Row height for 1-line text: 44px
```

### Pattern 2: Side-by-Side Columns

```
Content width: 600px (680 - 40 - 40)
For N columns: each_width = (600 - gap × (N-1)) / N
Common: 2 cols = 294px each (gap=12), 3 cols = 192px each (gap=12), 4 cols = 140px each (gap=20)
```

### Pattern 3: Flow with Arrows

```
Include arrow marker in <defs>:
<marker id="arrow" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse">
  <path d="M2 1L8 5L2 9" fill="none" stroke="context-stroke" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
</marker>

Arrow path: <path d="M{x1} {y1}L{x2} {y2}" fill="none" stroke="{color}" stroke-width="1.5" marker-end="url(#arrow)"/>
```

### Pattern 4: Container with Children

```
Container: rect with padding=20 on all sides
Children positioned at: container_x + 20, container_y + title_height + 12
Container height = children_total_height + 40 (20 top + 20 bottom)
```

## Validation Checklist

Before delivering the SVG, verify ALL of the following:

1. **XML validity**: Parse with `xml.dom.minidom` — no parse errors
2. **Boundary check**: Run `svg_boundary_check.py` — 100% pass
3. **Hardcoded colors**: No CSS variables, no `currentColor` (except in arrow marker stroke)
4. **Equal sizing**: Same-level elements have identical dimensions
5. **Uniform padding**: Container padding is equal on all sides
6. **Text centering**: Multi-line text blocks are visually centered
7. **Wikilink reference**: `![[filename.svg|620]]` is written in the correct MD location
8. **viewBox height**: H = max_y + 24 (at minimum)

## Scripts

### svg_boundary_check.py

Boundary validation script. Scans all SVG elements and checks they fall within the safe zone.

```bash
# Check all SVGs in default directory
python3 _scripts/svg_boundary_check.py

# Check a single file
python3 _scripts/svg_boundary_check.py path/to/file.svg

# Auto-fix viewBox height
python3 _scripts/svg_boundary_check.py --fix path/to/file.svg
```

Safe zone: top 40px, bottom 20px, left 40px, right 40px (within 680px width).

### svg_layout.py (Optional)

Python layout engine for programmatic SVG generation. All coordinates are computed, never hardcoded.

```python
from svg_layout import *

doc = Doc("Title", "Description")
box = make_box("Title", "Subtitle", color='blue', level=100)
doc.add(box)
doc.save('output.svg')
```

See [LAYOUT_ENGINE.md](references/LAYOUT_ENGINE.md) for the full API.

## References

- [LAYOUT_ENGINE.md](references/LAYOUT_ENGINE.md) — svg_layout.py API documentation
- [COLOR_PALETTE.md](references/COLOR_PALETTE.md) — Full color ramp table with usage guidelines
