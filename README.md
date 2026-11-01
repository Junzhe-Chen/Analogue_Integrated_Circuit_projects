# Analogue integrated circuit design

Design a power management unit (PMU) and a surface electromyography (sEMG) recording amplifier in GPDK180. Use transistor-level circuits in Cadence Virtuoso and verify them through schematic simulations. Layout and fabrication are outside the project scope.

## Design tasks

| Chapter | Circuit | Main targets |
| --- | --- | --- |
| [Task 1: PMU](docs/task-1-pmu.md) | Bandgap reference, LDO and their operational amplifiers | 1.25 V reference; 1.8 V regulated output up to 20 mA from a 2.0-2.5 V supply |
| [Task 2: sEMG recording amplifier](docs/task-2-semg.md) | One differential recording channel | 1.8 V supply; 40 dB gain; 10 Hz to 1 kHz nominal passband; quiescent current ≤ 20 µA |

## System context

The intended application is a forearm-worn gesture interface. Wet Ag/AgCl electrodes detect muscle activity; the recording amplifier rejects electrode DC offsets and conditions the differential signal for an ADC. A processor then classifies the activity pattern. Published examples include an EMG-based AR/VR system and embedded gesture learning. [VLSI 2023](https://doi.org/10.23919/VLSITechnologyandCir57934.2023.10185392), [TBioCAS 2019](https://doi.org/10.1109/TBCAS.2019.2914476).

The BGR supplies the LDO with a nominal 1.25 V reference, and the LDO delivers the recording amplifier's 1.8 V rail. Reference noise and supply disturbances can affect the acquired signal, so verify the complete PMU with the actual reference and load. The 20 mA LDO rating represents supply capacity for a larger system; one recording channel draws at most 20 µA. ADC and classifier design remain outside the project scope.

Read the [system description and IEEE papers](docs/system-context.md) for the signal and power paths, the reasons for the design targets, and examples from ISSCC, JSSC, VLSI, CICC, TCAS-I and TBioCAS.

![Forearm electrodes, recording amplifier, ADC and gesture recognition, with PMU support.](assets/AFE_system_diagram_rm_hint.png)

*The three-op-amp circuit illustrates one possible recording amplifier. Its repeated A2 label denotes separate amplifiers. The amplifier's output-bias reference and the ADC reference have distinct functions.*

## Design method

Start with hand calculations for device sizing and bias currents. Adapt published circuits to GPDK180, then check the operating points and complete feedback loops. Record simulation conditions and compare each result with its target, explaining any shortfall.

Task-specific reading is linked in each chapter. Supporting papers are in [resources/articles](resources/articles/), and the [gm/ID guide](resources/useful_techniques/Analysis_and_Implementation_of_gm_over_id_Methodology.pdf) covers a transistor-sizing method.
