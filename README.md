
# Seismic Refraction Acquisition, Forward Modelling, and Traveltime Tomography Inversion

This repository contains a synthetic seismic refraction workflow developed using Python and [pyGIMLi](https://www.pygimli.org/). The notebook covers the complete modelling sequence from the construction of a synthetic subsurface model and acquisition geometry to first-arrival traveltime simulation and seismic refraction tomography inversion.

The project was developed as part of a seismic exploration practicum and is intended to demonstrate the relationship between acquisition geometry, subsurface P-wave velocity distribution, forward modelling, and traveltime inversion.

---

## Project Workflow

The workflow implemented in the notebook is:


Surface Horizon
      ↓
Synthetic Three-Layer Model
      ↓
Mesh Generation
      ↓
P-Wave Velocity Assignment
      ↓
Source–Receiver Geometry
      ↓
Forward Modelling
      ↓
Synthetic First-Arrival Traveltimes
      ↓
Synthetic Noise Addition
      ↓
Traveltime Tomography Inversion
      ↓
Recovered Velocity Model
      ↓
Data Fit and Chi-Square Evaluation

---

## Synthetic Model

The subsurface is represented by a three-layer synthetic model with P-wave velocity increasing with depth.

| Layer | P-Wave Velocity |
|------:|----------------:|
| Layer 1 | 400 m/s |
| Layer 2 | 525 m/s |
| Layer 3 | 1300 m/s |

The model geometry follows a non-flat surface horizon and is discretized using a triangular mesh.

### Mesh Parameters

| Parameter | Value |
|---|---:|
| Mesh quality | 34.3 |
| Maximum mesh area | 3.0 |
| Mesh smoothing | `[1, 10]` |
| Mesh nodes | 11,674 |
| Mesh cells | 22,639 |

---

## Acquisition Geometry

A full-line synthetic acquisition geometry is used.

| Parameter | Value |
|---|---:|
| Receiver positions | 1.5–166.5 m |
| Receiver spacing | 3 m |
| Number of receivers | 56 |
| Shot positions | 0–168 m |
| Shot spacing | 3 m |
| Number of shots | 57 |
| Unique sensor positions | 113 |
| Total source–receiver traces | 3,192 |

The shot positions are also grouped into five categories for visualization:

- `P.Near`
- `Near`
- `Mid`
- `Far`
- `P.Far`

These groups are used only to visualize the spatial distribution of the shots and do not represent different physical modelling methods.

---

## Forward Modelling

Forward modelling is performed using the slowness model

\[
s = \frac{1}{V}
\]

where `V` is the P-wave velocity.

First-arrival traveltimes are calculated for all source–receiver combinations.

Synthetic noise is added to provide a more realistic inversion input:

| Parameter | Value |
|---|---:|
| Relative noise | 0.001 |
| Absolute noise | 0.001 s |
| Random seed | 1337 |

The resulting synthetic traveltime range is approximately:


0.003526 s – 0.182414 s


---

## Traveltime Tomography Inversion

The synthetic first-arrival traveltimes are inverted using `pygimli.physics.traveltime.TravelTimeManager`.

The final inversion parameters are:

| Parameter | Value |
|---|---:|
| `secNodes` | 2 |
| `paraMaxCellSize` | 15 |
| `maxIter` | 30 |

The inversion converges before the maximum number of iterations is reached.

```text
Actual iterations : 16 / 30
Final chi-square  : ≈ 0.97
```

A chi-square value close to 1 indicates that the model response explains the synthetic data within the assumed data uncertainty. The value should not be interpreted as a percentage accuracy.

---

## Main Outputs

The notebook produces the following main visualizations:

1. Synthetic three-layer geological model
2. Forward modelling mesh
3. Synthetic acquisition geometry
4. True P-wave velocity model
5. Simulated first-arrival traveltimes
6. Traveltime tomography inversion result
7. Observed vs. calculated first-arrival traveltime

The recovered velocity model reproduces the main velocity trend of the synthetic model, although layer boundaries appear smoother because of the regularized nature of seismic tomography inversion.

---

## Repository Structure

```text
.
├── README.md
└── Seismic_Acquisition_Forward_Modelling_Traveltime_Inversion_Final.ipynb
```

---

## Requirements

The notebook requires Python with the following main packages:

```text
numpy
matplotlib
pygimli
jupyter
```

The project was developed using a Python environment with pyGIMLi installed.

For pyGIMLi installation instructions, refer to:

https://www.pygimli.org/installation.html

---

## Running the Notebook

Clone the repository:

```bash
git clone https://github.com/LaksamanaPtra/Semester-3.git
```

Activate the Python environment containing pyGIMLi and start Jupyter:

```bash
jupyter notebook
```

Then open:

```text
Seismic_Acquisition_Forward_Modelling_Traveltime_Inversion_Final.ipynb
```

For reproducibility, run the notebook from the beginning using:

```text
Kernel → Restart & Run All
```

The random seed is fixed at `1337`, so the synthetic noise can be reproduced when the same software configuration is used.

---

## Interpretation Notes

This project uses a synthetic subsurface model. Therefore, the velocity values should not be directly interpreted as specific lithologies without additional geological or geophysical information.

The inversion result is also not expected to reproduce the true model exactly. Regularization, ray coverage, model parameterization, and data uncertainty influence the spatial resolution and smoothness of the recovered velocity distribution.

The acquisition geometry used in this project provides dense synthetic coverage but should not automatically be considered an optimal field acquisition design. Actual field design must account for target depth, topography, available equipment, environmental noise, source energy, receiver coupling, and operational constraints.

---

## References

- Everett, M. E. (2013). *Near-Surface Applied Geophysics*. Cambridge University Press.
- Rücker, C., Günther, T., & Wagner, F. M. (2017). pyGIMLi: An open-source library for modelling and inversion in geophysics. *Computers & Geosciences, 109*, 106–123. https://doi.org/10.1016/j.cageo.2017.07.011
- Sheehan, J. R., Doll, W. E., & Mandell, W. A. (2005). An evaluation of methods and available software for seismic refraction tomography analysis. *Journal of Environmental and Engineering Geophysics, 10*(1), 21–34. https://doi.org/10.2113/JEEG10.1.21
- Zhang, J., & Toksöz, M. N. (1998). Nonlinear refraction traveltime tomography. *Geophysics, 63*(5), 1726–1737. https://doi.org/10.1190/1.1444468

---

## Author

Developed for an academic seismic exploration practicum.

**Laksamana Putra Yulistiono**

---