# Fuzzy Sliding-Mode Control of a Lower-Limb Exoskeleton

## Overview

This repository contains a MATLAB/Simulink implementation of a fuzzy sliding-mode controller (FSM) for trajectory tracking of a lower-limb exoskeleton joint. The simulation combines a sliding-mode control structure with Mamdani fuzzy inference systems that adjust the controller coefficients according to the tracking and sliding conditions.

The model is intended for research, teaching, and simulation studies of intelligent control for rehabilitation exoskeletons. It includes the reference trajectory, tracking-error calculation, fuzzy adaptation blocks, controller computation, identified joint transfer-function model, scopes, and workspace data logging.

## Repository Contents

| File | Description |
| --- | --- |
| `fsm_simulink.slx` | Main Simulink model containing the reference trajectory, error calculation, fuzzy controllers, controller dynamics, exoskeleton joint model, scopes, and data-logging blocks. |
| `slide.fis` | Mamdani fuzzy inference system used by the fuzzy controller blocks in the Simulink model. |

## Controller Structure

The desired joint trajectory is compared with the simulated joint response to obtain the tracking error. The error and its derived signals are used to form the sliding variable. Two fuzzy-controller instances process pairs of controller signals and generate adaptive coefficients used in the control law. The resulting control input is applied to the identified joint transfer-function model, and the joint response is returned through the feedback loop.

The supplied fuzzy inference system has the following configuration:

- Type: Mamdani
- Inputs: `s` and `ds`
- Output: `pwm`
- Input range: `[-30, 30]`
- Output range: `[0, 12]`
- Membership functions: five triangular membership functions for each input and the output
- Rule base: 25 rules
- AND method: minimum
- OR method: maximum
- Implication method: minimum
- Aggregation method: maximum
- Defuzzification method: centroid

## Software and Hardware Requirements

The simulation was developed and tested using:

- Windows 11
- MATLAB/Simulink R2023b
- Fuzzy Logic Toolbox
- AMD Ryzen 5 processor
- 16 GB RAM

MATLAB R2023b or a compatible newer release is recommended.

## How to Run the Simulation

1. Download or clone the repository.
2. Open MATLAB and set the repository folder as the current working directory.
3. Load the fuzzy inference system into the MATLAB base workspace:

   ```matlab
   slide = readfis("slide.fis");
   ```

4. Open the Simulink model:

   ```matlab
   open_system("fsm_simulink.slx");
   ```

5. Run the model from Simulink, or execute it from the MATLAB Command Window:

   ```matlab
   sim("fsm_simulink.slx");
   ```

The model is configured for a simulation time of 20 seconds using a variable-step solver.

## Logged Simulation Signals

After the simulation, the following time-series variables are available in the MATLAB workspace:

| Variable | Description |
| --- | --- |
| `desired` | Desired joint trajectory |
| `Actual` | Simulated actual joint trajectory |
| `error` | Joint trajectory-tracking error |
| `de` | Derived tracking-error signal |
| `s` | Sliding variable |
| `ds` | Derived sliding-variable signal |
| `lambda` | First fuzzy-adjusted controller coefficient |
| `kf` | Second fuzzy-adjusted controller coefficient |
| `tau` | Generated controller input |

The desired and actual trajectories can be plotted after simulation using:

```matlab
figure;
plot(desired.Time, desired.Data, "--", "LineWidth", 1.5);
hold on;
plot(Actual.Time, Actual.Data, "LineWidth", 1.5);
grid on;
xlabel("Time (s)");
ylabel("Joint angle");
legend("Desired", "Actual");
```

## Notes

- Keep `slide.fis` and `fsm_simulink.slx` in the same folder unless the MATLAB path is configured accordingly.
- The Fuzzy Logic Toolbox is required because the model uses Fuzzy Logic Controller blocks.
- The model is provided for simulation and research purposes. Hardware implementation requires appropriate actuator limits, sampling-time selection, sensor filtering, safety constraints, and experimental validation.
- Controller parameters, membership-function ranges, fuzzy rules, reference trajectories, and plant coefficients can be modified for other joints or operating conditions.

## Related Research

This implementation accompanies research on adaptive fuzzy sliding-mode trajectory control for lower-limb rehabilitation exoskeletons. If you use or modify this model in academic work, please cite the associated manuscript and acknowledge this repository.

## Disclaimer

This software is a research prototype and is not a certified medical device. It must not be used for human rehabilitation or clinical operation without appropriate safety analysis, hardware validation, ethical approval, and regulatory compliance.

## Authors and Affiliations

- **Sahbi Boubaker** — Corresponding author  
  Department of Computer & Network Engineering, College of Computer Science and Engineering, University of Jeddah, Jeddah, Saudi Arabia  
  King Salman Center for Disability Research, Riyadh, Saudi Arabia  
  Email: sboubaker@uj.edu.sa

- **Mohammad Soleimani Amiri**  
  Faculty of Artificial Intelligence and Cyber Security, Universiti Teknikal Malaysia Melaka, Melaka, Malaysia

- **Rizauddin Ramli**  
  Department of Mechanical and Manufacturing Engineering, Faculty of Engineering and Built Environment, Universiti Kebangsaan Malaysia, Selangor, Malaysia

- **Habib Dimassi**  
  Laboratory of Automatics, Electrical Systems and Environment (LASEE), Electrical Engineering Department, National Engineers School of Monastir, University of Monastir, Monastir, Tunisia

- **Salim Hadj Said**  
  Laboratory of Automatics, Electrical Systems and Environment (LASEE), Electrical Engineering Department, National Engineers School of Monastir, University of Monastir, Monastir, Tunisia
