DC Motor Speed Control Using MPC and Classical Controllers
A Python-based DC motor simulation project for studying and comparing
Model Predictive Control (MPC) with P, PD, PI, and PID
controllers. The project focuses on speed tracking, motor response
visualization, disturbance handling, and operating constraints such as
armature voltage and current limits.

Overview
The simulation models the electrical and mechanical dynamics of a DC
motor and evaluates how different controllers regulate angular speed. An
interactive Jupyter Notebook GUI allows users to change motor
parameters, controller gains, target speed, MPC horizons, objective
weights, and selected limits.
Main features
- DC motor state-space model.
- Controller selection: P, PD, PI, PID, and MPC.
- MPC prediction and control horizons.
- Adjustable MPC weights for tracking error and control effort.
- Configurable target speed and motor parameters.
- Voltage, current, and speed constraint handling for MPC.
- Reference tracking and load-disturbance simulation, where
  configured.
- Plots for reference/actual speed, armature current, angular
  position, and applied voltage.
- Performance evaluation using rise time, settling time, overshoot,
  steady-state error, RMSE, and constraint checks.
- Interactive controls using ipywidgets.
Note: Results depend on the motor parameters, sampling time,
controller tuning, optimization settings, and simulation scenario. Add
measured results and screenshots from your actual run before
presenting quantitative performance claims.

Controllers
  Controller              Control law             Purpose
                          (conceptual)            
  P                       Proportional action on  Simple tracking
                          speed error             response

  PD                      Proportional and        Improve damping and
                          derivative actions      transient response

  PI                      Proportional and        Reduce steady-state
                          integral actions        error

  PID                     Proportional, integral, Combine tracking
                          and derivative actions  correction and
                                                  transient shaping

  MPC                     Optimize predicted      Track the reference
                          future behavior over a  while considering
                      finite horizon          configured constraints
For the classical controllers, the speed error is
\[

\]
The MPC objective typically balances speed-tracking error and control
effort. Depending on the implementation, it can also penalize changes in
the applied voltage.
DC Motor Model
The motor is represented by electrical and mechanical equations:

\(V_a = R_a i_a + L_a`\frac{di_a}{dt}`{=tex} + K_e`\omega`{=tex}\)

\(J`\frac{d\omega}{dt}`{=tex} = K_t i_a - b`\omega `{=tex}- T_L\)

\(`\frac{d\theta}{dt}`{=tex} = `\omega`{=tex}\)

where:
- (V_a): armature voltage (V)
- (i_a): armature current (A)
- (R_a): armature resistance (Ω)
- (L_a): armature inductance (H)
- (K_e): back-EMF constant
- (K_t): torque constant
- (\omega{=tex}): angular speed (rad/s)
- (\theta{=tex}): angular position (rad)
- (J): rotor/load inertia (kg·m²)
- (b): viscous friction coefficient
- (T_L): load torque
The MPC implementation uses a discrete-time prediction model derived
from the motor dynamics.
MPC Objective and Constraints
A representative finite-horizon cost function is
\[
Q(`\hat{\omega}`{=tex}*{k+j}-r\_{k+j})\^2 + `\sum`{=tex}*{j=0}\^{N_c-1}
R(`\Delta `{=tex}u*{k+j})\^2 + `\sum`{=tex}*{j=0}\^{N_c-1} S u*{k+j}\^2
\]
where (N_p) is the prediction horizon, (N_c) is the control horizon, (Q)
weights tracking error, (R) weights changes in control input, and (S)
weights control effort. The exact objective depends on the
implementation in the notebook.

Typical operating constraints are:

\(-V\_{`\max`{=tex}} `\leq `{=tex}u_k `\leq `{=tex}V\_{`\max`{=tex}}\)
`\leq `{=tex}I\_{`\max`{=tex}} \]

\[ `\omega`{=tex}\_{`\min`{=tex}} `\leq `{=tex}`\omega`{=tex}*k
`\leq `{=tex}`\omega`{=tex}*{`\max`{=tex}} \]

The implementation should be checked to confirm which constraints are
enforced directly by the optimizer and which are only monitored after
simulation.

## Requirements

-   Python 3
-   Jupyter Notebook or Google Colab
-   NumPy
-   SciPy
-   pandas
-   Matplotlib
-   ipywidgets

## Installation

### 1. Clone the repository

``` bash
git clone https://github.com/Ayushmaantiwari/motorsim.git
cd motorsim
```

### 2. Create and activate a virtual environment (optional)

**Windows PowerShell**

``` powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
```

**Linux/macOS**

``` bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install dependencies

``` bash
python -m pip install numpy scipy pandas matplotlib ipywidgets jupyter
```

If the notebook contains an installation cell, you may run that cell
instead. For Jupyter environments that require it, enable or configure
`ipywidgets` according to the environment's documentation.

## How to Run

1.  Open the project notebook in Jupyter Notebook, JupyterLab, or Google
    Colab.
2.  Run the dependency-installation and import cells.
3.  Run the simulation/GUI cells in order.
4.  Select a controller: P, PD, PI, PID, or MPC.
5.  Set the motor parameters and target speed.
6.  For classical controllers, tune the available gains.
7.  For MPC, configure the prediction horizon, control horizon,
    objective weights, and available constraints.
8.  Run the simulation and inspect the response plots and performance
    metrics.
9.  Repeat with the same reference and disturbance profile to compare
    controllers fairly.

**Reproducibility tip:** Use the same motor parameters, sampling time,
initial state, reference trajectory, load disturbance, and actuator
limits when comparing controllers.

## Performance Metrics

  -----------------------------------------------------------------------
  Metric                              What it measures
  ----------------------------------- -----------------------------------
  Rise time                           Time for speed to rise from 10% to
                                      90% of the target change

  Settling time                       Time required to remain within a
                                      chosen tolerance band, such as ±2%

  Overshoot                           Amount by which speed exceeds its
                                      final target

  Steady-state error                  Difference between reference and
                                      final measured speed

  RMSE                                Root mean squared speed-tracking
                                      error over the simulation

  Peak current                        Maximum absolute armature current

  Voltage limit check                 Whether applied voltage remains
                                      within configured bounds

  Constraint violations               Number or duration of
                                      current/speed/voltage limit
                                      violations
  -----------------------------------------------------------------------

No controller should be described as the best until the metrics have
been calculated from the actual simulation output.

## Example Results

Add plots exported from your simulation to a folder such as
`docs/images/`, then embed them below. Update the filenames to match the
files in your repository.

``` markdown
![Controller comparison](docs/images/controller_comparison.png)
![Armature current](docs/images/armature_current.png)
![Applied voltage](docs/images/applied_voltage.png)
![Angular position](docs/images/angular_position.png)
```

### Results table

Replace the placeholders with measurements from your run.

  Metric                           P    PD    PI   PID   MPC
  ---------------------------- ----- ----- ----- ----- -----
  Rise time (s)                  ---   ---   ---   ---   ---
  Settling time (s)              ---   ---   ---   ---   ---
  Overshoot (%)                  ---   ---   ---   ---   ---
  Steady-state error (rad/s)     ---   ---   ---   ---   ---
  RMSE (rad/s)                   ---   ---   ---   ---   ---
  Peak current (A)               ---   ---   ---   ---   ---

## Suggested Repository Structure

Adjust this example to match the actual files in the repository.

``` text
motorsim/
├── README.md
├── notebooks/
│   └── dc_motor_simulation.ipynb
├── docs/
│   └── images/
└── report/
    └── dc_motor_mpc_report.tex
```

## Limitations

-   Simulation fidelity depends on the motor model and parameter
    accuracy.
-   MPC performance depends on the objective weights, horizons, solver,
    and feasibility of the optimization problem.
-   Classical controllers may require anti-windup and output saturation
    handling.
-   Constraint guarantees depend on the implementation and optimizer
    success.
-   Simulation results alone do not establish real-hardware performance.

## Future Improvements

-   Add automatic gain tuning for P, PD, PI, and PID.
-   Compare multiple MPC horizons and objective-weight settings.
-   Add noise and model-parameter uncertainty.
-   Export plots and metric summaries automatically.
-   Add hardware-in-the-loop or physical motor validation.


## Author

**Ayushmaan Tiwari**


------------------------------------------------------------------------

If you use or adapt this project, please cite the repository and
describe any modifications you make.
