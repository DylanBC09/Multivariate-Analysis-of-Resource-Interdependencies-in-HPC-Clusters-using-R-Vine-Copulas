# Multivariate Analysis of Resource Interdependencies in HPC Clusters using R-Vine Copulas

Code and data accompanying the paper *"Multivariate Analysis of Resource Interdependencies in HPC Clusters: A Validated R-Vine Copula Approach"* (Dylan Benavides, National High Technology Center, Costa Rica).

## Overview

This repository reproduces every table and figure in the paper: descriptive statistics, bivariate and R-Vine copula fitting, an exhaustive structure-selection robustness check (Section IV-C), and a six-part statistical validation battery (Section IV-E): parametric model comparison, out-of-sample predictive validation, tie-breaking sensitivity, bootstrap confidence intervals, formal conditional-independence testing, and temporal-stability assessment.

## Repository structure

```
.
├── analysis/
│   └── copula_analysis.Rmd      # Full analysis pipeline (knit to reproduce all tables/figures)
├── data/
│   └── sacct.csv                # NOT tracked in git -- download from Zenodo, see Data below
├── checkpoints/                 # Optional: intermediate .rds results from a full run (see below)
├── README.md
├── LICENSE
└── .gitignore
```

## Requirements

- R >= 4.5.0
- Packages:
  ```r
  install.packages(c("VineCopula", "copula", "dplyr", "ggplot2", "scales",
                      "patchwork", "knitr", "RANN", "sessioninfo",
                      "fitdistrplus", "GGally"))
  ```
- Tested with VineCopula 2.6.1 and copula 1.1-7 on R 4.5.2 (see full session info printed at the end of the knitted output).

## Data

The dataset (`sacct.csv`, 33.7 MB) is archived on Zenodo rather than stored in this repository:

> Benavides Castillo, D. S. (2026). SLURM Accounting Dataset for "Multivariate Analysis of Resource Interdependencies in HPC Clusters: A Validated R-Vine Copula Approach" [Dataset]. Zenodo. https://doi.org/10.5281/zenodo.21463675

Download it from Zenodo and place it at `data/sacct.csv` before running the analysis. It contains job-level resource-consumption records extracted via SLURM's `sacct` accounting utility from the Kabré supercomputer, covering June 18, 2024 -- July 14, 2026, across three partitions with distinct hardware profiles: Dribe (memory-optimized), Kurá (general-purpose CPU), and Nukwa (GPU-accelerated). Only `ConsumedEnergyRaw`, `CPUTimeRAW`, `ReqMem`, `ReqCPUS` (Table II of the paper), plus `Partition`, `State`, and `Submit` (used only for partition/mode stratification and the temporal-stability check, Section IV-E6) are included; no user-identifying or job-command fields.

## Reproducing the results

1. Download `sacct.csv` from the Zenodo record above into `data/sacct.csv` (or adjust the path in the first data-loading chunk).
2. Knit `analysis/copula_analysis.Rmd` in RStudio, or from the command line:
   ```bash
   Rscript -e 'rmarkdown::render("analysis/copula_analysis.Rmd")'
   ```
3. The script defaults to `MODO_RAPIDO <- FALSE` (full publication-quality run: exhaustive structure comparison, 300-resample bootstrap, 300-point predictive-validation grid; ~1-3 hours depending on hardware). Set it to `TRUE` for a fast (~15-20 min) verification run with reduced bootstrap/grid resolution before committing to the full run.
4. Progress and per-stage timing are logged to the console via timestamped checkpoints; each expensive stage also saves its result to `checkpoints/*.rds`, so an interrupted run can be resumed by loading the relevant checkpoint instead of restarting from scratch (see comments at the top of the `.Rmd`).
5. All tables appear in the knitted output in the order they are reported in Results and Discussion (Section IV). Full session and package version information is printed at the end via `sessioninfo::session_info()`.

## Citation

If you use this code, please cite the paper; if you use the data, please cite the dataset:

> Benavides, D. (2026). Multivariate Analysis of Resource Interdependencies in HPC Clusters: A Validated R-Vine Copula Approach.

> Benavides Castillo, D. S. (2026). SLURM Accounting Dataset for "Multivariate Analysis of Resource Interdependencies in HPC Clusters: A Validated R-Vine Copula Approach" [Dataset]. Zenodo. https://doi.org/10.5281/zenodo.21463675

## Acknowledgments

This research was supported by the Kabré Supercomputer at the National High Technology Center of Costa Rica.
