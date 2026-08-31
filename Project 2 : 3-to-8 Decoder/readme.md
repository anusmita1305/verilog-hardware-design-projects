# Project 2: 3-to-8 Decoder

## Overview

This project implements a 3-to-8 binary decoder with an active-high enable signal using Verilog.

When the decoder is enabled, the 3-bit input selects exactly one of the eight output lines. When the decoder is disabled, all outputs remain LOW.

## Design Specifications

- Input: 3-bit binary value
- Enable: Active-high
- Output: 8-bit one-hot decoded output
- Design type: Combinational logic
- Synthesizable Verilog

## Functionality

| Enable | Input | Output |
|--------|-------|--------|
| 0 | Any | `00000000` |
| 1 | `000` | `00000001` |
| 1 | `001` | `00000010` |
| 1 | `010` | `00000100` |
| 1 | `011` | `00001000` |
| 1 | `100` | `00010000` |
| 1 | `101` | `00100000` |
| 1 | `110` | `01000000` |
| 1 | `111` | `10000000` |

## Key RTL Concepts

- Combinational logic
- Active-high enable
- One-hot output
- Variable bit-select
- Latch prevention
- Synthesizable RTL

## Hardware Interpretation

A 3-bit input provides 8 possible combinations. The decoder converts each input combination into a one-hot output, where exactly one of the eight output lines is HIGH when enabled.

The variable bit-select operation uses the input value to determine which output bit is asserted.

For example:

- `in = 3'b000` → `out[0] = 1`
- `in = 3'b001` → `out[1] = 1`
- `in = 3'b010` → `out[2] = 1`
- `in = 3'b111` → `out[7] = 1`

When `en = 0`, all outputs are LOW.

## Design Analysis & Extensions

### 1. 4-to-16 Decoder Using Two 3-to-8 Decoders

A 4-to-16 decoder can be constructed using two 3-to-8 decoders.

- Connect the lower 3 input bits to both decoders.
- Use the MSB as the enable-selection signal.
- When MSB = `0`, the first decoder is enabled and generates outputs 0–7.
- When MSB = `1`, the second decoder is enabled and generates outputs 8–15.

This demonstrates hierarchical construction of a larger decoder using smaller decoder blocks.

### 2. Output When Enable Is LOW

When `en = 0`, the decoder is disabled and all output lines are LOW:

`out = 8'b00000000`

This ensures that no output line is selected while the decoder is disabled.

### 3. Decoder vs. Demultiplexer

A decoder converts a binary input into a one-of-many output selection.

A demultiplexer has a data input and routes that data to one selected output based on the select lines.

**In simple terms:**

- Decoder → selects an output line.
- Demultiplexer → routes data to a selected output line.

### 4. BCD-to-7-Segment Decoder

A BCD-to-7-segment decoder converts a 4-bit BCD input into seven output signals corresponding to segments `a–g`.

- BCD values `0000`–`1001` represent decimal digits 0–9.
- Each valid BCD value produces the required segment pattern.
- Values `1010`–`1111` are invalid BCD inputs.

This extends the basic decoder concept to a practical display-decoding application.

### 5. Overlapping Enable Signals and Glitches

If two enable signals overlap, more than one decoder output path can become active.

This can result in multiple outputs being HIGH at the same time, violating the expected one-hot behavior.

In hardware, propagation delays can also cause temporary glitches when enable signals change.

Therefore, enable signals should be properly controlled and mutually exclusive when required.

## Applications

- Memory address decoding
- Register selection
- Control logic
- Display decoding
- Digital systems requiring one-of-many selection
