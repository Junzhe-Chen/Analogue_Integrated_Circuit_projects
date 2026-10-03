# Chapter 1: Task 1 - Power Management Unit

[Coursework overview](../README.md) · [Next: Task 2](task-2-semg.md)

## 1.1 Objective and system context

Design a transistor-level power management unit (PMU) in GPDK180 using Cadence Virtuoso. The PMU supplies an electronic system with a stable 1.8 V rail at load currents up to 20 mA despite changes in supply, temperature, process and load.

The PMU contains a bandgap reference (BGR), which generates a nominal 1.25 V reference, and a low-dropout regulator (LDO), which uses that reference to regulate the output. Each block requires a suitable operational amplifier, bias circuitry and a supply arrangement that supports reliable startup.

![Bandgap reference connected to the LDO reference input, with a common external supply and a regulated output.](../assets/PMU_rm_hint.png)

*Figure 1. Example PMU architecture. Connect the BGR output to the LDO reference input; the external supply feeds the power path. The figure's input label should read “unregulated”, and its “±2” annotation is superseded by the < 2 mV maximum-to-minimum temperature variation specified below.*

## 1.2 Required specifications

| Block | Parameter | Target |
| --- | --- | --- |
| LDO | Input voltage | 2.0-2.5 V |
| LDO | Output voltage | 1.8 V |
| LDO | Maximum output current | 20 mA |
| LDO | Quiescent current | < 100 µA |
| LDO | Dropout voltage | < 200 mV |
| LDO | PSRR | ≥ 40 dB at 100 kHz |
| LDO | Integrated output noise | < 100 µV RMS over 0.1-100 Hz |
| LDO | Loop phase margin | ≥ 60° |
| BGR | Supply voltage | 2.5 V maximum |
| BGR | Output voltage | Nominally 1.25 V |
| BGR | Temperature variation | Maximum minus minimum reference voltage < 2 mV over 0-75 °C |
| BGR | PSRR | ≥ 40 dB at 100 kHz |
| BGR | Quiescent current | < 100 µA |

Include operational-amplifier, bias and startup-circuit currents in the corresponding block's quiescent-current budget. Exclude external load current. Identify shared bias currents and count them only once in the total PMU current.

The < 2 mV BGR requirement is the full maximum-to-minimum variation over temperature, not a ±2 mV tolerance. State the supply and reference loading used for this measurement.

## 1.3 Operational-amplifier design

Select and justify an amplifier topology for each block. A two-stage amplifier is a starting option; other architectures are allowed when their suitability is demonstrated. The BGR and LDO amplifiers need not be identical.

![Example two-stage operational amplifier with differential input pair, active loads, second stage and master-current bias.](../assets/OpAmp_two-stage_unspecified_VDD_voltage.png)

*Figure 2. Starting op-amp architecture. Determine device dimensions, bias generation and compensation from the connected block's requirements.*

| Aspect | Required design and verification evidence |
| --- | --- |
| Devices and topology | Use permitted GPDK180 devices; justify architecture and any differences between the BGR and LDO amplifiers. |
| Supply and startup | Verify startup using the block's actual supply arrangement, without assuming an already-established regulated output. |
| Input common-mode range | Demonstrate coverage of the BGR or LDO sensing-node voltages under the assessed operating conditions. |
| Output swing and drive | Verify operation with the connected load, including the LDO pass-transistor gate. |
| DC gain | Report open-loop gain at the relevant operating point and relate it to reference accuracy and output regulation. |
| Bandwidth and slew rate | Report unity-gain bandwidth and slew rate with the specified load; justify them using startup and transient results. |
| Stability | Verify the actual feedback loops. The LDO requires ≥ 60° phase margin; report the BGR loop results as well. |
| Current | Include amplifier and associated bias currents within each complete block's < 100 µA budget. |
| Noise and PSRR | Include amplifier contributions in the corresponding block's overall verification. |

The brief does not prescribe separate numerical op-amp gain, bandwidth or slew-rate targets. Derive suitable values from the PMU requirements and demonstrate the resulting system performance.

## 1.4 BGR, LDO and integration

For the BGR, explain the PTAT and CTAT contributions, bipolar-device area ratios, resistor choices and amplifier feedback. Design a startup circuit and assess non-idealities such as finite output resistance and channel-length modulation. Demonstrate reference generation and temperature behaviour with the actual reference load.

For the LDO, size the pass transistor and feedback divider for the required output voltage, current and dropout. Explain compensation and verify stability with the selected output capacitor and equivalent series resistance (ESR). Evaluate line regulation, load regulation and dynamic load response.

Finally, connect the BGR reference output to the LDO reference input and simulate the complete PMU. Confirm that startup, reference loading and shared biasing remain valid after integration.

## 1.5 Verification checklist

State supply voltage, load current, output capacitance, ESR, reference loading, process corner and temperature for every result. For amplifier characterisation, also state input common mode and output operating point.

- [ ] DC operating points and device operating regions.
- [ ] Supply and load sweeps, line/load regulation and dropout characterisation.
- [ ] BGR reference sweep over 0-75 °C, reporting maximum-to-minimum variation.
- [ ] Startup of the BGR, LDO and integrated PMU.
- [ ] Transient response to supply and load changes, with step amplitudes and edge times stated.
- [ ] LDO and BGR loop-stability analysis under the connected load conditions.
- [ ] PSRR at 100 kHz for each block, supported by frequency-response plots.
- [ ] LDO integrated output noise over 0.1-100 Hz.
- [ ] Quiescent-current accounting, including startup and bias circuits.
- [ ] Process-corner checks at TT, FF and SS, plus temperature sweeps.
- [ ] Integrated PMU verification and a target-versus-result summary with failures explained.

Where the brief does not define a test value, such as output capacitance, ESR or a load-step edge time, state and justify the chosen condition rather than treating it as a supplied specification.

## 1.6 Design-library interface and deliverables

Provide a report section of at most 8 pages and a complete Cadence design library as one ZIP or TAR archive. Include all hierarchy and views needed to reproduce the PMU simulations.

| Interface item | Required value |
| --- | --- |
| Library | `cw_submission` |
| Top-level cell | `pmu_top` |
| Cell views | `schematic` and `symbol` |
| Netlist terminal order | `VDD VSS VOUT` |
| Symbol placement | `VDD` left; `VSS` bottom; `VOUT` right |

| Terminal | Cadence pin direction | Function |
| --- | --- | --- |
| `VDD` | `inputOutput` | External unregulated supply driven by the testbench |
| `VSS` | `inputOutput` | Ground and supply return |
| `VOUT` | `output` | Regulated PMU output |

Terminal names are case-sensitive. Internal cell names and hierarchy are flexible. Preserve this interface for compatibility with the common verification testbench described in the brief. Interface templates and testbench files are separate supporting materials; this chapter does not supply them.

## 1.7 Suggested collaboration and report structure

One contributor can lead the BGR, startup, temperature response and reference line regulation. The other can lead pass-device sizing, the LDO divider, stability and load transients. Both should contribute to amplifier design, initial hand calculations, schematic review, simulation cross-checking, integration and reporting.

Organise the report around:

1. **Operational amplifiers:** topology justification, sizing, biasing, compensation, gain, bandwidth and stability.
2. **BGR:** circuit operation, PTAT/CTAT design, startup and reference-voltage results.
3. **LDO:** pass device, feedback network, regulation, stability and transient results.
4. **Integrated PMU:** reproducible system results, corner comparison, limitations and individual contributions.

## 1.8 Reading

- B. Razavi, *The Design of an LDO Regulator*, IEEE Solid-State Circuits Magazine, 14(2), 7-17, 2022. [DOI](https://doi.org/10.1109/MSSC.2022.3167308) · [Local PDF](../resources/%5BRazavi%5D_The_Design_of_An_LDO_Regulator.pdf).
- [Bandgap-reference design reading](../resources/%5BRazavi%5D_The_Design_of_A_Bandgap_Reference.pdf).
- [Single-stage op-amp design reading](../resources/%5BRazavi%5D_One_Stage_OpAmp.pdf).

[Back to overview](../README.md) · [Continue to Task 2](task-2-semg.md)
