# Day 2 – Good floorplan vs Bad floorplan and introduction to library cells

## Overview

Day 2 focused on:

1. Good Floor Plan vs Bad Floor Plan
2. Introduction to Library Cells

The session was divided into four parts:

1. Chip Floor Planning Considerations
2. Library Binding and Placement
3. Cell Design & Characterization Flow
4. General Timing Characterization Parameters

---

## 1. Chip Floor Planning Considerations

### Floor planning
<img width="747" height="550" alt="Screenshot (73)" src="https://github.com/user-attachments/assets/9d876163-1e94-4ceb-bdf1-8ffe65917897" />


Floor planning defines how blocks in an SoC are placed within the chip area. A good floor plan reduces wire length, improves timing, and simplifies routing. A bad floor plan causes congestion, noise, and timing violations.


### Utilization Factor

* Definition: Percentage of the core area occupied by the logic netlist.

```
Utilization Factor = (Area of Netlist / Total Core Area) × 100%
```

* Ideal utilization is typically between 70%–85%. Too low results in wasted area, too high leads to congestion.

### Aspect Ratio

* Definition: Ratio of core height to core width.

```
Aspect Ratio = Height / Width
```

* A balanced aspect ratio improves layout efficiency, routing, and timing predictability.

### Core & Die Size

* Core: Area containing active logic.
* Die: Entire chip including cores, IO, and bonding pads.

### Pre-Placed Cells

* Components fixed in the layout before placement, e.g., decoupling capacitors and IO pads.
* Pre-placed cells require spacing to prevent noise coupling.

### Power Planning

* Ensures uniform power distribution, minimum IR drop, and correct placement of decoupling capacitors.

### Placement Blockages

* Regions where cells cannot be placed due to pre-placed macros, decoupling capacitors, or power grids.

### Magic Flowplan Review

Steps to review layout in Magic VLSI:

1. Load layout and DEF/GDS files.
2. Validate placement rectangles.
3. Check blockages.
4. Inspect routing channels.
5. Annotate timing congestion.

### Logic Levels

For digital logic:

| Logic         | Voltage           |
| ------------- | ----------------- |
| Logic 0 (VIL) | ≤ Lower threshold |
| Logic 1 (VIH) | ≥ Upper threshold |

---

## 2. Library Binding and Placement

### Library Binding

Library binding associates standard cells in the netlist with physical library cells, ensuring correct placement for performance.

### Initial Placement

* Aligns cells approximately based on connectivity, area, and optimization goals.

### Placement Optimization

* Reduces wire length, balances cell density, and minimizes capacitance and delays.
* Includes global placement (coarse) and detailed placement (fine).

### Netlist Connectivity

* Analyze netlist structure, calculate fan-in and fan-out, group related cells, and reduce wire length.

### Placement & Routing Flow

1. Netlist analysis
2. Floor planning
3. Initial placement
4. Placement optimization
5. Routing
6. Timing closure

### Final Placement Optimization

* Check for timing violations
* Adjust for congestion
* Verify no overlap with blockages

---

## 3.Cell Design & Characterization Flow              

<img width="309" height="461" alt="Screenshot (69)" src="https://github.com/user-attachments/assets/cec7167d-2f00-4f01-a052-9cf9adaa4909" />

### Inputs

* Functional specifications
* Timing requirements
* Power constraints
* Physical library rules

### Design Steps

1. Circuit design: logic schematic and transistor sizing
2. Layout design: physical representation, DRC/LVS clean

### Characterization Flow

* Extract timing parameters (max/min delays)
* Parasitic extraction
* Power modeling
* Load/transition tables

### Timing Terms

* Propagation delay: time for input change to reflect at output
* Transition time: rate at which output changes between logic levels

---

## 4. General Timing Characterization Parameters

* Setup time
* Hold time
* Rise time / fall time
* Capacitance
* Output drive strength

Accurate timing parameters help predict clock frequency limits, signal integrity, and timing closure.

---

## Tools and Labs

* Magic VLSI
* GSPICE simulation: CMOS inverter SPICE deck, delay measurement, waveform verification

---

## Conclusion

Day 2 provided an understanding of:

* Floor planning fundamentals
* Placement strategies
* Library binding process
* Cell design basics
* Timing characterization essentials

These are foundational concepts for complex back-end design flows in SoC planning

PDN Generation:

<img width="982" height="264" alt="Screenshot (70)" src="https://github.com/user-attachments/assets/51e77087-6bfb-4b87-af14-a556a51576d4" />

<img width="362" height="532" alt="Screenshot (71)" src="https://github.com/user-attachments/assets/e912a95a-efd7-42e7-a84d-f33c5fcec1c3" />

<img width="789" height="477" alt="Screenshot (72)" src="https://github.com/user-attachments/assets/d6333d43-a5c3-4b25-9521-8d8e9e6593be" />


