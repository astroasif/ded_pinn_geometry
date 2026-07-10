# Bead Geometry Prediction in Wire Arc Directed Energy Deposition Using Physics-Informed Machine Learning and Low-Fidelity Data

This repository accompanies the following publication:

> Rashid, A., Vatandoust, F., Kota, A., & Melkote, S. N. (2025). Bead geometry prediction in wire arc directed energy deposition using physics-informed machine learning and low-fidelity data. *Additive Manufacturing*, 109, 104881. https://doi.org/10.1016/j.addma.2025.104881

George W. Woodruff School of Mechanical Engineering, Georgia Institute of Technology, Atlanta, GA 30332-0405, USA

![Architecture diagram](figures/architecture.png)

## Overview

Wire Arc Directed Energy Deposition (Wire Arc DED), also known as Wire Arc Additive Manufacturing (WAAM), is a metal 3D-printing process, and accurately predicting the geometry of each deposited bead is difficult because of the tightly coupled thermal and geometric behavior involved. This work develops a coupled Physics-Informed Neural Network (PINN) framework that predicts bead geometry by combining the governing process physics with experimental data, rather than relying solely on expensive numerical simulations or purely data-driven models.

The framework uses a sequential two-step workflow:
1. A **thermal model** predicts the spatiotemporal temperature evolution during deposition (governed by the 3D transient heat conduction equation with a moving Gaussian heat source).
2. A **geometry model** uses that temperature field to predict the bead's cross-sectional profile, solving a surface-equilibrium (energy minimization) problem that accounts for arc pressure, surface tension, and gravity.

Both a **high-fidelity** model (finer temporal resolution) and a **low-fidelity** model (coarser temporal resolution) are developed and compared. Incorporating a small number of measured bead geometry data points into training substantially improves accuracy for both models, and the models generalize well to bead locations beyond where the data was collected.

### Key results
- The high-fidelity PINN model (0.2 s temporal step) reaches an average bead height prediction error of 8.38% and width error of 1.09%, after about 12.7 hours of training on four H100 GPUs.
- The low-fidelity PINN model (0.5 s temporal step) reaches comparable accuracy (8.33% height error, 1.56% width error) with just 2.7 hours of training on a single H100 GPU — a 79% reduction in training time with substantially lower hardware requirements.
- Incorporating as few as six experimental data points meaningfully improves prediction accuracy for both models, with the low-fidelity model benefiting the most.

Bead deposition experiments used to validate the models were performed on a six-axis KUKA KR 210 robotic Wire Arc DED testbed with a Lincoln Electric GMAW welding system, depositing mild steel wire (ER70S-6) beads that were characterized with a Keyence VR-6000 3D optical profilometer.

## Data and Code Location

The full dataset, trained model files, and source code are hosted on the Georgia Tech data repository (not in this GitHub repo) and can be accessed here:

**[Georgia Tech Repository — Companion Code and Dataset](https://repository.gatech.edu/entities/publication/9cccc5de-9096-4f8d-8cc4-ae228c164c8e)**

Please download and extract the full dataset from that link before running any of the code.

## Repository Structure

The dataset/code repository is organized into the following self-contained subfolders. Each includes the relevant code (model setup, architecture, training pipeline), corresponding datasets, trained model files, and the outputs/figures used in the paper.

| Folder | Description |
|---|---|
| `geometry_physics/` | Bead geometry model trained using physics constraints only (no experimental data) |
| `geometry_physics_and_data/` | Bead geometry model combining physics constraints with experimental data integration |
| `temperature_high_fidelity/` | High-fidelity thermal model: setup, training, and visualization (fine temporal resolution) |
| `temperature_low_fidelity/` | Low-fidelity thermal model: setup, training, and visualization (coarse temporal resolution) |

## Usage

- Each subfolder can be run independently, with no need to modify paths or directory structures.
- Ensure the full dataset has been downloaded and extracted from the [Georgia Tech repository](https://repository.gatech.edu/entities/publication/9cccc5de-9096-4f8d-8cc4-ae228c164c8e) link above.
- The provided Python scripts/notebooks are ready to run as-is.
- Required Python packages must be installed in your environment.

## Citation

If you use this dataset or code in your research, please cite the paper:

```bibtex
@article{rashid2025bead,
  title   = {Bead geometry prediction in wire arc directed energy deposition using physics-informed machine learning and low-fidelity data},
  author  = {Rashid, Asif and Vatandoust, Farzad and Kota, Akshar and Melkote, Shreyes N.},
  journal = {Additive Manufacturing},
  volume  = {109},
  pages   = {104881},
  year    = {2025},
  doi     = {10.1016/j.addma.2025.104881}
}
```

Please also cite the companion dataset hosted at the [Georgia Tech Repository](https://repository.gatech.edu/entities/publication/9cccc5de-9096-4f8d-8cc4-ae228c164c8e).

## License

The paper is published open access under a [CC BY 4.0 license](http://creativecommons.org/licenses/by/4.0/). Refer to the linked Georgia Tech repository page for the rights/terms governing the dataset and code themselves.
