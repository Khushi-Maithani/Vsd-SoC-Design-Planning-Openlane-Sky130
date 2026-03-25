# Day 5 – Final Steps for RTL to GDS Using TritonRoute and OpenSTA

## Overview

Day 5 focused on the final stages of the ASIC physical implementation flow — routing, verification, power network construction, and timing closure.
The goals of the day were to:

1. Perform global and detailed routing
2. Understand and apply design rule checking (DRC)
3. Build a Power Distribution Network (PDN)
4. Use TritonRoute features to carry out routing
5. Verify post‑route timing using OpenSTA
6. Export final layout data (GDS2)

---

## 1. Routing and Design Rule Check (DRC)

### 1.1 Physical Connectivity

Physical connectivity starts once placement is complete.
The router must connect all nets (signals) from the placed standard cells according to the netlist while obeying spacing, width, and design rules imposed by the process technology.

---

### 1.2 Global Routing vs. Detailed Routing

* **Global Routing** determines *which general paths* nets will follow across the design. It assigns approximate routing *regions* and layers without exact track positions.
* **Detailed Routing** assigns *exact wire tracks* to nets. At this stage, the router enforces exact spacing, metal layer usage, via placement, and all process design rules.

Global routing is a planning stage; detailed routing is the final wire‑level implementation.

---

### 1.3 Introduction to Maze Routing and Lee’s Algorithm

**Maze Routing** is a foundational routing method that finds a path from a source to a destination in a grid while avoiding obstacles.

**Lee’s Algorithm** is a breadth‑first search technique used in maze routing for shortest path discovery:

* Start from the source cell.
* Expand to all reachable adjacent grid cells, marking each with a cost (or distance).
* Continue expanding outward until the destination is reached.
* Backtrace from destination to source to extract a valid path.

Key features of Lee’s Algorithm:

* **Guaranteed shortest path** in grid routing
* **Flood‑fill approach** that assigns increasing costs
* Can handle *obstacles and blockages* easily
* Works well for early layers and track usage definitions

Lee’s Algorithm is often used conceptually in global routing and grid‑based routers as the base for cost propagation and path extraction.

---

### 1.4 Design Rule Check (DRC)

**Design Rule Checking** verifies that the layout obeys manufacturing constraints specific to the process technology — such as:

* Minimum wire widths per metal layer
* Minimum spacing between wires
* Via enclosure and cut spacing
* Density and antenna rules

DRC occurs after routing but often iteratively during detailed routing to maintain correctness before GDS export.

---

## 2. Power Distribution Network (PDN) and Routing

A reliable PDN ensures uniform delivery of VDD and GND to all cells with minimal voltage drop (IR drop) and noise.

### 2.1 Power Straps and PDN Construction

Power straps are wide metal segments that distribute supply voltages across the chip.

**Steps to build PDN:**

1. Define power grid topology (mesh, stripes, or combination)
2. Assign wide metal layers for power rails
3. Connect cell pins to the nearest stripes or mesh points
4. Ensure continuity and redundancy
5. Verify with IR/EM tools (Post‑route electrical checks)

### 2.2 Global & Detailed Routing with PDN

* PDN routing often reserves *preferred layers* to minimize interference with signal routing.
* The PDN must be placed first or concurrently to avoid conflicts.
* Routing tools like TritonRoute can combine PDN and signal routing layers for efficiency.

### 2.3 Configuring TritonRoute

TritonRoute is a detailed router that:

* Accepts placement data (DEF + LEF)
* Routes all nets with DRC checks
* Handles multi‑layer via insertion
* Uses routing guides and cost metrics

---

## 3. TritonRoute Features and Routing Topology

TritonRoute provides advanced routing capabilities for modern ASIC design.

### 3.1 Corner Pre‑Processed Route Guide

Before full routing, TritonRoute pre‑processes the netlist to create *route guides*, which are directions for nets to help focus routing resources and optimize paths.

### 3.2 Inter‑Guide Connectivity and Routing

Inter‑guide connectivity ensures:

* Nets can connect between different route regions
* Adjacent routing guides share track availability
* Complex nets spanning blocks can be routed efficiently

### 3.3 Intra‑ and Inter‑Layer Routing

* **Intra‑layer routing:** Routing within a single metal layer
* **Inter‑layer routing:** Routing that uses vias to switch layers for optimized paths

TritonRoute must manage via placement, multiple metal layers, and DRC on spacing and widths.

### 3.4 Routing Topology Algorithm

Routing topology refers to how the router decides:

* Which layers nets will take
* Where vias should be inserted
* How nets cross each other
* How congestion and cost are managed

Topology optimization seeks to minimize:

* Total wire length
* Delay
* Crosstalk
* Congestion hotspots

### 3.5 Final Files Post‑Route

When routing completes successfully:

* Updated DEF (with routing)
* Routed GDS2
* Routing reports
* Congestion maps
* DRC

These final files represent the fully routed design ready for verification.

---

## 4. Common Issues and Configuration Variables

### 4.1 `LIB_SYNTH_COMPLETE`

This variable must be defined in OpenLane configurations to indicate that all standard cell libraries are ready for synthesis, timing, and routing steps.

### 4.2 PDN Configuration

`gen_pdn` or PDN enable flags in config files tell the flow to generate and include a PDN before routing.
           It sets up the power grid, defines power rails, and ensures proper connectivity.

Common Issues

LIB_SYNTH_COMPLETE:
This variable must be defined in the config.tcl file.
It is called by the gen_pdn procedure defined in the or_pdn.tcl file.

LEF_MERGED_UNPADDED:
Ensure that this variable is correctly set in the config.tcl file.
It provides essential information for PDN generation.
---

## 5. Routing Control and Utility Commands

During routing, these checks and commands manage routing strategies and validations.

Command to perform routing

<img width="591" height="342" alt="Screenshot (81)" src="https://github.com/user-attachments/assets/ea943c3b-b461-4fa0-b1aa-9632ec7bf634" />

```bash
# Check value of current diff (difference between estimates)
check_value_of current_diff

# Echo a current variable for validation
echo $proportion_A_and_B_current_diff

# Check routing strategy configuration
check_value_of routing_strategy = $proportion_A_and_B

# Conditional routing, if low
if <condition>
    run dot
fi
```

Then the detailed routing command:

```bash
run route
```

and once routing finishes:

```bash
report route
```

---

## 6. Initiating Routing Stage

After placement and CTS:

1. Load placed and buffered DEF
2. Load cell LEFs
3. Load PDN config
4. Run global routing
5. Run detailed routing with DRC enforcement
6. Generate routed DEF and reports

This stage is typically invoked in the OpenLane interactive or scripted flow.

---

## 7. Middle Layer Utilization and Wire Optimization

* **Middle layers** (e.g., Metal 3–Metal 6) are often designated for horizontal and vertical routing channels.
* The router must balance utilization to avoid congestion — high use leads to conflicts, low use wastes routing capacity.
* Wire optimization includes minimizing detours, better via placements, and reducing layer changes.

---

## 8. Post‑Routing Static Timing Analysis (STA)

After routing and DRC:

* OpenSTA is run again with *real parasitics* extracted from the routed DEF.
* Setup and hold slack are calculated for critical paths.
* Real clocks (including CTS delays) are used to verify timing closure.

---

## 9. Final Layout and GDS2 Export

Once routing, PDN, and STA are complete:

1. Verify DRC is clean
2. Verify timing slack is met
3. Generate **GDS2** (final layout format)
4. Prepare hand‑off data (DEF + GDS2 + reports)

GDS2 is the format used for tape‑out and manufacturing.

---

## Commands Used in Day 5

### Enter the OpenLane Flow

```bash
cd OpenLane
./flow.tcl -interactive
```

### Prepare Design for Routing

```bash
prep_design PicoRV32A
```

### Run Clock Tree Synthesis (if not done already)

```bash
run CTS
report CTS
```

### Run Routing (TritonRoute)

```bash
run route
```

### View Routing Reports

```bash
report route
```

### Inspect Log Files

Check the `runs/PicoRV32A/logs` directory for routing and DRC logs.

### Open Routed Layout in Magic

```bash
magic -T sky130A.tech -d red \
    -rcfile sky130A.magicrc \
    -r "load routed.def"
```

---

## Timing Concepts

**Setup Time (t<sub>su</sub>)**: Minimum time before clock edge input must be stable.
**Hold Time (t<sub>h</sub>)**: Minimum time after clock edge input must remain stable.
**Slack**: Difference between required time and arrival time — positive means timing met.

Post‑route STA includes wire parasitics, via delays, and real clock delays for accurate timing.

---

## Conclusion

Day 5 completed the end‑to‑end physical design flow from:

* Placement → CTS → Routing → PDN → DRC → STA → GDS2 export.

The process involved:

* Using **TritonRoute** for routing
* Enforcing **Design Rule Checks**
* Building a **robust Power Distribution Network**
* Performing **final STA with real clocks**
* Generating the final **GDS2 layout database**

