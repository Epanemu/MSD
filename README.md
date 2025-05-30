# Bias Detection via Maximum Subgroup Discrepancy

This repository contains code for detecting intersectional bias using the **Maximum Subgroup Discrepancy (MSD)** approach, comparing distributions (e.g., two subsets of US Census data) via protected subgroups.

It also includes a modified version of hte [AIX 360 library](https://github.com/Trusted-AI/AIX360/).

---

## Installation

**Install Dependencies**:
    ```
    pip install -r requirements.txt
    ```

   - **Python 3.9+** recommended.
   - **Gurobi**: If using the MIO-based OneRule approach, ensure Gurobi is installed & licensed or switch to a different solver in the code.

---

## Repository Structure

- **experiment_enumerative.py**
  - Runs the enumerative or direct approach comparing MSD against other distances.

- **experiment_sample_complexity.py**
  - Varies sample sizes to show how quickly each distance converges.

- **scenarios/folktables_scenarios.py**
  - Loads and preprocesses Folktables-based datasets (US Census ACS).

- **plot-maker/**
  - Contains scripts (plot_exploration.py, plots_for_paper.py) for generating figures.

- **methods.py, one_rule.py, utils.py**
  - Core logic for the MIO-based OneRule (single-term) solver, plus helper functions for distances, evaluation, etc.

- **conf/**
  - Hydra configuration files, specifying scenarios, seeds, model parameters.

---

## How to Run Experiments

Below are common examples using Hydra-style overrides:

1. **OneRule MSD** on ACSIncome (California):
   ```
   python experiment_enumerative.py -m
   ```
   Use config *conf/enumerative.yaml*.

2. **Sample Complexity** experiments:
   ```
   python experiment_sample_complexity.py -m
   ```
   Use config *conf/distances.yaml*.


Results are typically saved to `./multirun/`. To utilize the structure as the precomputed results, add `+dir_structure=cluster_batch` as a command line argument, and the results will be saved in `./batch_out/` folder.

---

## Plotting Results

After running experiments, you can produce plots:

1. **Enumeration Comparison**
   ```
    python ./plot-maker/plot_exploration.py
    ```

2. **Paper Figures**
   ```
    python ./plot-maker/plots_for_paper.py base
    ```
   And instead of 'base' you can also choose 'relative' or 'RSE' for different y-axis interpretations

Results are will be saved to the working folder (`./`).

---

## Notes

- Plots are being generated from data in folder `batch_precomputed`. If you would like to plot your own experiments, move it to the folder, and rename them, or change the plotting scripts.
- The code expects certain columns in Folktables data; see `PROTECTED_ATTRS` in `scenarios/folktables_scenarios.py`.
- BRCG and Ripper require AIX360. There is manually installed AIX360 with modified versions of the algorithms. MDSS or other advanced fairness methods can require AIF360.
- Hydra automatically creates separate output folders for each run, storing logs and an `output.txt` with the distance results.

