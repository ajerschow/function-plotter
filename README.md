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

## Built-in presets

Maxwell–Boltzmann speed distribution · particle in a box (ψₙ and |ψₙ|²) · Morse vs
harmonic potential · Lennard-Jones potential · van der Waals isotherms (CO₂) ·
Arrhenius rate constant (log y) · Planck black-body spectrum · hydrogen radial
distribution functions · Boltzmann populations of a two-level system.

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

## Running locally

Open `index.html` in a browser. No build step, no server, no installation. The only
network request is for the IBM Plex webfont; without it the page falls back to system
fonts and works unchanged.
