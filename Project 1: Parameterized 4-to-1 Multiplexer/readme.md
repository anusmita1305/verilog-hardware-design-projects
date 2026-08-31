# Parameterized 4-to-1 Multiplexer

## Overview

This project implements a parameterized 4-to-1 multiplexer in Verilog.  
The multiplexer selects one of four input channels based on a 2-bit select signal.

The design is fully combinational and supports configurable data width using parameterization.

## Design Features

- 4-to-1 multiplexer
- 2-bit select input
- Parameterized data width
- Fully combinational RTL design
- Complete output assignment to avoid unintended latch inference
- Synthesizable Verilog implementation

## Functional Operation

| Select (`sel`) | Output (`y`) |
|---|---|
| `00` | `in0` |
| `01` | `in1` |
| `10` | `in2` |
| `11` | `in3` |

## RTL Design Concepts

- Combinational logic using `always @(*)`
- `case`-based input selection
- Parameterization for reusable RTL
- Complete case handling to prevent latch inference

## Design Extensions & Key Insights

### 1. Building an 8-to-1 MUX

An 8-to-1 MUX can be constructed using two 4-to-1 MUXes followed by a final 2-to-1 selection stage.

- The first 4:1 MUX handles inputs 0–3.
- The second 4:1 MUX handles inputs 4–7.
- The lower two select bits control both 4:1 MUXes.
- The MSB of the select signal chooses between their outputs.

This demonstrates hierarchical RTL design and reuse of smaller modules.

### 2. FPGA Implementation

When targeting an FPGA, multiplexer logic is generally mapped into LUT-based resources by the synthesis tool.

For larger multiplexers, multiple LUTs and available FPGA routing/multiplexer resources may be required.

### 3. Critical Path of a 32-to-1 MUX

A balanced 32-to-1 MUX constructed from 2-to-1 MUXes requires:

`log2(32) = 5 levels`

Therefore, the critical path passes through approximately five 2-to-1 MUX stages.

Approximate critical-path delay:

`Tcritical ≈ 5 × Tmux`

This illustrates how a balanced MUX tree provides logarithmic logic depth.

### 4. Priority MUX Extension

A standard MUX selects an input according to a select signal.

A priority MUX instead selects the highest-priority active input when multiple conditions are active.

For example, with:

`I3 > I2 > I1 > I0`

if both `I3` and `I1` are active, `I3` receives priority.

This can be implemented using priority-based conditional logic such as an `if-else` chain.

## Verification

The design can be verified using a Verilog testbench by applying all possible select values and checking that the corresponding input appears at the output.

Additional verification can be performed using different `DATA_WIDTH` values to confirm correct parameterized operation.

