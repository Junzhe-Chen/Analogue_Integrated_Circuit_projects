# Task 2: sEMG recording amplifier

[Overview](../README.md) · [System context and papers](system-context.md) · [Task 1](task-1-pmu.md)

## Circuit

Design one transistor-level recording channel in GPDK180 using Cadence Virtuoso. It amplifies differential muscle signals from wet Ag/AgCl forearm electrodes for digitisation and gesture recognition [1]. The PMU supplies 1.8 V; this task covers the amplifier and its local bias and common-mode circuits.

## Specifications

| Parameter | Target |
| --- | --- |
| Supply voltage | 1.8 V |
| Channel configuration | Differential input; single-ended or fully differential output |
| Voltage gain | 40 dB; gain error < 0.5 dB at 100 Hz |
| Lower -3 dB cutoff | 10 ± 2 Hz |
| Upper -3 dB cutoff | 1 ± 0.2 kHz |
| Input-referred noise | < 5 µV RMS integrated over 10-1000 Hz |
| Linearity | THD < 0.5% at 100 Hz with a 2 mV peak-to-peak differential input |
| Differential electrode DC offset | Tolerate -50 mV to +50 mV after settling, without saturation, while retaining the specified gain and THD |
| CMRR | > 60 dB at 500 Hz in nominal matched simulation |
| PSRR | > 40 dB at 500 Hz |
| Input impedance | ≥ 10 MΩ at 500 Hz |
| Channel current | ≤ 20 µA total quiescent current, including local bias and common-mode circuitry; excludes the LDO and BGR |
| Stability | Phase margin ≥ 45° if feedback is used |
| Load evaluation | Assess operation with 1-5 pF capacitive loads |
| Area | Record total capacitance and estimated device/passive area; no layout or fabricated-area requirement |

Use the 10 ± 2 Hz lower cutoff in the brief's Table 6; its system overview states 20 Hz. Electrode DC offset is an external differential voltage, distinct from the OTA's intrinsic input offset.

At 1.8 V, the current limit gives 36 µW maximum quiescent power. A 40 dB voltage gain is 100, so a 2 mV peak-to-peak input produces approximately 200 mV peak-to-peak output at 100 Hz.

## Architecture

Select a topology that meets the complete channel specifications. Use hand calculations to set transistor sizes, bias currents and component values, then explain the resulting trade-offs.

| Option | Design considerations |
| --- | --- |
| Current-feedback instrumentation amplifier (CFIA) [2] | Input common mode, current feedback, offset handling, noise and power. |
| Capacitively coupled instrumentation amplifier (CCIA) [3] | Capacitor ratios, DC feedback, lower cutoff, input impedance and optional chopping. |
| Three-op-amp instrumentation amplifier [4] | Headroom at 1.8 V, electrode-offset rejection and the 20 µA total-current limit. |
| Inverter-based amplifier [5] | Bias and common-mode control; gain and bandwidth over process and supply variations. |

![Example capacitively coupled and current-feedback instrumentation amplifiers.](../assets/IA_example_two.png)

*Example architectures. Implement the bias and feedback circuits needed for GPDK180 and verify the complete channel.*

Establish the DC operating point and reject electrode offsets. Set both cutoff frequencies and check operation with the output load. Include common-mode feedback where required by the output configuration. Chopping is optional; alternative architectures are allowed.

## Simulation checks

Record supply voltage, input common mode, output operating point, electrode offset, input stimulus, output load, process corner and temperature. State the limits of any supply sweep; the brief does not prescribe them.

| Analysis | Check |
| --- | --- |
| DC operating point | Bias currents, device regions, input common mode and output swing. |
| AC response | Gain at 100 Hz and both -3 dB cutoffs; state the reference gain used for the cutoffs. |
| Noise | Input-referred spectrum and RMS noise over 10-1000 Hz, including bias and feedback-network noise. Distinguish amplifier noise from electrode-source noise. |
| Transient and distortion | THD for a 100 Hz, 2 mV peak-to-peak differential input after settling. |
| Electrode-offset sweep | Retain gain and THD limits without saturation after settling across -50 mV to +50 mV. |
| Interference rejection | CMRR and PSRR at 500 Hz; state stimuli and gain-ratio definitions. |
| Input impedance | Value at 500 Hz; state the per-input or differential convention. The brief leaves this undefined, so preferably record both. |
| Current | Total quiescent current, including local bias and common-mode circuits. |
| Stability and load | Loop phase margin where feedback is used; operation with 1-5 pF loads. |
| Corners | TT, FF and SS, supply variations and load-capacitance variations. |

Compare results with every specification and explain any shortfall. Record total capacitance and estimated device/passive area with the estimation method.

## Reading

1. X. Tang et al., *Hand Motion Classification Using a Multi-Channel Surface Electromyography Sensor*, Sensors, 2012. [DOI](https://doi.org/10.3390/s120201130).
2. R. F. Yazicioglu et al., *A 60 µW 60 nV/√Hz Readout Front-End for Portable Biopotential Acquisition Systems*, JSSC, 2007. [DOI](https://doi.org/10.1109/JSSC.2007.894804).
3. Q. Fan et al., *A 1.8 µW 60 nV/√Hz Capacitively-Coupled Chopper Instrumentation Amplifier in 65 nm CMOS for Wireless Sensor Nodes*, JSSC, 2011. [DOI](https://doi.org/10.1109/JSSC.2011.2143610).
4. Analog Devices, *MT-063: Basic Three Op Amp In-Amp Configuration*. [Tutorial](https://www.analog.com/media/en/training-seminars/tutorials/MT-063.pdf).
5. R. Vieira et al., *A Tunable Gain and Bandwidth Low-Noise Amplifier with 1.44 NEF for EMG and EOG Biopotential Signal*, Electronics, 2023. [DOI](https://doi.org/10.3390/electronics12122592).

Further examples: [Harrison and Charles](../resources/articles/%5BRecording_Amp%5D_Baseline_Harrison_and_Charles.pdf) and [Denison et al.](../resources/articles/%5BRecording_Amp%5D_Chopping_Denison_et_al.pdf).

[Overview](../README.md) · [Task 1](task-1-pmu.md)
