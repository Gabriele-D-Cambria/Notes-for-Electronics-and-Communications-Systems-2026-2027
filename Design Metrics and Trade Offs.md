---
title: Design Metrics and Trade Offs
---

# 1. Index

<!--toc:start-->

- [1. Index](#1-index)
- [2. Design Metrics and Trade Offs](#2-design-metrics-and-trade-offs)
  - [2.1. Fundamentals Design Metrics](#21-fundamentals-design-metrics)
  <!--toc:end-->

# 2. Design Metrics and Trade Offs

When we want to go from our `VHDL` to an actual Logical Synthesis we will
typically use an automated tool that will take our design and map it to a
specific technology. For instance, in this course we use `Vivado` to do so.

Though the process is automated, we still have to keep in mind that not all `VHDL`
is synthesizable. Some some constructs (like `real`) may not be supported by the
synthesis tool.

Some example of constraints could be related to area, speed, power
consumption, and cost.
These constraints can often conflict with each other, leading to trade-offs
that must be carefully considered during the design process.

Also the **Design Library** could affect the out design, since different
libraries may have different characteristics and capabilities.

Once we considered all these constraints and trade-offs, we will obtain:

- A **Gate-Level Netlist** that can be used to implement the design in hardware.
- A **Report** that summarizes the design metrics and trade-offs of the design.

## 2.1. Fundamentals Design Metrics

When designing any system the fundamentals metric that we must take into
account are:

- **Functionality**: The system must perform the intended function correctly
  and reliably.
- **Cost**: The system must fit within the budget constraints. This includes
  both _NRE (fixed) costs_ (i.e. design effort) (that can be divided over the
  production volume) and _RE (variable) costs_ (i.e. parts, assembly, testing, etc.).
- **Reliability**: The system must be able to operate correctly under expected conditions
  and over its expected lifetime. It must fit within _noise margins_ and/or be _noise
  immune_, avoiding effects like _capacitive coupling_ and _inducting coupling_.
- **Performance**: The system must meet the required performance specifications,
  such as speed, throughput, latency, power consumption, _fan-in_, _fan_out_, etc.
- **Time to Market**: The system must be designed and delivered within the required
  timeframe in order to meet market demands and stay competitive.
