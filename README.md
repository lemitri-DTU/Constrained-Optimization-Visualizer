# Constrained-Optimization-Visualizer

An interactive tool for visualizing **constrained optimization problems** with two variables, implemented in two languages. Define an objective function, add equality and inequality constraints, and instantly see the feasible set, constraint boundaries, and objective landscape rendered as synchronized 3D and 2D plots.

---

![Type](https://img.shields.io/badge/type-HTML%20%2F%20Vanilla%20JS-blue) ![Dependencies](https://img.shields.io/badge/dependencies-math.js%20%7C%20Plotly.js-informational) ![License](https://img.shields.io/badge/license-GNU-green)

---

## 🗂️ Repository Structure

```
constrained-optimization-visualizer/
│
├── constrained_optimizer_viz.ipynb     # Main Jupyter notebook
├── constrained_viz.png                 # Auto-generated output plot
├── constrained_optimizer_viz.html      # Standalone browser-based visualizer
└── README.md
```

---

## 🔀 Implementations

The Python notebook is ideal for academic and research workflows — it integrates with data pipelines, supports symbolic math via SymPy, and produces publication-quality figures. The HTML version prioritizes accessibility: anyone can open a link and explore the visualizer without installing anything. Both target the same use case but serve different audiences.

| Feature | Python (Jupyter) | HTML (Browser) |
|---|---|---|
| **Language** | Python 3 | HTML + CSS + JavaScript |
| **Libraries** | NumPy, Matplotlib, SymPy | Plotly.js / Math.js |
| **Environment** | Jupyter / Colab / VS Code | Any modern browser |
| **Input method** | Edit variables in the notebook | Input fields in the UI |
| **Output** | Inline plot + saved `.png` | Interactive chart in-browser |
| **Best for** | Data science workflows | Sharing without setup |

---

## 🧠 What it does

Both implementations solve the same problem: given a user-defined objective function $f(x_1, x_2)$ and a set of equality/inequality constraints, they:

- Evaluate `f` over a configurable grid
- Determine the **feasible set** — the region where all constraints are satisfied
- Render a **3D surface** of `f` with the feasible region overlaid in a contrasting color
- Render a **2D heatmap** with contour lines, the feasible region shaded, and each constraint boundary drawn explicitly

Both plots update together on every solve.

---

## 🐍 Python implementation

Built as a Jupyter notebook using NumPy, Matplotlib, and SymPy. Designed for use in data science environments where you can edit the source directly.

### ⚙️ Dependencies

Make sure the following Python libraries are installed:

```bash
pip install numpy matplotlib sympy ipywidgets
```

Or run the commented-out cell at the top of the notebook:

```python
# %pip install numpy matplotlib sympy ipywidgets --quiet
```

---

### 🚀 How to Use

**1. Open the notebook** in Jupyter or any compatible environment (JupyterLab, VS Code, Google Colab).

**2. Edit Section ①** — Define your problem by modifying these variables:

```python
# Objective function
f_expr_str = "x1**2 + x2**2"

# Equality constraints g(x) = 0  →  list of strings, or [] for none
equality_constraints = [
    "x1 + x2 - 1"    # means x1 + x2 = 1
]

# Inequality constraints h(x) ≤ 0  →  list of strings, or [] for none
inequality_constraints = [
    "x1**2 + x2**2 - 4",   # x1² + x2² ≤ 4
    "-x1",                  # x1 ≥ 0
    "-x2",                  # x2 ≥ 0
]

# Plot domain
x1_range = (-3, 3)
x2_range = (-3, 3)
n_grid   = 300    # increase for smoother output
```

**3. Run all cells** — The plot will render inline and be saved as `constrained_viz.png` in the same directory.

---

### 📐 Constraint Syntax

Expressions are parsed using [SymPy](https://www.sympy.org/), so standard mathematical notation applies:

| Symbol | Usage |
|--------|-------|
| `**` | Exponentiation (`x1**2` = $x_1^2$) |
| `*` | Multiplication |
| `sqrt(x1)` | Square root |
| `exp(x1)` | Exponential |
| `log(x1)` | Natural log |

> **Tip:** Equality constraints are evaluated with a small tolerance band (`eps_eq = 0.08`) to handle discrete grid approximation. Tighten or widen this in Section ② if needed.

---

### 🖼️ Output

The figure is saved automatically as **`constrained_viz.png`** (150 dpi) in the working directory.

---

## 🌐 HTML implementation 

A fully self-contained interactive browser app, in which the user can interact directly with the interface.

### How to run

No installation or build step required. 

**1. Open the file directly in any modern browser (Chrome, Firefox, Edge, Safari):**

```bash
open constrained_optimization_viz.html       # macOS
start constrained_optimization_viz.html      # Windows
xdg-open constrained_optimization_viz.html  # Linux
```

> No internet connection required after the first load (if JS libraries are bundled). If CDN-linked, a connection is needed to load Plotly.js and Math.js.

Two CDN scripts are loaded automatically on first open (internet connection required):

| Library | Version | Purpose |
|---|---|---|
| [math.js](https://mathjs.org) | 11.11.0 | Parsing and evaluating user-entered expressions |
| [Plotly.js](https://plotly.com/javascript/) | 2.26.0 | Interactive 3D surface and 2D heatmap rendering |


**2. Enter your objective function and constraints in the input fields.**
**3. Click "Visualize" to render the plot instantly.**
   
---

### Interface overview

The sidebar on the left contains all inputs. The main area shows the two plots stacked vertically.

#### Quick Presets

Six built-in examples load a complete problem with one click — useful for exploring the tool or as starting templates:

| Preset | Objective | Notes |
|---|---|---|
| **Paraboloid** | `x₁² + x₂²` | Disk region, first-quadrant constraint |
| **Rosenbrock** | `(1−x₁)² + 100(x₂−x₁²)²` | Classic banana function, disk constraint |
| **Saddle** | `x₁² − x₂²` | Linear equality constraint |
| **Himmelblau** | `(x₁²+x₂−11)² + (x₁+x₂²−7)²` | Box-constrained, four local minima |
| **Sine waves** | `sin(x₁)·cos(x₂)` | Strip domain |
| **Linear** | `2x₁ + 3x₂` | Polytope feasible region |

#### Objective Function

Enter any expression in `x₁` and `x₂`. Standard math.js syntax applies:

```
x1^2 + x2^2
sin(x1) * cos(x2)
(1-x1)^2 + 100*(x2 - x1^2)^2
```

#### Equality Constraints `g(x) = 0`

Each row defines one constraint of the form `g(x₁, x₂) = 0`. Enter the expression for `g` (the `= 0` part is implicit). Multiple constraints are ANDed together.

```
x1 + x2 - 1        →  x₁ + x₂ = 1
x1^2 + x2^2 - 1    →  unit circle
```

Click **+ add equality** to add rows, **✕** to remove them.

#### Inequality Constraints `h(x) ≤ 0`

Each row defines one constraint of the form `h(x₁, x₂) ≤ 0`. Enter the expression for `h`.

```
x1^2 + x2^2 - 4    →  x₁² + x₂² ≤ 4  (inside disk of radius 2)
-x1                 →  x₁ ≥ 0
x1 + 2*x2 - 4       →  x₁ + 2x₂ ≤ 4
```

Multiple inequality constraints are ANDed together. To encode `x₁ ≥ 0`, use `-x1`; for `x₂ ≥ 0`, use `-x2`.

#### Domain

Set the plot window for both axes independently. This controls what region is rendered — it does not affect the constraint logic.

```
x₁ ∈ [x₁ min, x₁ max]
x₂ ∈ [x₂ min, x₂ max]
```

**Grid resolution** sets how many sample points per axis (default 120). Higher values give smoother plots but take longer to compute. Values above ~200 may be slow on older hardware.

#### Plot button

Click **Plot ↗** to recompute and redraw. Both plots refresh together.

---

### Reading the plots

#### 3D Surface (top panel)

- The **Viridis-colored surface** shows `f(x₁, x₂)` across the full domain
- The **red-orange overlay** shows the feasible subset of the surface — points where all constraints are satisfied
- Contour lines are projected onto the surface for depth reference
- The plot is interactive: click and drag to rotate, scroll to zoom, hover for exact values

#### 2D Feasible Set (bottom panel)

- The **heatmap background** encodes `f` values using the Viridis colorscale (same scale as 3D)
- Faint **white contour lines** show level sets of `f`
- The **red shaded region** is the feasible set
- **Teal/green solid lines** show equality constraint boundaries `g(x) = 0`
- **Dotted orange/red lines** show inequality constraint boundaries `h(x) = 0`

---

### Expression syntax

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

### Example problems to try

#### Minimize a paraboloid on the unit circle
```
Objective:   x1^2 + x2^2
Equality:    x1^2 + x2^2 - 1
Inequality:  (none)
```

#### Linear program on a polytope
```
Objective:   2*x1 + 3*x2
Equality:    (none)
Inequality:  -x1, -x2, x1+2*x2-4, 2*x1+x2-5
Domain:      x₁, x₂ ∈ [-0.5, 4]
```

#### Non-convex function with ellipse constraint
```
Objective:   sin(x1)*cos(x2)
Equality:    (none)
Inequality:  x1^2/4 + x2^2 - 1
Domain:      x₁, x₂ ∈ [-3, 3]
```

---

### File structure

```
constrained_optimization_viz.html   — entire application (single self-contained file)
README.md                           — this file
```

All logic — expression parsing, grid evaluation, feasibility checking, and Plotly rendering — lives in a single HTML file. No bundler, no server, no build step.

---

### Browser compatibility

Requires a browser with ES6 and WebGL support (for the 3D surface):

- Chrome / Edge 80+
- Firefox 75+
- Safari 14+

---

### Limitations

- **2D input space only** — `f` must be a function of exactly two variables, `x1` and `x2`
- **No automatic optimization** — the tool visualizes the problem; it does not compute minima or maxima numerically. Use the plots to reason about the solution geometrically
- **Equality constraints are approximated** — a point is considered to satisfy `g(x) = 0` if `|g(x)| ≤ 0.08`. Narrow or highly curved equality constraints may render as thin or broken bands depending on grid resolution; try increasing the grid resolution if boundaries look coarse
- **No gradient or KKT display** — Lagrange multipliers and KKT conditions are not computed or shown
- **Performance** — grid resolution above ~200 may be slow on older hardware; very complex expressions with deep nesting or many function calls add to compute time

---

## License

This project is open source and available under the [GNU General Public License v3.0](LICENSE).
