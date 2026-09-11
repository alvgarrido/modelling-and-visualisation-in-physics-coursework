# Modelling and Visualisation in Physics

Coursework for the University of Edinburgh's [Modelling and Visualisation in Physics](https://www.drps.ed.ac.uk/18-19/dpt/cxphys10035.htm) course. The repository contains 20 Python Jupyter notebooks covering statistical physics simulations, cellular automata, and numerical solutions of partial differential equations.

## Contents

| Folder | Topics and notebooks |
| --- | --- |
| `CHECKPOINT_1_Ising_Model/` | Two-dimensional Ising model: Glauber and Kawasaki dynamics, animation, data generation, and plots of thermodynamic observables. |
| `CHECKPOINT_2_Cellular_Automata/` | Conway's Game of Life, equilibration-time histograms, and the susceptible–infected–recovered–susceptible (SIRS) model, including parameter scans, infection variance, and immunity. |
| `CHECKPOINT_3_Partial_Differential_Equation/` | Cahn–Hilliard simulations, electric and magnetic fields, and Jacobi, Gauss–Seidel, and successive over-relaxation (SOR) solvers. |

All coursework source files are notebooks (`.ipynb`), including `SOR.py.ipynb`.

## Setup

Install Python 3 and create a virtual environment from the repository root.

### Windows (PowerShell)

```powershell
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe -m jupyterlab
```

### macOS / Linux

```bash
python3 -m venv .venv
.venv/bin/python -m pip install -r requirements.txt
.venv/bin/python -m jupyterlab
```

These commands use the virtual environment directly, so activation is unnecessary. In JupyterLab, open a checkpoint folder and choose a notebook with the Python 3 kernel.

`requirements.txt` includes NumPy and SciPy for numerical calculations, pandas for CSV data, Matplotlib for plotting, and JupyterLab with ipykernel for running notebooks. Standard-library modules such as `math`, `random`, `sys`, and `time` require no separate installation.

The saved notebook metadata records Python 3.7.6 or 3.9.2. The original package versions are not recorded, so dependencies are unpinned; this is not a reconstruction of the original environment, and compatibility with newly resolved versions has not been verified.

## Running the coursework

Run cells from top to bottom and respond to any input prompts. Simulation parameters are set either through prompts or directly in cells. Review lattice sizes, sweep counts, and other parameters before running long simulations. Interrupt the kernel to stop a running simulation.

The notebooks read and write CSV files using relative paths. Keep the kernel's working directory in the notebook's checkpoint folder so generators and plotting notebooks use the same files. You can inspect it with `%pwd` in a notebook cell. Generated CSV files are not included in this repository.

### Checkpoint 1: Ising model

- Open `ANIMATION.ipynb` to visualise spin dynamics; it prompts for the lattice size, temperature, and algorithm (`g` for Glauber or `k` for Kawasaki).
- Run `GLAUBERdatagenerator.ipynb` or `KAWASAKIdatagenerator.ipynb` to produce CSV data.
- Then use `PLOTTING_ISING_MODEL(GLAUBER&KAWASAKI).ipynb` to analyse the generated data.

The plotting notebook reads hard-coded `N50` filenames even though it prompts for `N`. Generate data with `N = 50`, or update its input filenames to match the lattice size you used.

### Checkpoint 2: Cellular automata

- `GameOfLife.ipynb` supports random, glider, and oscillator initial states. `GameOfLife_HISTOGRAM.ipynb` generates and plots histogram data.
- `SIRS_VISUALIZATION.ipynb` runs a SIRS visualisation with prompted transition probabilities.
- Run `SIRSdatacollection.ipynb` before `SIRS_PLOTTING.ipynb`.
- Run `SIRS_varianceCut.ipynb` before `SIRS_varianceCut_PLOTTING.ipynb`.
- Run `SIRS_inmune.ipynb` before `SIRS_inmune_PLOTTING.ipynb` (the filenames use the spelling `inmune`).

Two filename mismatches need to be reconciled before using the separate plotting notebooks:

| Generator output | Plotting notebook expects |
| --- | --- |
| `InfectedData100.csv`, `VarianceData100.csv` | `InfectedData.csv`, `VarianceData.csv` |
| `InfectedCutData.csv` | `InfectCutData.csv` |

Update the relevant `pd.read_csv(...)` calls to use the generated filenames, or copy the generated files under the expected names.

### Checkpoint 3: Partial differential equations

- `cahn-hilliard_phi0.ipynb` and `cahn-hilliard_phi0.5.ipynb` simulate Cahn–Hilliard evolution for two mean order parameters.
- `JacobiEfield.ipynb` and `Gauss-Seidel.ipynb` calculate electric-field results using different iterative solvers.
- `Bfield.ipynb` calculates magnetic-field results and prompts for a Jacobi or Gauss–Seidel solver.
- `SOR.py.ipynb` and `SOR2D.ipynb` explore successive over-relaxation.

These notebooks contain their own simulation and output code; choose one and execute its cells in order.

## Plotting and reproducibility

Some visualisations update figures using `plt.pause(...)`. Their display behaviour depends on the Matplotlib backend. If live updates do not appear in JupyterLab, a local desktop session can use `%matplotlib tk` before importing `matplotlib.pyplot`, provided Python's Tk support is installed. Restart the kernel before changing backends if needed.

The simulations use random initial states or stochastic updates, so results can vary between runs. For repeatable experiments, seed both Python's `random` module and NumPy's random generator before initialisation, and record simulation parameters and installed package versions. Re-running data-generation cells can overwrite their CSV outputs.
