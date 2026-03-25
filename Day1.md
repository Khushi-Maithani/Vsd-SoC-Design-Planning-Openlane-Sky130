# Day 1 – Sky130 Workshop: Inception of Open Source EDA, OpenLane, and Sky130 PDK

*Overview:*  
Day 1 introduces the fundamentals of SoC design using open-source EDA tools, the Sky130 PDK, and OpenLane. The session is divided into three main parts:  

1. How to talk to computers  
2. SoC Design and OpenLane  
3. Get familiar with Open Source EDA tools  

---

## Part 1: How to Talk to Computers

*Overview:*  
This section covers the basics of computer hardware, RISC-V architecture, and how software translates into hardware instructions.  

### Chip Packaging and Fundamentals
- *QFN48 Package:* Standard IC package with 48 pins, used for compact and reliable chip mounting.  
- *Chip Components:* Pads, cores, die, and IP blocks.  
- *Standard Library Cells:* Pre-designed cells used for digital logic construction.  

### Introduction to RISC-V
- *ISA (Instruction Set Architecture):* Defines instructions a CPU can execute.  
- *PicoRV32A:* A small, open-source RISC-V processor core.  
- *RTL (Register Transfer Level):* Describes the hardware logic at a high level.  

### Software to Hardware
- *Compiler & Assembler:* Convert high-level programs into machine code.  
- *Operating System (OS):* Manages hardware resources, I/O, memory, and executes applications.  

---

## Part 2: SoC Design and OpenLane

*Overview:*  
Covers digital ASIC design concepts and the RTL to GDS2 flow.

### ASIC Design Concepts
- *ASIC (Application-Specific Integrated Circuit):* A chip designed for a specific application.  
- *EDA Tools:* Software tools used to design and verify integrated circuits.  
- *PDK (Process Design Kit):* Collection of files that models a fabrication process for EDA tools. Includes definitions of standard cells, layers, and design rules.

### RTL to GDS2 Flow
The simplified flow from hardware design to final layout:  

*Flowchart (Textual Representation):*

RTL Design → Synthesis → Floor Planning (FP) → Power Planning (PP) → Placement → Clock Tree Synthesis (CTS) → Routing → Sign-off

### Step Descriptions
- *Synthesis:* Converts RTL to a gate-level circuit.  
- *Floor Planning:* Determines macro placement and chip layout.  
- *Power Planning:* Defines power rails and distribution networks.  
- *Placement:* Positions standard cells within the layout.  
- *Clock Tree Synthesis (CTS):* Distributes the clock signal evenly to all flip-flops.  
- *Routing:* Connects all cells using metal layers according to the netlist.  
- *Sign-off Verification:*  
  - *DRC (Design Rule Check):* Verifies layout rules are met.  
  - *LVS (Layout vs. Schematic):* Ensures layout matches schematic design.  
  - *STA (Static Timing Analysis):* Confirms timing constraints are satisfied.

---

## Part 3: Get Familiar with Open Source EDA Tools

*Overview:*  
This section introduces the OpenLane directory, design preparation, synthesis, and result characterization.

### OpenLane Directory Structure
- Contains project folders, scripts, and configuration files.  

### Design Preparation
- Steps include creating a new project, selecting PDK, and preparing RTL files.  

### Running Synthesis
```bash
# Navigate to OpenLane environment
cd openlane

# Clone the repository and enter project directory
git clone <repo-link>
cd <project-folder>

# Launch OpenLane interactive mode
./flow.tcl -interactive

# Run synthesis
run_synthesis

Viewing Reports

Synthesis Reports: less run/PicoRV32A/report/synthesis/*.rpt

Cell Statistics & Flop Ratio:

Flop Ratio = Number of D flip-flops / Total number of cells
% of DFFs = Flop Ratio × 100

Example: 1613 / 14876 ≈ 10.84%



Characterizing Synthesis Results

Open library files (.LEF) and review synthesized outputs.

Verify timing and functional correctness.



---

Key Takeaways (Day 1):

Understanding chip packages and standard cells.

Basics of RISC-V and RTL design.

Overview of the ASIC design process and RTL to GDS2 flow.

Hands-on with OpenLane: preparing projects, running synthesis, and analyzing reports.
