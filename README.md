# Aircraft Ground Power Unit (GPU) — Design and Simulation

A simulation-based design of an **aircraft ground power unit (GPU)** intended to supply electrical power to an aircraft while it is on the ground. The project investigates the power-electronic stages required to provide both **400 Hz AC power** and **regulated 28 V DC power**, using **MATLAB/Simulink** and **PLECS**.

> **Project scope:** Converter modelling and simulation. The repository does not claim hardware implementation or certification to an aircraft power standard.

## Design targets

| Output | Nominal requirement | Intended application |
|---|---|---|
| Three-phase AC | 115 V RMS phase-to-neutral / 200 V RMS line-to-line, 400 Hz | Aircraft AC distribution |
| DC | 28 V | Aircraft DC distribution and auxiliary loads |

These are **design targets**, not measured results. Actual regulation, distortion, ripple and transient performance must be established from simulation outputs.

## System overview

The model studies two main power-delivery paths:

1. **AC output path:** A voltage-source inverter (VSI) synthesizes a 400 Hz three-phase output. A sub-cycle-averaged VSI model is used to study system-level behaviour without resolving every switching transition.
2. **DC output path:** Rectification and DC–DC conversion stages are investigated to obtain a regulated 28 V output. Modelled building blocks include a diode rectifier, an LLC resonant converter and a buck converter.

The detailed interconnection and control of these stages are documented in the simulation models.

## Engineering work

- Defined the AC and DC output requirements for aircraft ground-power operation.
- Modelled the inverter and DC–DC conversion stages in MATLAB/Simulink and PLECS.
- Investigated voltage conversion, output regulation and the response of the stages to load conditions.
- Identified simulation challenges associated with switching transitions, solver settings and model complexity.

## Simulation and verification

The following quantities are relevant when validating the design:

| Subsystem | Verification parameters |
|---|---|
| AC output | Phase and line RMS voltages, frequency, waveform quality, total harmonic distortion (THD) |
| DC output | Average output voltage, ripple, regulation and load-step response |
| Power stages | Device stresses, converter losses and efficiency, where modelled |
| Overall system | Startup behaviour, numerical stability and response to load changes |

**Note:** Numerical performance results are intentionally not listed here because validated plots and measurements have not been supplied.

## Tools

- **MATLAB/Simulink** — system-level modelling and simulation
- **PLECS** — power-electronic converter modelling and analysis

## Getting started

1. Open the relevant model in MATLAB/Simulink or PLECS.
2. Review the input source, stage parameters and load settings.
3. Run the simulation and inspect the AC output voltages and DC output voltage.
4. Compare the observed results against the design targets above.

Simulation files, plots and any initialization scripts should be kept together so the operating conditions can be reproduced.

## Recommended repository structure

```text
.
├── README.md
├── models/       # Simulink and PLECS project files
├── calculations/ # Converter and component design calculations
├── results/      # Waveforms and performance plots
└── docs/         # Project report and supporting notes
```

## Future work

- Complete quantitative verification of voltage regulation, THD and DC ripple.
- Evaluate performance across load conditions and startup transients.
- Compare switching-level and averaged converter models.
- Assess protection requirements and hardware feasibility.

## Status

**Simulation and design study.** Any hardware implementation or aircraft-standard compliance assessment is outside the scope of the results presented here.
