# Growth and lifetime of submesoscale eddies in slope currents

Code for *Beyond linear barotropic instability: a study on growth and lifetime of submesoscale
frontal eddies in slope currents* (Ellepola, Shakespeare & Yang).

The study compares a linear stability theory for a barotropic current over a sloping shelf
(Ohshima 1987) with nonlinear single-layer MOM6 simulations in a periodic channel, across four
nondimensional parameters: the Rossby number $Ro$, the depth ratio $\hat h$, the slope width
$\hat W$ and the position of the current on the slope $\hat X_j$.

## Repository layout

| Folder | Contents |
|---|---|
| `MOM6_init_files_creation/` | Notebooks that create the MOM6 input files (`ocean_topog.nc`, `init_vel.nc`, `init_eta.nc`) for each experiment: a tanh along-shelf velocity profile over a tanh shelf slope, on a 300 m grid. |
| `MOM6_rundir_example/` | Example MOM6 run directory (payu `config.yaml`, `MOM_input`, `MOM_override`, `diag_table`, `input.nml`): single-layer barotropic configuration, f-plane, 500 × 400 km domain, Laplacian viscosity. |
| `analysis/` | Notebooks for the theory (Ohshima and Dickinson & Clare solvers), the processing of the simulations (growth rate, dominant wavelength, eddy lifetime) and all manuscript figures. See [`analysis/README.md`](analysis/README.md). |

### `MOM6_init_files_creation/`

| Notebook | Experiments | Parameter varied |
|---|---|---|
| `create_ic_160_189_width_tanh_U02__300m.ipynb` | test160–189 | slope width, for $L_{jet}$ = 5, 10, 15 km ($U_0$ = 0.2 m s⁻¹) |
| `create_ic_190_207_height_tanh_U02_300m.ipynb` | test190–207 | slope height (depth ratio $\hat h$), for $L_{jet}$ = 5, 10 km |
| `create_ic_208_227_width_tanh_U02__300m.ipynb` | test208–227 | slope width, for $L_{jet}$ = 5, 10 km ($U_0$ = 0.3 m s⁻¹) |
| `create_ic_228_239_Yj_varies_tanh_U02_300m.ipynb` | test228–239 | jet-centre position $Y_j$ (55 km slope) |
| `create_ic_245_253_rossby_sweep_U_varied_300m.ipynb` | test245–253 | Rossby number via $U_0$ ($\hat W$ = 1) |
| `create_ic_254_262_rossby_sweep_U_varied_300m.ipynb` | test254–262 | Rossby number via $U_0$ ($\hat W$ = 10) |
| `create_ic_263_274_Yj_varies_tanh_U02_300m.ipynb` | test263–274 | jet-centre position $Y_j$ (50 km slope, $\hat W$ = 10) |

The full parameter table for every experiment is `experiment_params` in
`analysis/07_figs4-6_theory_vs_simulations.ipynb`.

