# Constrained-Optimization-Visualizer

An interactive browser-based tool for visualizing **constrained optimization problems** in two variables. Define an objective function, add equality and inequality constraints, and instantly see the feasible set, constraint boundaries, and objective landscape rendered as synchronized 3D and 2D plots.

![Type](https://img.shields.io/badge/type-HTML%20%2F%20Vanilla%20JS-blue) ![Dependencies](https://img.shields.io/badge/dependencies-math.js%20%7C%20Plotly.js-informational) ![License](https://img.shields.io/badge/license-GNU-green)

---

## What it does

Given a function `f(x₁, x₂)` and a set of constraints, the tool:

- Evaluates `f` over a configurable grid
- Determines the **feasible set** — the region where all constraints are satisfied
- Renders a **3D surface** of `f` with the feasible region overlaid in a contrasting color
- Renders a **2D heatmap** with contour lines, the feasible region shaded, and each constraint boundary drawn explicitly

Both plots update together on every solve.

---

## Getting started

No installation or build step required. Open the file directly in any modern browser:

```bash
open constrained_optimization_viz.html       # macOS
start constrained_optimization_viz.html      # Windows
xdg-open constrained_optimization_viz.html  # Linux
```

Two CDN scripts are loaded automatically on first open (internet connection required):

| Library | Version | Purpose |
|---|---|---|
| [math.js](https://mathjs.org) | 11.11.0 | Parsing and evaluating user-entered expressions |
| [Plotly.js](https://plotly.com/javascript/) | 2.26.0 | Interactive 3D surface and 2D heatmap rendering |

---

## Interface overview

The sidebar on the left contains all inputs. The main area shows the two plots stacked vertically.

### Quick Presets

Six built-in examples load a complete problem with one click — useful for exploring the tool or as starting templates:

| Preset | Objective | Notes |
|---|---|---|
| **Paraboloid** | `x₁² + x₂²` | Disk region, first-quadrant constraint |
| **Rosenbrock** | `(1−x₁)² + 100(x₂−x₁²)²` | Classic banana function, disk constraint |
| **Saddle** | `x₁² − x₂²` | Linear equality constraint |
| **Himmelblau** | `(x₁²+x₂−11)² + (x₁+x₂²−7)²` | Box-constrained, four local minima |
| **Sine waves** | `sin(x₁)·cos(x₂)` | Strip domain |
| **Linear** | `2x₁ + 3x₂` | Polytope feasible region |

### Objective Function

Enter any expression in `x₁` and `x₂`. Standard math.js syntax applies:

```
x1^2 + x2^2
sin(x1) * cos(x2)
(1-x1)^2 + 100*(x2 - x1^2)^2
```

### Equality Constraints `g(x) = 0`

Each row defines one constraint of the form `g(x₁, x₂) = 0`. Enter the expression for `g` (the `= 0` part is implicit). Multiple constraints are ANDed together.

```
x1 + x2 - 1        →  x₁ + x₂ = 1
x1^2 + x2^2 - 1    →  unit circle
```

Click **+ add equality** to add rows, **✕** to remove them.

### Inequality Constraints `h(x) ≤ 0`

Each row defines one constraint of the form `h(x₁, x₂) ≤ 0`. Enter the expression for `h`.

```
x1^2 + x2^2 - 4    →  x₁² + x₂² ≤ 4  (inside disk of radius 2)
-x1                 →  x₁ ≥ 0
x1 + 2*x2 - 4       →  x₁ + 2x₂ ≤ 4
```

Multiple inequality constraints are ANDed together. To encode `x₁ ≥ 0`, use `-x1`; for `x₂ ≥ 0`, use `-x2`.

### Domain

Set the plot window for both axes independently. This controls what region is rendered — it does not affect the constraint logic.

```
x₁ ∈ [x₁ min, x₁ max]
x₂ ∈ [x₂ min, x₂ max]
```

**Grid resolution** sets how many sample points per axis (default 120). Higher values give smoother plots but take longer to compute. Values above ~200 may be slow on older hardware.

### Plot button

Click **Plot ↗** to recompute and redraw. Both plots refresh together.

---

## Reading the plots

### 3D Surface (top panel)

- The **Viridis-colored surface** shows `f(x₁, x₂)` across the full domain
- The **red-orange overlay** shows the feasible subset of the surface — points where all constraints are satisfied
- Contour lines are projected onto the surface for depth reference
- The plot is interactive: click and drag to rotate, scroll to zoom, hover for exact values

### 2D Feasible Set (bottom panel)

- The **heatmap background** encodes `f` values using the Viridis colorscale (same scale as 3D)
- Faint **white contour lines** show level sets of `f`
- The **red shaded region** is the feasible set
- **Teal/green solid lines** show equality constraint boundaries `g(x) = 0`
- **Dotted orange/red lines** show inequality constraint boundaries `h(x) = 0`

---

## Expression syntax

Expressions use [math.js syntax](https://mathjs.org/docs/expressions/syntax.html). Use `x1` and `x2` as the two variable names.

| Math notation | Expression to enter |
|---|---|
| x₁² + x₂² | `x1^2 + x2^2` |
| sin(x₁) · cos(x₂) | `sin(x1)*cos(x2)` |
| √(x₁² + x₂²) | `sqrt(x1^2 + x2^2)` |
| \|x₁\| | `abs(x1)` |
| e^(−x₁²) | `exp(-x1^2)` |
| ln(x₁) | `log(x1)` |

---

## Example problems to try

### Minimize a paraboloid on the unit circle
```
Objective:   x1^2 + x2^2
Equality:    x1^2 + x2^2 - 1
Inequality:  (none)
```

### Linear program on a polytope
```
Objective:   2*x1 + 3*x2
Equality:    (none)
Inequality:  -x1, -x2, x1+2*x2-4, 2*x1+x2-5
Domain:      x₁, x₂ ∈ [-0.5, 4]
```

### Non-convex function with ellipse constraint
```
Objective:   sin(x1)*cos(x2)
Equality:    (none)
Inequality:  x1^2/4 + x2^2 - 1
Domain:      x₁, x₂ ∈ [-3, 3]
```

---

## File structure

```
constrained_optimization_viz.html   — entire application (single self-contained file)
README.md                           — this file
```

All logic — expression parsing, grid evaluation, feasibility checking, and Plotly rendering — lives in a single HTML file. No bundler, no server, no build step.

---

## Browser compatibility

Requires a browser with ES6 and WebGL support (for the 3D surface):

- Chrome / Edge 80+
- Firefox 75+
- Safari 14+

---

## Limitations

- **2D input space only** — `f` must be a function of exactly two variables, `x1` and `x2`
- **No automatic optimization** — the tool visualizes the problem; it does not compute minima or maxima numerically. Use the plots to reason about the solution geometrically
- **Equality constraints are approximated** — a point is considered to satisfy `g(x) = 0` if `|g(x)| ≤ 0.08`. Narrow or highly curved equality constraints may render as thin or broken bands depending on grid resolution; try increasing the grid resolution if boundaries look coarse
- **No gradient or KKT display** — Lagrange multipliers and KKT conditions are not computed or shown
- **Performance** — grid resolution above ~200 may be slow on older hardware; very complex expressions with deep nesting or many function calls add to compute time

---

## License

MIT — free to use, modify, and share.
