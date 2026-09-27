# Analogue Integrated Circuit Coursework

## Aim and approach

The basic requirements should be achievable for most students who have studied the course. More ambitious students can earn additional marks through advanced design features.

Use the circuit structures, design methods and specifications in Razavi’s article as a starting point. Students must adapt the designs to the available PDK and determine device sizes through their own design and simulation work. Copying the article’s dimensions is insufficient: for example, a 140 nm channel length is not available in a PDK with a minimum channel length of 180 nm.

A 1.8 V supply is proposed. Ambitious students are encouraged to explore operation at 1.2 V to reduce power consumption, while recognising the greater design challenge in device operation region.

## 0. TODO: 

 - [ ] op-amp design specs
 - [ ] circuit diagram drawing
 - [ ] descriptions of the extra tasks, alongside with references

## 1. Low-dropout regulator (LDO)

| Parameter | Proposed specification |
| --- | --- |
| Input voltage | 2.5 V |
| Output voltage | 1.8 V |
| Maximum output current | 5 mA |
| PSRR | 40 dB at 1 MHz |
| Output noise spectral density | < 100 nV/√Hz at 1 MHz |

## 2. Bandgap reference

| Parameter | Proposed specification |
| --- | --- |
| Supply voltage | 1.8 V |
| Output voltage | 1.25 V |
| Output voltage variation | < 5 mV over 0–100 °C |
| PSRR | 40 dB |
| Power consumption | < 1 mW |

## 3. Recording amplifier

### Application scenario

Design a recording amplifier for a neural-signal acquisition front end. It should amplify small differential signals from electrodes and provide a 10 kHz upper −3 dB cutoff frequency before digitisation. Consider how electrode offsets, noise and the capacitive load of the next stage affect the design.

### Basic specifications

| Parameter | Proposed specification |
| --- | --- |
| Supply voltage | 1.8 V |
| Channel configuration | Differential input; single-ended output acceptable, differential output preferred |
| Voltage gain | 40 dB, with gain error < 1 dB |
| Upper −3 dB cutoff frequency | 10 kHz |
| Input-referred noise | < 5 µV RMS |
| Linearity | THD < 0.5% |
| Input offset | < 400 mV |
| CMRR | > 60 dB |
| PSRR | > 40 dB |
| Stability | Report phase margin (PM) and assess stability for load capacitances of 1–5 pF |
| Area | Report area and discuss capacitance usage |

### Extension features and marking expectations

A successful basic design without additional functions should be capable of earning approximately **65%**.

Here are some extra functions to be implemented in order to get extra marks 

- Gain control.
- Chopping.
- Dynamic offset correction or offset calibration.

 The quality of its implementation and evaluation should inform the final mark.

## Details to confirm before issuing the brief

- Add the full reference or link to Razavi’s article.
- Specify the PSRR test frequencies for the bandgap reference and recording amplifier, and the CMRR test frequency for the amplifier.
- Define the integration bandwidth for input-referred RMS noise and the input amplitude and frequency for THD testing.
- Clarify whether the 400 mV input-offset specification refers to allowable electrode DC offset or the amplifier’s own input-referred offset.
- Confirm which blocks are eligible for the optional 1.2 V supply challenge, since a 1.25 V reference output requires reconsideration at that supply.
