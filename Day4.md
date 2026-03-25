# Day 4 – Pre‑Layout Timing Analysis and Importance of Good Clock Tree

## Overview

Day 4 focused on pre‑layout timing modeling, timing analysis with ideal and real clocks, clock tree synthesis (CTS), and signal integrity. The objective was to understand timing behavior before layout, perform timing checks using OpenSTA, and learn clock distribution and synthesis.

The four parts of the day were:

1. Timing Modeling Using Delay Tables
2. Timing Analysis with Ideal Clocks Using OpenSTA
3. Clock Tree Synthesis (TritonCTS) and Signal Integrity
4. Timing Analysis with Real Clocks Using OpenSTA

---

## 1. Timing Modeling Using Delay Tables

### 1.1 Concepts

* **Delay Tables:**
  Lookup tables that store timing delay values (rise, fall, transition) for a cell under different input slope and output load combinations. Delay tables are used by synthesis and STA tools to estimate timing of cells.

* **Track Info and Grid Info:**
  ASIC floorplanning uses *grid info* to define placements and routing grids. *Track info* translates that into actual metal tracks for routing resources.

* **Magic → LEF Conversion:**
  Converting physical layout (Magic) into a library exchange format (LEF) that captures cell size, pin locations, and metal layer usage for place & route.

* **Synthesis Inclusion:**
  Steps required to include a new cell in synthesis flow (LEF + delay tables + power/pin info) to allow timing estimators to use the new cell properly.

### 1.2 Labs

* Convert grid information to track information
* Convert Magic cell layout to a standard cell LEF
* Intro to timing lab, include new cell for synthesis
* Configure delay tables for the new cell
* Use delay tables in synthesis
* Configure synthesis to fix slack using timing models

---

## 2. Timing Analysis with Ideal Clocks Using OpenSTA

### 2.1 Concepts

* **OpenSTA:**
  An open‑source Static Timing Analysis tool that reads netlists, libraries, and constraints to compute timing metrics (slack, arrival times, path delays).

* **Ideal Clock:**
  A clock with no skew, jitter, or uncertainty — used as a reference model before real clock distribution is synthesized.

* **Setup Time:**
  The minimum time before the clock edge that the data must be stable to ensure correct latch/flip‑flop capture.

* **Jitter & Uncertainty:**
  Jitter is variation in clock edge timing; uncertainty is STA’s margin for variation due to phase and distribution differences.

* **Post‑Synthesis Timing:**
  Timing analysis performed after synthesis using ideal clocks to find violations before placement.

### 2.2 Labs

* Configure OpenSTA for post‑synthesis timing
* Introduce setup time, clock, and uncertainty constraints
* Optimize synthesis to reduce setup violations
* Run basic timing analysis using OpenSTA

---

## 3. Clock Tree Synthesis (TritonCTS) and Signal Integrity

### 3.1 Concepts

* **Clock Distribution:**
  The process of delivering a clock signal from the clock source to all sequential elements with minimal skew and predictable delay.

* **Need for Clock Tree Synthesis (CTS):**
  After placement, physical distances and loading differ; CTS balances the clock network to reduce skew and ensure timing closure.

* **Global vs Detailed Routing:**
  *Global routing* plans high‑level paths for signals; *detailed routing* assigns actual wires and layers.

* **Clock Tree Synthesis Techniques:**
  CTS algorithms like H‑tree, balanced trees, grid clocking. Buffers and inverters are inserted to maintain signal integrity.

* **Signal Integrity:**
  Crosstalk, noise, and reflections that affect timing. Clock nets are often shielded or buffered to reduce coupling.

### 3.2 Labs

* Run CTS using TritonCTS
* Verify CTS results and timing improvements
* Understand the role of library cells during clock buffering and insertion

---

## 4. Timing Analysis with Real Clocks Using OpenSTA

### 4.1 Concepts

* **Real Clocks:**
  Clock with real delay, skew, uncertainty, and jitter after CTS is applied.

* **Setup & Hold Analysis:**
  Post‑CTS timing analysis examining setup and hold slack to ensure data is captured correctly at clock edges.

* **Impact of CTS Buffers:**
  Buffers increase delay but aim to reduce skew; effects must be analyzed on setup and hold paths.

### 4.2 Labs

* Perform post‑CTS timing analysis using real clocks in OpenSTA
* Use real libraries and CTS assignments
* Observe effect of CTS buffers on timing slack

--->   Commands for tkcon window to set grid as tracks of locali layer

  # Get syntax for grid command help grid

  # Set grid values accordingly grid 0.46um 0.34um 0.23um 0.17um

  <img width="415" height="543" alt="Screenshot (78)" src="https://github.com/user-attachments/assets/77bedff2-3408-4fd9-8eba-c8f522b811ce" />

---

## Keyword Glossary 

| Term             | Description                                                                                                                  |
| ---------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| Delay Tables     | Lookup tables storing timing data (rise/fall) for library cells used in STA.                                                 |
| LEF              | Library Exchange Format that describes physical cell characteristics used for placement.                                     |
| OpenSTA          | Static Timing Analyzer that computes arrival times, required times, and slack using SDC constraints, netlist, and libraries. |
| Ideal Clock      | Clock with no skew or uncertainty used in initial timing checks.                                                             |
| Setup Time       | Minimum time data must be stable before clock edge for correct latching.                                                     |
| Jitter           | Variation in clock arrival time due to noise or variations in the clock source.                                              |
| Uncertainty      | STA slack margin for clock phase differences and variations.                                                                 |
| CTS              | Clock Tree Synthesis; inserting buffers/inverters to balance clock paths.                                                    |
| Skew             | Difference in clock arrival times at different flip‑flops; minimized during CTS.                                             |
| H‑Tree           | Balanced clock distribution network topology often used in CTS.                                                              |
| Crosstalk        | Unwanted noise induced by adjacent signal lines affecting timing.                                                            |
| Shielding        | Adding ground/ power wires around critical nets (e.g., clock) to reduce noise.                                               |
| Global Routing   | Assignment of coarse paths for signals before detailed routing.                                                              |
| Detailed Routing | Actual routing on specific metal layers after global routing.                                                                |
| Slack            | Difference between required arrival time and actual arrival time; positive slack = timing met.                               |
| Real Clocks      | Clock with real physical delays and skew after CTS and physical effects.                                                     |

---

## Timing Flowchart Description

Below is the **clock tree synthesis timing flow** to include inside your MD file:

```
Placement + Floorplan
        |
        v
Clock Tree Synthesis Engine
        |
        v
Clock Buffer & Inverter Insertion
        |
        v
Clock Skew Optimization
        |
        v
Updated CTS Report + Depth + Skew Metrics
```

**Clock Tree Synthesis (CTS)** transforms a single clock net into a balanced distribution of networks using library cells to **reduce skew and maintain timing stability**.

---

## Commands Used in Day 4

```bash
# Enter OpenLane environment
cd OpenLane
./flow.tcl -interactive

# Prepare design for CTS
prep_design PicoRV32A

# Run clock tree synthesis
run CTS

# View CTS reports
report CTS

# Inspect CTS logs
# (uses results directory and log files for CTS)
```

After running CTS, you would open updated DEF in Magic and verify placement updates from CTS.

---

## Timing Formulas (Quick Reference)

**Rise Transition Time:**

```
t_rise = T(output rises to 80%) - T(output rises to 20%)
```

**Fall Transition Time:**

```
t_fall = T(output falls to 20%) - T(output falls to 80%)
```

**Propagation Delay (Rise):**

```
t_p(↑) = T(output rises to 50%) - T(input rises to 50%)
```

**Propagation Delay (Fall):**

```
t_p(↓) = T(output falls to 50%) - T(input falls to 50%)
```

---

## Conclusion

Day 4 provided a deeper understanding of:

* How timing models (delay tables) are used before layout
* Performing timing analysis with both ideal and real clocks using OpenSTA
* Importance of clock tree synthesis and its impact on skew and timing
* Practical workflow for CTS using TritonCTS
* Commands for synthesis, timing checks, and report review

This documentation captures both **theory** and **lab practices** required for pre‑layout timing analysis and clock distribution.
