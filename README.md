# PChem Function Plotter

A single-file, dependency-free function plotter for physical chemistry teaching and
quick exploration. Everything — expression parser, renderer, presets — lives in one
HTML file that runs offline in any modern browser.

**Live version: https://ajerschow.github.io/function-plotter/**

## Features

- **Expression parser** with implicit multiplication (`2x`, `3sin(x)`), `^` or `**`
  for powers, and clear error messages pointing at the offending token.
- **Named parameters.** Any identifier that isn't the plot variable, a constant, or a
  function becomes a slider, so curves can be varied interactively.
- **Multiple curves** on shared axes, each with its own colour.
- **Log x / log y axes**, optional grid, adjustable sample count (50–5000 points).
- **Custom axis labels** and manual or automatic axis ranges.
- **Copy data as CSV** for the sampled points.
- **Light/dark theme**, following the system setting unless overridden.
- **State is remembered** in `localStorage` between sessions.
- **Named definitions** (opt-in, see below), usable in any curve and selectable as the x-axis.

## Built-in presets

Maxwell–Boltzmann speed distribution · particle in a box (ψₙ and |ψₙ|²) · Morse vs
harmonic potential · Lennard-Jones potential · van der Waals isotherms (CO₂) ·
Arrhenius rate constant (log y) · Planck black-body spectrum · hydrogen radial
distribution functions · Boltzmann populations of a two-level system · reduced van der
Waals isotherms Pᵣ(Vᵣ) · reduced van der Waals compressibility factor Z(Pᵣ) · molar
enthalpy of ethane integrated from its heat capacity · isothermal and reversible
adiabatic expansion of a monatomic and a diatomic ideal gas.

## Expression syntax

| | |
|---|---|
| Operators | `+` `-` `*` `/` `^` (or `**`), parentheses, implicit multiplication |
| Functions | `sin` `cos` `tan` `asin` `acos` `atan` `sinh` `cosh` `tanh` `exp` `ln` `log` `log10` `log2` `sqrt` `abs` `erf` `floor` `ceil` `min` `max` `pow` |
| Constants | `pi` `e` `R` `kB` `h` `hbar` `c` `NA` `F` `qe` `me` `amu` `eps0` `a0` |

Constants are SI values from CODATA. `erf` uses the Abramowitz & Stegun 7.1.26
approximation (absolute error < 1.5×10⁻⁷).


Example — a Gaussian with adjustable width, where `sigma` becomes a slider:

```
exp(-x^2/(2sigma^2))/(sigma sqrt(2pi))
```

## Defining quantities and choosing the x-axis

Sometimes the quantity you want on the x-axis isn't the natural variable. The
compressibility factor is the standard example: the reduced van der Waals equation gives
Pᵣ as a function of Vᵣ, so Z(Pᵣ) has no closed form — getting it directly would mean
solving a cubic at every point.

Open **Advanced: define quantities and choose the x-axis** under Axes and name an
expression in the variable. The name can then be used in any curve, and picked from that
curve's *plot against* menu as its x-axis. The variable is swept behind the scenes and
need not appear on either axis. For the compressibility chart, sweeping Vᵣ:

```
definition   PR_10 = 8*1.0/(3*VR - 1) - 3/VR^2        (P_R for this isotherm)
curve        y: 3*PR_10*VR/(8*1.0)   plot against: PR_10
```

The y expression is just Z = 3PᵣVᵣ/(8Tᵣ) with the definition substituted in, so it reads
the way the physics does. This is exact at every point, including the critical point
where a root-finder is worst conditioned.

Definitions resolve in the order listed, so one may build on those above it; referring to
a later one is reported rather than silently treated as a free parameter. A defined name
never becomes a slider. Each curve chooses its own x, so a family of isotherms can share
one plot even though each has a different Tᵣ.

Two companion settings appear alongside: an explicit **x-axis range** (the *Variable
from/to* boxes then bound the sweep, not the axis), and **sample the variable
logarithmically**, which matters when a linear sweep would crowd its points into one
corner of the plot.

With no definitions the panel stays collapsed, the *plot against* menus don't appear, and
ordinary y-against-x plotting is untouched.

## Version history

Tags mark states worth returning to:

| Tag | What it holds |
|---|---|
| `v1-parametric-x` | Per-curve parametric x expressions, before named definitions. `git show v1-parametric-x` for the full note. |

To restore just the app from a tag: `git checkout <tag> -- index.html`.

## Running locally

Open `index.html` in a browser. No build step, no server, no installation. The only
network request is for the IBM Plex webfont; without it the page falls back to system
fonts and works unchanged.
