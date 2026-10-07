# Layout Engine API (svg_layout.py)

Programmatic SVG generation with automatic layout calculation. All coordinates are computed, never hardcoded.

## Installation

Copy `svg_layout.py` to your project's `_scripts/` directory.

```python
from svg_layout import *
```

## Core Classes

### Doc

The document container. Manages elements and computes viewBox automatically.

```python
doc = Doc(title="Diagram Title", desc="One-line description")
doc.add(element)
doc.add_arrow(arrow)
doc.save('output.svg')  # Auto-validates after saving
```

### Box

A labeled rectangle with optional subtitle.

```python
box = make_box(
    title="Box Title",
    subtitle="Optional subtitle",  # or None for single-line
    w=200,  # optional, auto-calculated if omitted
    h=64,   # optional, auto-calculated if omitted
    color='blue',  # ramp name
    level=100,     # color level (50/100/400/600/800/900)
    rx=8           # corner radius
)
```

### Container

Wraps child elements with automatic padding.

```python
container = make_container(
    children=[box1, box2, box3],
    padding=20,
    color='blue',
    level=50,
    rx=10,
    title="Container Title"  # optional
)
```

### Arrow

Connects two points with an arrowhead.

```python
arrow = Arrow(x1=100, y1=50, x2=200, y2=50, stroke='#378ADD')
doc.add_arrow(arrow)
```

## Layout Functions

### Vertical Layout

Stack elements vertically with consistent spacing.

```python
layout_vertical(
    elements=[box1, box2, box3],
    start_x=40,
    start_y=40,
    gap=12,
    align='left'  # or 'center'
)
```

### Horizontal Layout

Arrange elements in a row.

```python
layout_horizontal(
    elements=[box1, box2, box3],
    start_x=40,
    start_y=40,
    gap=12,
    align='top'  # or 'center'
)
```

### Grid Layout

Arrange elements in a grid.

```python
layout_grid(
    elements=[box1, box2, box3, box4],
    cols=2,
    start_x=40,
    start_y=40,
    col_gap=12,
    row_gap=12
)
```

## Example: Complete Diagram

```python
from svg_layout import *

# Create document
doc = Doc("Product Chain", "Four-stage process flow")

# Create boxes
box1 = make_box("Monitor", "Collect data", color='blue', level=100)
box2 = make_box("Close Loop", "Manage process", color='blue', level=100)
box3 = make_box("Assess", "Enable exemption", color='blue', level=100)
box4 = make_box("Guarantee", "Dare to compensate", color='blue', level=400)

# Layout horizontally
layout_horizontal([box1, box2, box3, box4], start_x=48, start_y=60, gap=24)

# Add to document
doc.add(box1)
doc.add(box2)
doc.add(box3)
doc.add(box4)

# Add arrows
doc.add_arrow(Arrow(box1.x + box1.w, box1.y + box1.h/2, box2.x, box2.y + box2.h/2))
doc.add_arrow(Arrow(box2.x + box2.w, box2.y + box2.h/2, box3.x, box3.y + box3.h/2))
doc.add_arrow(Arrow(box3.x + box3.w, box3.y + box3.h/2, box4.x, box4.y + box4.h/2))

# Save (auto-validates)
doc.save('output.svg')
```

## Validation

The `save()` method automatically runs boundary validation. If any element overflows the safe zone, it prints a warning.

For manual validation:

```bash
python3 _scripts/svg_boundary_check.py path/to/file.svg
```

## Limitations

- Text width estimation is approximate (14px per CJK character, 8px per Latin character)
- Complex multi-line text may need manual adjustment
- The engine does not handle text wrapping — keep titles short
