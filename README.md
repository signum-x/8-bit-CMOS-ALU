# 8-Bit CMOS Arithmetic Logic Unit — VLSI Design

A transistor-level CMOS implementation of an 8-bit ALU designed and simulated in **Cadence Virtuoso**, comparing a traditional multiplexer-based architecture against a novel **DEMUX-based power-gated architecture** that achieves a 33.6% improvement in Power Delay Product (PDP).

---

## Overview

Conventional ALUs compute all arithmetic and logical operations simultaneously and use multiplexers to select the desired output. This causes unnecessary switching activity across inactive blocks, increasing dynamic power consumption.

This project proposes an alternative architecture where a 1×8 DEMUX selectively routes VDD only to the block corresponding to the active operation — eliminating multiplexer overhead and suppressing switching in idle blocks.

---

## Supported Operations

The ALU supports 8 operations via a 3-bit control signal:

| `OP[2:0]` | Operation   | Type       |
|-----------|-------------|------------|
| `000`     | Addition    | Arithmetic |
| `001`     | Subtraction | Arithmetic |
| `010`     | Increment   | Arithmetic |
| `011`     | Decrement   | Arithmetic |
| `100`     | NOT         | Logical    |
| `101`     | NOR         | Logical    |
| `110`     | NAND        | Logical    |
| `111`     | XOR         | Logical    |

---

## Architecture

### Traditional MUX-Based ALU

All blocks are powered simultaneously. Output selection is done by multiplexers after all operations are computed. Every block undergoes switching transitions regardless of whether its result is needed.

### Proposed DEMUX-Based Power-Gated ALU

- A **1×8 DEMUX** routes VDD to only the selected operation block
- A **voltage separator circuit** prevents reverse voltage propagation into inactive blocks and enables output merging
- **Differentiation buffers** isolate each block's output bus
- Inactive blocks receive no supply voltage, eliminating their dynamic switching activity entirely

---

## Design Blocks

### Arithmetic

- **Full Adder** — 8-bit ripple carry adder (`Sum = A ⊕ B ⊕ Cin`, `Carry = AB + BCin + ACin`)
- **Full Subtractor** — binary subtraction with borrow propagation
- **Incrementer** — adds 1 to the 8-bit input
- **Decrementer** — subtracts 1 from the 8-bit input

### Logical

- **NOT** — CMOS inverter (complement)
- **NOR** — CMOS NOR gate
- **NAND** — CMOS NAND gate
- **XOR** — transmission-gate based XOR

---

## Simulation

| Parameter        | Value             |
|------------------|-------------------|
| Tool             | Cadence Virtuoso  |
| Technology       | CMOS              |
| Supply Voltage   | 1.8 V             |
| Simulation Type  | Transient         |
| Bit Width        | 8-bit             |

Power was measured via current integration over an 80 ns window:

$$P_{avg} = \frac{\int_0^{80\text{ns}} I(t) \times 1.8 \, dt}{80\text{ns}}$$

For the DEMUX-based design, power was decomposed into three subsystems and summed:

$$P_{final} = P_{demux} + P_{buffer} + P_{TRUTH}$$

---

## Results

| Parameter          | MUX-Based ALU | DEMUX-Based ALU | Improvement |
|--------------------|---------------|-----------------|-------------|
| Average Power      | 0.1707 mW     | 0.1294 mW       | ↓ 24.2%     |
| Propagation Delay  | 2.614 ns      | 2.2884 ns       | ↓ 12.5%     |
| PDP                | 0.4462 pWs    | 0.2961 pWs      | ↓ **33.6%** |
| Switching Activity | High          | Reduced         | —           |
| Multiplexer Usage  | Required      | Eliminated      | —           |
| Power Gating       | No            | Yes             | —           |

---

## Limitations

- Sensitivity to floating nodes in inactive blocks
- Possible short-duration distortions due to noise during switching
- Output instability during capacitor discharge at block power-on

---

## Applications

- Low-power embedded processors
- Battery-powered electronics
- Energy-efficient VLSI systems
- IoT edge devices

---

## Future Work

- Full layout implementation and DRC/LVS verification
- Post-layout simulation with parasitic extraction
- Glitch suppression and optimization
- Extension to wider bit widths (16-bit, 32-bit)
- Integration into a complete processor datapath

---

## Project Details

| Field             | Details                              |
|-------------------|--------------------------------------|
| Author            | Ratna Paul. I                        |
| Roll Number       | 22ECE1013                            |
| Department        | Electronics and Communication Engineering |
| Institute         | National Institute of Technology Goa |
| Course            | VLSI Design Laboratory               |
| Academic Year     | 2025–2026                            |

---

## References

- Weste & Harris — *CMOS VLSI Design*, Pearson Education
- M. Morris Mano — *Digital Design*, Pearson Education
- Jan M. Rabaey — *Digital Integrated Circuits*, Prentice Hall
- Kang & Leblebici — *CMOS Digital Integrated Circuits*, McGraw-Hill
- Sedra & Smith — *Microelectronic Circuits*, Oxford University Press