# Day 3 – Design Library Cell Using Magic Layout and NGSPICE Characterization

## Overview

Day 3 covered the following major topics:

1. **Labs for CMOS Inverter & NGSPICE Simulation**
2. **Inception of Layout & CMOS Fabrication Process**
3. **Sky130 Technology File Labs**
4. **Introduction to Placement in ASIC Physical Design**
5. **Commands Used in Day 3**
6. **Timing Calculations**

---

## 1. Labs for CMOS Inverter & NGSPICE Simulation

This section focuses on designing a CMOS inverter, creating a SPICE netlist, deriving switching characteristics, and performing static/dynamic simulations using NGSPICE.

### 1.1 IO Placer Revision

* Review of IO placement concepts and constraints
* How to fix input/output pads in the layout
* Importance of IO placement in timing and routing

### 1.2 SPICE Tech Creation for CMOS Inverter

* Create a SPICE deck for a CMOS inverter
* Define PMOS and NMOS models
* Set supply voltage (VDD) and ground (GND)

### 1.3 Switching Threshold (V<sub>TH</sub>)

* Switching threshold is the input voltage at which output changes logic state
* Measured using transient analysis

### 1.4 Static and Dynamic Simulation

* **Static simulation:** DC sweep for voltage transfer characteristics
* **Dynamic simulation:** Transient analysis to visualize propagation delay and transition times

### 1.5 Lab Steps to Get Flowed STD Cell Design

* Setup design environment
* Create netlist for the inverter
* Run simulation and extract key metrics
* Validate results against expected specifications

---

## 2. Inception of Layout & CMOS Fabrication Process

This section outlines the foundational steps in physical layout creation and an introduction to CMOS fabrication.
              
              Active region → n/p wells → gate → LDD → source/drain → local interconnect → higher metal layers.

### 2.1 Active Regions

* Create diffusion regions where transistors will be formed
* Defines where transistors can exist

### 2.2 Formation of n‑Well and p‑Well

* n‑Well created in p‑type substrate for PMOS
* p‑Well created in n‑type substrate for NMOS
* Well formation is the first step in establishing transistor regions

### 2.3 Gate Terminal Formation

* Pattern polysilicon gate regions
* Gate length determines device speed and threshold

### 2.4 Lightly Doped Drain (LDD)

* LDD regions reduce hot carrier effects
* Placed between heavily doped source/drain and channel

### 2.5 Source‑Drain Formation

* Heavily doped regions form source and drain
* Contacts attached for electrical connectivity

### 2.6 Local Interconnect Formation

* First metal layer connections between transistors
* Used for short local routing

### 2.7 Higher Level Metal Formation

* Additional metal layers for long interconnects
* Power and signal routing across the chip

### 2.8 Sky130 Basic Layered Layout & LEF

* Introduction to Sky130 layered technology
* LEF files define standard cell physical rules
* Example: Inverter layered layout

### 2.9 Lab Steps to Create SDC Layout & Extract SPICE Netlist

* Create layout in Magic VLSI
* Place diffusion, gate, and contacts
* Extract SPICE netlist using Magic extraction tools

---

## 3. Sky130 Technology File Labs

This section focuses on hands‑on work using the Sky130 PDK (process design kit).

### 3.1 Create Final SPICE Tech Using Sky130

* Generate a SPICE technology file based on Sky130
* Includes device models, parasitics, and interconnect effects

### 3.2 Characterize Inverter Using Sky130 Model

* Run simulations using Sky130 MOS models
* Extract propagation delay, rise/fall times

### 3.3 Magic Tool and DRC Rules

* Use Magic VLSI tool with Sky130 rule files
* Follow design rule checks (DRC) for layout compliance

### 3.4 Sky130 PDK Introduction

* Sky130 PDK is a set of technology files
* Includes LEF, GDS, SPICE models, and DRC/LVS rules
* Steps to obtain and install the PDK

---

## 4. Introduction to Placement in ASIC Physical Design

Placement is a key part of physical design that determines where standard cells are positioned before routing.

### 4.1 Global Placement

* High‑level placement of cells across the chip
* Goal: minimize wire length, balance utilization
* Uses analytical or stochastic algorithms

### 4.2 Detailed Placement

* Fine‑tunes the position of cells
* Removes overlaps
* Improves local timing and congestion

### 4.3 Density Optimization

* Ensures even cell distribution
* Adjusts cell density to reduce empty regions or overcrowding

### 4.4 Role of Library Cells

* Standard cells represent logic gates (inverter, NAND, flip‑flops)
* Cells have fixed height and variable width
* Placement uses library characteristics (timing/power)

### 4.5 Initiating Global Placement

* Read netlist and floorplan
* Assign cell positions based on connectivity
* Use placement engine to optimize

### 4.6 Evaluating Placement Quality

Placement quality is assessed using:

* Even cell distribution
* Minimal overlaps
* Logical grouping of related blocks
* Wire length estimates
* Timing estimates

---

## 5. Commands Used in Day 3

Commands for preparing design, running placement, and inspecting results in OpenLane and Magic.

### 5.1 Cloning the Repository

```bash
git clone <repository_link>
cd <repository_folder>
```

### 5.2 Navigate to OpenLane Directory

```bash
cd OpenLane
```

### 5.3 Prepare the Design

```bash
prep_design PicoRV32A
```

Output files:

```
runs/PicoRV32A/logs
runs/PicoRV32A/reports
runs/PicoRV32A/results
runs/PicoRV32A/temp
```

### 5.4 Launch OpenLane Interactively

```bash
./flow.tcl -interactive
```

### 5.5 Execute Placement Stage

```bash
run placement
```

Output:

```
runs/PicoRV32A/results/placement/placement.def
```

### 5.6 Open Layout Using Magic

```bash
magic -T sky130A.tech -d red -rcfile sky130A.magicrc - \
  -r "load placement.def"
```

### 5.7 View Placement DEF File Using Vim

```bash
vim runs/PicoRV32A/results/placement/placement.def
```

### 5.8 Check Placement Reports

```bash
report placement
```

---

## 6. Timing Calculations

Formulas used to calculate transition times and cell delays.

### 6.1 Transition Time

* **Rise Transition Time (t<sub>trise</sub>):**

```
t_rise = time(output rises to 80%) - time(output rises to 20%)
```

* **Fall Transition Time (t<sub>tfall</sub>):**

```
t_fall = time(output falls to 20%) - time(output falls to 80%)
```

### 6.2 Propagation Delay

* **Rise Delay (t<sub>p(↑)</sub>):**

```
t_p(↑) = time(output rises to 50%) - time(input rises to 50%)
```

* **Fall Delay (t<sub>p(↓)</sub>):**

```
t_p(↓) = time(output falls to 50%) - time(input falls to 50%)
```
LEF File Creation
Now that we have successfully characterized the inverter, the next step is to create a LEF (Library Exchange Format) file.

wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz

VLSI Layout Geometries and DRC Errors

In this section, we explore independent example layout geometries (M3.1, M3.2, M3.5, and M3.6) and highlight the specific DRC (Design Rule Check) errors associated with each:

M3.1 (Metal Width DRC):

Violation: The metal trace width in M3.1 is below the specified minimum width threshold.
Error: Metal width does not meet design rules.
M3.2 (Metal Spacing DRC):

Violation: The distance between adjacent metal traces in M3.2 does not meet the required spacing.
Error: Metal spacing violation.
M3.5 (Via Overlapping DRC):

Violation: The vias in M3.5 overlap with each other.
Error: Via overlapping issue.
M3.6 (Minimum Area DRC):

Violation: The enclosed area within M3.6 does not meet the specified minimum area requirement.
Error: Minimum area violation.
Use the command magic -d XR to open the Magic tool

<img width="758" height="300" alt="Screenshot (76)" src="https://github.com/user-attachments/assets/47318467-adea-4b1c-8efd-4f3adc063027" />

## Conclusion

Day 3 deepened practical understanding of:

* CMOS inverter design, simulation and extraction
* Physical layout processes and device formation steps
* Use of Sky130 PDK for standard cell layout and characterization
* Placement strategies in ASIC design
* Commands for preparing, placing, and analyzing designs
