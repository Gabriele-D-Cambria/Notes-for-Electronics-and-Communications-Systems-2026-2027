---
title: Digital Systems
---

# 1. Index

# 2. Digital Design Technology Map

We will be working with **Field Programmable Gate Arrays (FPGAs)**, which are
integrated circuits that can be configured by the user after manufacturing.
They consist of an array of programmable logic blocks and a hierarchy of
reconfigurable interconnects that allow the blocks to be wired together.

Some configurable resources in FPGAs include:

- Configurable Logic Blocks (CLBs)
- Embedded memory blocks (ROM, RAM)
- DSP
- I/O blocks
- Programmable interconnection matrix

The design flow for FPGAs typically involves the following steps:

<img class="" src="./images/digital_design/fpga-design-flow.png">

From the RTL Code, which gives a logic description of the system, where logic
gates have no delay, area occupation, or power consumption, we move to the
Synthesis step, which converts the RTL code into a **Gate-Level Netlist**,
made by FPGA (family) library cells.

One important note is that the synthesis does NOT place the components on the
FPGA, that step is done in the **Implementation**, which creates a **Native Circuit
Description** (NCD) file, which is a binary file that contains the routed circuit
on the FPGA, mapping the gates with specific FPGA blocks.
