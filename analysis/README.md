# Analysis notebooks

Code for the figures and analysis in *Beyond linear barotropic instability: a study on growth and
lifetime of submesoscale frontal eddies in slope currents* (Ellepola, Shakespeare & Yang).

The notebooks compare a linear stability theory for a barotropic current over a sloping shelf
(Ohshima 1987) with nonlinear single-layer MOM6 simulations, in terms of eddy growth rate, eddy
spacing (dominant wavelength) and eddy lifetime.

## Notebooks

| Notebook | What it does | Manuscript |
|---|---|---|
| `00_make_intake_datastore.ipynb` | Builds an intake-esm datastore (JSON catalog) for each MOM6 experiment, used to load `u` and `v` in notebooks 04 and 05. | – |
| `01_fig1_schematic_flow_and_topography.ipynb` | Schematic of the tanh velocity profile $U(y)$ and the tanh depth profile $h(y)$, with $L_{jet}$, $Y_j$, $Y_s$, $W_s$, $h_{shallow}$, $h_{deep}$ marked. | Fig. 1 |
| `02_ohshima_solver.ipynb` | Linear stability solver (strip method of Ohshima 1987) for the nondimensional stability equation with $U_0$ and $L_{jet}$ scaling. Validated against Ohshima's published eigenvalues, checked to be independent of $Ro$ for a flat bottom, and cross-checked against notebook 03. | Sec. 2.1, Eq. 3 |
| `02_flat_bottom_rayleigh_vs_ohshima.ipynb` | Flat-bottom case solved two ways (Rayleigh equation with the Dickinson & Clare method, and the Ohshima solver with $\hat h=1$), overlaid, then repeated for $U_0$ = 0.2, 0.3 and 0.4 m s⁻¹. | Flat-bottom reference |
| `03_dickinson_clare_solver.ipynb` | Rayleigh–Kuo eigenvalue solver for a hyperbolic-tangent shear layer, following Dickinson & Clare (1973): 4th-order Numerov discretisation, radiation boundary conditions, Newton iteration on the determinant, eigenfunction by inverse iteration, and growth rate against wavenumber. | Flat-bottom reference |
| `04_compute_growth_rate_wavelength_lifetime.ipynb` | Processes every MOM6 experiment: domain-integrated EKE $E(t)$ in a jet-following cross-shelf band, reference time $t_0$, growth rate, dominant wavelength from the along-shelf power spectrum of $u'$, and end of the eddy lifetime. Saves everything to one NetCDF file. | Sec. 2.3 |
| `05_fig2_flat_bottom_eke_growth_pv.ipynb` | Flat-bottom control (`test242`): $E(t)$, growth rate and dominant wavelength against time, and surface PV at $t_0$, at peak growth and at the end of the eddy lifetime. | Fig. 2 |
| `06_fig3_pv_multi_experiment_panel.ipynb` | Surface PV at four times for slope widths $\hat W$ = 1, 5, 9, 13, 17 and the flat-bottom control. | Fig. 3 |
| `07_figs4-6_theory_vs_simulations.ipynb` | Theory curves and simulated values of peak growth rate (Fig. 4) and dominant wavelength (Fig. 5) against $Ro$, $\hat h$, $\hat W$ and $\hat X_j$. Data-completeness filter, and the pooled comparison of growth rate, wavelength and lifetime with least-squares fits and cluster-bootstrap 95% confidence intervals (Fig. 6). | Figs. 4–6 |

## Workflow

```
MOM6 output ──> 00 (catalogs) ──> 04 (growth rate, wavelength, lifetime -> NetCDF) ──> 07 (Figs 4-6)
                     └──────────> 05 (Fig 2)
MOM6 output ──────────────────────> 06 (Fig 3)
01 (Fig 1), 02 and 03 (theory) run on their own.
```

1. Run `00` to build the datastores.
2. Run `04` to compute the per-experiment diagnostics. It processes experiments in concurrent
   batches on a Dask cluster and needs a large node (it was run on a 28-core, 256 GB node).
3. Run `05`, `06` and `07` to make the figures. `07` computes the theory curves itself (a few
   minutes; in parallel if `joblib` is installed) and caches them in `figures/`.

`01`, `02`, `02_flat_bottom_…` and `03` need no model output.

## Inputs and outputs

| Notebook | Reads | Writes |
|---|---|---|
| `00` | MOM6 experiment folders `test*-*` | `intake_style_datastore/test*.json` |
| `04` | intake catalogs, `ocean_static.nc`, `MOM_IC.nc` | `300_nondimplot_data_13_10.nc` (processed results) |
| `05` | intake catalog for `test242`, `ocean_static.nc`, `ocean_interior.nc` (PV) | `four_panel_plots/` |
| `06` | `ocean_static.nc`, `MOM_IC.nc`, `ocean_interior.nc` (PV) | `pv_plots/` |
| `07` | processed results NetCDF | `figures/` (PDF/PNG figures, theory caches `*.npz`, `pooled_comparison_*.csv`) |
| `01`, `03` | – | `idealized_IC_diagram.png/.pdf`, `pv_plots/theory.png` |

Data paths are set near the top of each notebook and point to the NCI Gadi locations used for
the paper (`/g/data/nm03/...`, `/scratch/nm03/...`). Change them to your own locations.

**Note:** `04` saves `300_nondimplot_data_13_10.nc`, while `07` reads
`300_nondimplot_data_19_10.nc` (the file used for the paper). Rename the file, or change
`DATA_FILES` in `07`, when rerunning.

## Environment

Run on NCI Gadi with the `conda/analysis3` environment. Main packages: `numpy`, `scipy`,
`matplotlib`, `pandas`, `xarray`, `dask`/`dask.distributed`, `xgcm`, `intake`/`intake-esm`,
`access_nri_intake` (notebook 00), and optionally `joblib`.

## References

- Ohshima, K. I. (1987). Stability of a barotropic jet on a sloping bottom. *Journal of the
  Oceanographical Society of Japan*, 43(1), 49–60.
- Dickinson, R. E., & Clare, F. J. (1973). Numerical study of the unstable modes of a
  hyperbolic-tangent barotropic shear flow. *Journal of the Atmospheric Sciences*, 30(6), 1035–1049.
