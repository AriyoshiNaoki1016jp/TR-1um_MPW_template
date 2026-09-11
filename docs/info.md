# Project Datasheet

## Project Name

9-NAND Full Adder with Buffered and Diagnostic I/O

---

## Overview

This project implements a 1-bit CMOS full adder using the OpenSUSI TR-1um process.

The main full-adder logic is constructed from nine 2-input NAND gates.
The circuit accepts three logic inputs, A, B, and Cin, and generates Sum and Cout.

Both the internal full-adder outputs and buffered outputs are connected to external pads.
In addition, an independent NAND gate and inverter circuits are connected to unused pads for diagnostic testing.

The main features are:

- 1-bit full adder implemented using nine NAND gates
- Direct observation of the unbuffered Sum and Cout nodes
- Two-stage output buffers for Sum and Cout
- Additional standalone NAND and inverter test circuits
- 5 V CMOS operation with the TR-1um process
- ESD-protected I/O through the supplied OSS_FRAME

The additional diagnostic outputs allow individual logic gates to be evaluated even if another part of the full-adder circuit does not operate as expected.

---

## How it works

The main circuit is a 1-bit full adder implemented using nine 2-input NAND gates.

The logical function is:

Sum = A XOR B XOR Cin

Cout = (A AND B) OR (A AND Cin) OR (B AND Cin)

The full-adder logic is constructed entirely from NAND gates.

The internal Sum and Cout signals are connected directly to external pads for observation.
Each signal is also connected to a two-stage CMOS inverter buffer before being connected to another output pad.

The output buffer structure is:

Full-adder output
→ first inverter
→ second inverter
→ output pad

The first inverter uses:

- NMOS: W = 5 um, L = 1 um
- PMOS: W = 10 um, L = 1 um

The second inverter uses:

- NMOS: W = 10 um, L = 1 um, m = 2
- PMOS: W = 20 um, L = 1 um, m = 2

The NAND gates in the full-adder use:

- NMOS: W = 3.4 um, L = 1 um
- PMOS: W = 3.4 um, L = 1 um

Additional standalone logic circuits are included for independent testing:

- one 2-input NAND gate
- one inverter using the first-stage buffer size
- one inverter using the second-stage buffer size

These circuits make it possible to test basic CMOS logic independently of the complete full-adder path.

---

## Interface

The circuit operates from a nominal 5 V supply.

There is no clock or reset signal because the design is purely combinational.

| Signal | Direction | Description |
|--------|-----------|-------------|
| P1 | input | Full-adder Cin |
| P2 | input | Full-adder B |
| P3 | input | Full-adder A |
| P4 | input | Standalone NAND input 1 |
| P5 | input | Standalone NAND input 2 |
| P6 | output | Standalone NAND output |
| P7 | input | Standalone inverter input |
| P9 | output | Standalone inverter output |
| P10 | input | Second standalone inverter input |
| P11 | output | Second standalone inverter output |
| P12 | output | Unbuffered Cout |
| P13 | output | Unbuffered Sum |
| P14 | output | Buffered Cout |
| P15 | output | Buffered Sum |
| VDD | power | 5 V supply |
| VSS | power | Ground |

All signal pads are connected through the ESD protection circuitry included in the supplied OSS_FRAME.

---

## How to test

### Full-adder test

Apply all eight combinations of A, B, and Cin to P3, P2, and P1.

Expected results are:

| A | B | Cin | Sum | Cout |
|---|---|-----|-----|------|
| 0 | 0 | 0 | 0 | 0 |
| 0 | 0 | 1 | 1 | 0 |
| 0 | 1 | 0 | 1 | 0 |
| 0 | 1 | 1 | 0 | 1 |
| 1 | 0 | 0 | 1 | 0 |
| 1 | 0 | 1 | 0 | 1 |
| 1 | 1 | 0 | 0 | 1 |
| 1 | 1 | 1 | 1 | 1 |

Observe:

- P13 for the unbuffered Sum
- P12 for the unbuffered Cout
- P15 for the buffered Sum
- P14 for the buffered Cout

The unbuffered and buffered outputs should represent the same logical values.

### Standalone NAND test

Apply logic levels to P4 and P5 and observe P6.

Expected behavior:

| P4 | P5 | P6 |
|----|----|----|
| 0 | 0 | 1 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

### Standalone inverter test

Apply a logic level to P7 and observe P9.

Apply a logic level to P10 and observe P11.

Expected behavior:

| Input | Output |
|-------|--------|
| 0 | 1 |
| 1 | 0 |

A logic LOW is approximately 0 V and a logic HIGH is approximately 5 V.

---

## Layout notes

The design is implemented in the OpenSUSI TR-1um CMOS process.

The full-adder core consists of nine NAND gates.
Two-stage inverter buffers are placed between the main Sum/Cout nodes and the final output pads.

The supplied OSS_FRAME is used for the chip I/O interface and includes ESD protection for the signal and power pads.

The internal Sum and Cout nodes are also routed directly to separate pads.
This provides additional observability and allows the full-adder core to be evaluated independently of the output buffers.

Additional standalone NAND and inverter circuits are included to provide independent test structures for basic CMOS logic operation.

The prBoundary layer (GDS layer 235/0) is used in the PDK as a cell-boundary representation.
Electrical connectivity and device geometry are defined by the actual process drawing layers such as active, poly, contacts, and metal layers.

---

## External hardware

No special external hardware is required.

For measurement, the following equipment may be used:

- regulated 5 V power supply
- digital pattern generator or logic signal source
- oscilloscope or logic analyzer

The input voltage must remain within the supported 5 V CMOS voltage domain.

---

## Known limitations

The design is intended primarily for functional verification of the TR-1um CMOS process and basic logic operation.

The direct internal-node outputs P12 and P13 are connected to external pad and ESD structures.
Therefore, these nodes have additional capacitive loading compared with an internal-only implementation.

The buffered outputs P14 and P15 are intended to provide greater output drive capability.

The maximum operating frequency has not been specified as a guaranteed design parameter.
For initial silicon evaluation, static or low-frequency logic testing is recommended before high-speed characterization.

---

## Author

- Name: Naoki Ariyoshi
- Affiliation: Chukyo University, Faculty of Engineering
- GitHub: naoki1016jp-creator

---

## License

This design follows the license of the OpenSUSI TR-1um MPW repository unless otherwise specified.