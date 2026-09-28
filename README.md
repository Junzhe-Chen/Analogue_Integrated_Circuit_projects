# Analogue Integrated Circuit Coursework

## Aim and approach

The basic requirements should be achievable for most students who have studied the course. More ambitious students can earn additional marks through advanced design features.

Use the circuit structures, design methods and specifications in Razavi’s article as a starting point. Students must adapt the designs to the available PDK and determine device sizes through their own design and simulation work. Copying the article’s dimensions is insufficient: for example, a 140 nm channel length is not available in a PDK with a minimum channel length of 180 nm.

A 1.8 V supply is proposed. Ambitious students are encouraged to explore operation at 1.2 V to reduce power consumption, while recognising the greater design challenge in device operation region.

## 0. TODO: 

 - [ ] op-amp design specs
 - [ ] circuit diagram drawing
 - [ ] descriptions of the extra tasks, alongside with references

## Op-amp circuit options

The following diagrams show two starting topologies for the op-amp design. Compare their gain, stability, biasing and voltage-headroom requirements before choosing an implementation. The op-amp specifications remain to be confirmed in the instructor checklist above.

### Two-stage op-amp

![Two-stage op-amp with an NMOS differential input pair, resistive connections in the PMOS load, a second gain stage and master-current bias.](assets/TASK_1_two_stage_opamp_circuit_bg.png)

*Figure 1. Two-stage op-amp starting circuit at 1.8 V. Determine the device sizes and bias currents, and investigate where compensation is needed to meet the stability requirements.* [View SVG](assets/TASK_1_two_stage_opamp_circuit.svg)

### Folded-cascode op-amp

![Folded-cascode op-amp with a PMOS differential input pair, two output branches, master-current bias and four labelled bias voltages.](assets/TASK_1_folded_cascode_opamp_circuit_bg.png)

*Figure 2. Folded-cascode op-amp starting circuit at 1.8 V. Check the bias voltages and saturation margins throughout the signal path; consider the complementary topology if the required input common-mode range calls for it.* [View SVG](assets/TASK_1_folded_cascode_opamp_circuit.svg)

## 1. Low-dropout regulator (LDO)

The error amplifier drives the PMOS pass transistor so that the feedback-divider voltage follows the reference. Either op-amp topology above can be investigated as the error amplifier; verify stability with the pass transistor and load included.

![LDO with a PMOS pass transistor controlled by error amplifier A1, reference voltage Vref and output-feedback divider R1 and R2.](assets/TASK_2_LDO_circuit_bg.png)

*Figure 3. LDO starting architecture. The diagram shows the regulator feedback path; include the load and any chosen compensation or output capacitance in the simulation testbench.* [View SVG](assets/TASK_2_LDO_circuit.svg)

### Proposed specifications

| Parameter | Proposed specification |
| --- | --- |
| Input voltage | 2.5 V |
| Output voltage | 1.8 V |
| Maximum output current | 5 mA |
| PSRR | 40 dB at 1 MHz |
| Output noise spectral density | < 100 nV/√Hz at 1 MHz |

## 2. Bandgap reference

The circuit below is the starting topology for Task 3. The op-amp forces the two sensing nodes toward the same voltage, while the unequal bipolar-junction areas and resistor network generate the PTAT and CTAT contributions used to obtain a temperature-stable reference. Students should size the circuit, add a reliable startup network and assess the effect of finite output resistance.

![Task 3 bandgap reference with op-amp A1, bipolar transistors Q1 and Q2, resistor network, PMOS current mirror and buffered 1.25 V output.](assets/TASK_3_bandgap_reference_circuit_bg.png)

*Figure 4. Task 3 bandgap-reference starting circuit at 1.8 V. The startup circuit is intentionally omitted and must be designed and verified by the student.* [View SVG](assets/TASK_3_bandgap_reference_circuit.svg)

### Proposed specifications

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

- Confirm the LDO figure annotations against the specification table: the figure shows a 2–2.5 V input range and 0–75 °C operation, and its “±2” output-tolerance label needs a unit.
- Add the full reference or link to Razavi’s article.
- Specify the PSRR test frequencies for the bandgap reference and recording amplifier, and the CMRR test frequency for the amplifier.
- Define the integration bandwidth for input-referred RMS noise and the input amplitude and frequency for THD testing.
- Clarify whether the 400 mV input-offset specification refers to allowable electrode DC offset or the amplifier’s own input-referred offset.
- Confirm which blocks are eligible for the optional 1.2 V supply challenge, since a 1.25 V reference output requires reconsideration at that supply.
