# Reaction-Diffusion SIR Project (TMA4212 Project 1)

This repository accompanies the first project in *TMA4212 Numerical Solution of Differential Equations by Difference Methods*. It contains the LaTeX source for the report and Python implementations of the reaction–diffusion SIR model and supporting heat-equation solvers used in the analysis.

## Repository Layout
- `main.tex` and `sections/`: report source organised by section.
- `refs/`: shared LaTeX macros and bibliography (`biblatex`/`biber`).
- `simulation/`: Python code for the numerical experiments (SIR model and diffusion solvers).
- `figures/`, `extra/`: assets referenced by the report.
- `tma4212_project.pdf`: compiled report for quick reference.

## Python Environment
Python 3.10+ is recommended. Install dependencies into a virtual environment:

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install --upgrade pip
pip install numpy scipy matplotlib scienceplots
```

`scienceplots` is required only for the heat-equation visualisations in `simulation/theory.py`; the SIR scripts run without it.

## Running the SIR Simulations
The main demo animates infection dynamics on a square domain:

```bash
python simulation/SIRSimulation.py
```

Key parameters are defined in the `SIRSimulation` constructor:
- `M`: spatial resolution (number of grid points per axis).
- `dt`, `T`: time step and final time.
- `beta`, `gamma`: infection and recovery rates.
- `mu_s`, `mu_i`: diffusion coefficients for susceptible and infected populations.
- `n`: selects the initial infection pattern (`0` corners, `1` front, else random clusters).
- `dynamic_beta`: toggle for a spatially and temporally varying infection rate.

The script shows the initial infection map and a 3D animation of the infected fraction. Modify the parameters at the bottom of `simulation/SIRSimulation.py` to explore alternative scenarios. Animations open in an interactive window; close the window to end the run.

### Alternative SIR Experiment
`simulation/application.py` provides a more configurable SIR class with optional event-driven outbreaks. Run it the same way after adjusting the parameters and event dictionary near the end of the file. This script also generates 3D animations of the infected population over time.

## Heat-Equation / Theory Experiments
`simulation/theory.py` implements the Crank–Nicolson-based solver used for the theoretical analysis of the heat equation. The script demonstrates:
- assembling the 2D Laplacian with Neumann-like adjustments,
- applying boundary/initial conditions,
- time-stepping with predictor–corrector coupling,
- plotting or animating snapshots via Matplotlib (with `scienceplots`).

Edit the `Heat` object configuration near the bottom of the file to reproduce the experiments discussed in the report and call `python simulation/theory.py` to launch the plots/animations.

## Rebuilding the Report
The report relies on `latexmk` and `biber`. Rebuild it with:

```bash
latexmk -pdf main.tex
biber main
latexmk -pdf main.tex
```

Adjust the bibliography run (`biber main`) to `bibtex` if you swap backends. Outputs are written to the project root; clean auxiliary files with `latexmk -c` when needed.

## Tips
- Animations can be saved by replacing `plt.show()` with `FuncAnimation.save` calls (Matplotlib provides `ffmpeg`/`imagemagick` writers).
- Use consistent random seeds (`np.random.seed(...)`) before initialising simulations if you need reproducible infection patterns.
- Large grids or very small `dt` markedly increase runtime—start with moderate sizes (`M ≈ 50`) before scaling up.