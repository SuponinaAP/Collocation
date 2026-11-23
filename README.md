# Chebyshev Spectral Collocation: Numerical Solvers & Research

![Delphi](https://img.shields.io/badge/Delphi-VCL-orange)
![Numerical Methods](https://img.shields.io/badge/Numerical-Methods-blue)
![Spectral Methods](https://img.shields.io/badge/Spectral-green)
![Peer-Reviewed](https://img.shields.io/badge/Publication-MMG%202026-brightgreen)

A collection of Delphi applications implementing Chebyshev spectral collocation methods for solving boundary value problems and performing interpolation research. Developed at RUDN University (Dept. of Mathematical Modeling and AI) as part of the Master's thesis and a peer-reviewed publication.

## Repository Structure

This repository contains two independent Delphi projects sharing a common mathematical core:

### 1. `Diff_Int_Matrix/` — Cantilever Beam Modeling (Master's Thesis)

A desktop application for numerical simulation of elastic beam bending (cantilever) based on the spectral collocation method using Chebyshev polynomials of the first kind on a Gauss grid (1st kind).

**Source files:**
- `Project1.dpr` — Delphi project entry point
- `Unit1.pas` / `Unit1.dfm` — VCL GUI controller and form
- `uChebMath.pas` — Chebyshev polynomials, recurrence relations, Clenshaw algorithm
- `uChebInt.pas` — Lanczos spectral integration matrix construction
- `uChebDiff.pas` — Boundary value problem solver, cubic correction logic
- `uChartGridUtils.pas` — TeeChart helpers for visualization

**Key Features:**
- Solves the 4th-order Euler-Bernoulli beam equation: `EI·w''''(x) = q(x)`
- Uses sparse bidiagonal Lanczos matrix for O(N) spectral integration
- Implements cubic correction to handle boundary conditions on Gauss grids (which lack boundary nodes)
- Supports constant, sine, and cosine distributed loads
- Models contact interaction with non-zero tip force and bending moment
- Interactive GUI for real-time parameter tuning and residual error visualization

### 2. `Interpolation/` — Spectral Interpolation Research (Published Article)

A research application for comparing four variants of Chebyshev collocation methods:
- Chebyshev polynomials of the 1st kind (`T_n`) on Gauss grid (1st kind)
- Chebyshev polynomials of the 1st kind (`T_n`) on Lobatto grid (2nd kind)
- Chebyshev polynomials of the 2nd kind (`U_n`) on Gauss grid (1st kind)
- Chebyshev polynomials of the 2nd kind (`U_n`) on Lobatto grid (2nd kind)

**Source files:**
- `Project3.dpr`, `Unit1.pas`, `Unit1.dfm` — main application
- `compare*.csv` — raw experimental results (residual norms, coefficient values) for all tested functions and grid combinations

**Test functions:**
- `f1(x) = exp(α(x² − 1))` — smooth, rapidly changing near endpoints
- `f2(x) = sin(ωx)·e^(−x²)` — highly oscillatory (ω = 50)
- `f4(x) = |x − a|` — non-smooth (derivative discontinuity)

**CSV naming convention:**
- `_f` — function values
- `_rG` / `_rL` — residuals on Gauss / Lobatto grids
- `_uG` / `_uL` — expansion coefficients on Gauss / Lobatto grids
- `_nodesG` / `_nodesL` — grid node coordinates
- `first` / `second` — grid kind
- `both` — comparison across both polynomial kinds

## 🧮 Mathematical Core

Both projects share the same algorithmic foundation:

1. **Grid generation** — Chebyshev–Gauss nodes `x_k = cos((k+1/2)π/N)` or Chebyshev–Lobatto nodes `x_k = cos(kπ/N)`
2. **Discrete orthogonality** — Gram matrix `G = AᵀA` becomes diagonal (or nearly diagonal), reducing coefficient computation from O(N³) to O(N)
3. **Clenshaw algorithm** — numerically stable evaluation of Chebyshev series, avoiding Vandermonde ill-conditioning
4. **Lanczos integration matrix** — sparse bidiagonal structure for fast spectral integration

## Validation & Results

### Cantilever Beam Modeling (`Diff_Int_Matrix/`)

![Cantilever GUI](screenshots/cantilever_gui.png)
*Interactive VCL GUI: input parameters (E, I, L, q₀, N), load type selector, boundary conditions panel, and real-time deflection chart.*

![Cantilever deflection - cosine load](screenshots/cantilever_cosine_N18.png)
*Cosine distributed load at N=18: numerical solution (blue) coincides with analytical (red). Max residual ≈ 10⁻¹⁸.*

![Cantilever contact interaction](screenshots/cantilever_contact_force.png)
*Non-zero boundary conditions: tip force F = 10⁻⁹ N with bending moment — cubic correction successfully handles the BCs.*

### Spectral Interpolation Research (`Interpolation/`)

![Smooth function interpolation](screenshots/interpolation_smooth_n15.png)
*Smooth function f₁(x) = exp(α(x²−1)) interpolated on Chebyshev–Lobatto grid, n=15. Exponential convergence confirmed.*

![Non-smooth function interpolation](screenshots/interpolation_nonsmooth_n26.png)
*Non-smooth function f₄(x) = |x−a| on Chebyshev–Gauss grid, n=26. Algebraic convergence near the singularity point.*

## Publications

This repository accompanies the peer-reviewed article:

> **V. R. Nirdoshi, A. P. Suponina, K. P. Lovetskiy.** "Fast and correct computation of spectral representations of functions on Chebyshev grids" // *Mathematical Modelling and Geometry*, Vol. 14, No 2, pp. 21–44, 2026. DOI: [10.26456/mmg/2026-1423](http://mmg.tversu.ru)

## How to Run

Both projects are built using **Embarcadero Delphi** with the VCL framework.

1. Clone the repository: `git clone https://github.com/SuponinaAP/Collocation.git`
2. Open the corresponding `.dpr` file in Delphi (or Lazarus / Free Pascal):
   - For cantilever modeling: `Diff_Int_Matrix/Project1.dpr`
   - For interpolation research: `Interpolation/Project3.dpr`
3. Compile and run.
4. *Note: TeeChart components are required for visualization.*

## Context

- **Master's Thesis:** "Моделирование упругих свойств кантилевера методом Чебышёвских коллокаций на сетке 1-го рода", RUDN University, 2026
- **Research Project:** Comparative study of Chebyshev interpolation grids, published 2026

## License

MIT License. The mathematical core may be freely reused in scientific or engineering projects.
