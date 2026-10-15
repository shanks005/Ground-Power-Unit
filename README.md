# Aircraft Ground Power Unit (GPU) — Design and Simulation

**PLECS simulation of a power-electronic aircraft ground power unit** providing a three-phase **115 V RMS phase-to-neutral / 200 V RMS line-to-line, 400 Hz AC** supply and a nominal **28 V DC** supply. The objective is to power aircraft electrical loads while the aircraft is on the ground, reducing dependence on onboard engine/APU operation.

> **Scope:** Circuit-level modelling and simulation only; no hardware prototype, aircraft qualification or certification is claimed. The plots below are extracted from the project report.

## Design requirements

| Output | Target | Simulation result reported |
|---|---|---|
| Three-phase AC (phase-to-neutral) | 115 V RMS | 115.957 V RMS for phase A (Table 21) |
| Three-phase AC (line-to-line) | 200 V RMS | 200.843 V RMS for line AB (Table 21) |
| AC fundamental frequency | 400 Hz | 400 Hz (Table 21) |
| AC voltage THD | < 5% | 0.8% (Table 21) |
| DC output | 28 V | 27.8729 V (Table 22) |
| DC output ripple | Low | 0.1167 V peak-to-peak (Table 22) |

Values above reproduce **Tables 21–22** of the [project report](https://github.com/shanks005/Ground-Power-Unit/blob/main/Ground_Power_Unit_Report.pdf); the report's summary **Table 23** contains some differing numerical entries. The reported values should therefore be treated as simulation results, not independently validated measurements.

## System architecture

The PLECS schematic implements two supply paths from the AC input:

**AC path**

```text
Three-phase AC → Diode rectifier → DC-link / boost converter
               → Three-phase VSI (PWM) → LC filter → 400 Hz three-phase AC
```

**DC path**

```text
Three-phase AC → Delta–delta step-down transformer → Diode rectifier
               → DC-link filter → Buck converter → 28 V DC
```

The boost and buck stages employ feedback control in the schematic, while the three-phase inverter is driven with phase-shifted sinusoidal references and a triangular carrier.

### PLECS schematic

[![Ground Power Unit PLECS schematic](https://github.com/shanks005/Ground-Power-Unit/blob/main/gpu_schematic.png)

*Figure 13 of the report, p. 17. Select the image to view at full resolution.*

## Main simulation parameters

The following values are taken from **Table 20** of the report:

| Parameter | Simulated value |
|---|---:|
| Input AC frequency | 50 Hz |
| Inverter switching frequency | 10 kHz |
| AC output frequency | 400 Hz |
| AC filter inductor | 0.2 mH |
| AC filter capacitor | 1 µF |
| Buck switching frequency | 25 kHz |
| Buck inductor | 1 mH |
| Buck output capacitor | 200 µF |

## Simulation figures

### Three-phase inverter output

[![Three-phase inverter voltage and current waveforms](https://github.com/shanks005/Ground-Power-Unit/blob/main/115V_Inverter_Output.pdf)

*Unfiltered and filtered three-phase inverter voltages and load currents (report Figure 15, p. 22).*

### 28 V DC output

[![Buck converter output voltage](https://github.com/shanks005/Ground-Power-Unit/blob/main/buck_converter_output.pdf)

*Buck converter output voltage versus time (report Figure 19, p. 26).*

### Additional plots

| Plot | Link | Source |
|---|---|---|
| Rectifier / boost-stage voltage | [View waveform](https://github.com/shanks005/Ground-Power-Unit/blob/main/3ph_rectifier_boost_output.pdf) | Figure 14, p. 21 |
| AC voltage and current frequency spectrum | [View FFT spectrum](https://github.com/shanks005/Ground-Power-Unit/blob/main/115V_thd_analysis.pdf) | Figure 16, p. 23 |
| Input and step-down transformer voltages | [View waveforms](https://github.com/shanks005/Ground-Power-Unit/blob/main/grid_transformer_output.pdf) | Figure 17, p. 24 |
| Rectified intermediate DC voltage (~57 V) | [View waveform](https://github.com/shanks005/Ground-Power-Unit/blob/main/3ph_rectifier_output.pdf) | Figure 18, p. 25 |

> **Figure-label clarification:** Report Figure 16 is captioned as a buck-converter output, but the plotted x-axis is frequency and the legends are three-phase voltage/current signals. It is linked above as a frequency spectrum rather than a buck-converter waveform.

## Documentation

- **[Project report (PDF)](https://github.com/shanks005/Ground-Power-Unit/blob/main/Ground_Power_Unit_Report.pdf)** — see **Section 5, pp. 17–27**, for the GPU design, model, parameters and results.
- **[Simulation schematic](https://github.com/shanks005/Ground-Power-Unit/blob/main/gpu_schematic.png)** — extracted at its native resolution.
- **[Plot index](#simulation-figures)** — quick access to the waveforms.

## Repository structure

```text
.
├── README.md
├── docs/
│   └── Ground_Power_Unit_Report.pdf
└── figures/
    ├── gpu_schematic.png
    ├── rectifier_boost_voltage.png
    ├── three_phase_inverter_waveforms.png
    ├── ac_harmonic_spectrum.png
    ├── transformer_voltage_waveforms.png
    ├── rectified_dc_57v.png
    └── buck_output_28v.png
```

## Software and reproducibility

- **PLECS** — source of the simulation schematic and plotted results in this report.
- The original editable PLECS model is **not included** in this package. Add it under `models/` if available; the figures are images and cannot be used to rerun the simulation.

## Project status

**Simulation study.** Future work includes load-step testing, improved quantitative cross-checking of THD and regulation results, detailed loss/thermal evaluation, and hardware-level protection and compliance assessment.
