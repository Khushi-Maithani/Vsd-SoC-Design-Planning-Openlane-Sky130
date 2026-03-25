# **Day 1 – Sky130 Workshop: Inception of Open Source EDA, OpenLane, and Sky130 PDK**

**Overview:**
Day 1 introduces the fundamentals of SoC design using open-source EDA tools, Sky130 PDK, and OpenLane. The workshop is divided into three main parts: **How to talk to computers**, **SoC Design and OpenLane**, and **Get familiar with Open Source EDA tools**.

---

## **1. How to Talk to Computers**

This section covers the basics of computer hardware, RISC-V architecture, software-to-hardware translation, and standard libraries.

**Chip Packaging and Fundamentals:**
<img width="677" height="431" alt="Screenshot (59)" src="https://github.com/user-attachments/assets/c046d4bd-5ef2-484c-9695-3b2587ee6145" />

* **QFN48 Package:** A compact IC package with 48 pins designed for surface mounting.
<img width="759" height="406" alt="Screenshot (60)" src="https://github.com/user-attachments/assets/2b0e6d5d-2fa8-4c11-94f1-bca5eace6e8d" />


* **Chip Components:** Pads, cores, die, and IP (Intellectual Property) blocks. Pads connect external signals, cores contain functional units, and die is the physical silicon.

* -->A semiconductor die is the functional heart of a chip, containing circuitry (IP cores) and connection points (pads) for external interfacing. IP cores are reusable design blocks (e.g., processors, memory) that accelerate design, while pads provide the interface for signals between the die and package via wire bonding or flip-chip techniques.

* **Standard Library Cells:** Pre-designed cells used for constructing digital logic circuits efficiently.

**Introduction to RISC-V:**
 Generally speaking, we use coding languages like C, Java, and others to write programs that must be performed by the system, but machines are unable to comprehend these languages. Here's when ISA enters the picture. The written codes will be translated from assembly language to binary, or machine comprehensible language, using ISA. The RISC V ISA is the most recent ISA to be published, and it serves this purpose

 <img width="822" height="527" alt="Screenshot (62)" src="https://github.com/user-attachments/assets/f212147a-c39f-4a15-8c33-dd8023443e96" />
 
* **ISA (Instruction Set Architecture):** Defines all instructions a processor can execute.
* **PicoRV32A:** A small, open-source RISC-V CPU core.
* **RTL (Register Transfer Level):** Describes the hardware logic at a high level before synthesis.

**Software to Hardware Translation:**
<img width="825" height="457" alt="Screenshot (61)" src="https://github.com/user-attachments/assets/cad76215-3ea2-497e-961d-a37af0ed95f1" />

* **Compiler:** Converts high-level programming code into assembly/machine code.
* **Assembler:** Converts assembly code into machine language instructions.
* **Operating System (OS):** Manages hardware, executes programs, allocates memory, handles I/O, and performs low-level system functions.

---

## **2. SoC Design and OpenLane**

This section covers digital ASIC design concepts, RTL-to-GDS2 flow, and key steps in layout and verification.

**ASIC Design Concepts:**

* **ASIC (Application-Specific Integrated Circuit):** Custom-designed chips for specific applications.
* **EDA Tools:** Software used to design, simulate, and verify integrated circuits.
* **PDK (Process Design Kit):** Collection of files modeling fabrication process, including layers, standard cells, and design rules.

**Simplified RTL to GDS2 Flow:**
The complete flow from RTL to final layout is:

**RTL Design → Synthesis → Floor Planning → Power Planning → Placement → Clock Tree Synthesis → Routing → Sign-off**

**Step Descriptions:**

* **Synthesis:** Converts RTL to gate-level netlist using standard library cells.
* **Floor Planning:**

  * **Chip Floor Planning:** Organizes the macro blocks on the chip.
  * **Macro Floor Planning:** Places large functional blocks for optimal routing.
* **Power Planning:** Creates power and ground rails, ensures proper voltage distribution.
* **Placement:** Positions all standard cells accurately within defined areas.
* **Clock Tree Synthesis (CTS):** Ensures clock signal reaches all flip-flops with minimal skew.
* **Routing:** Connects all cells using metal layers according to netlist connectivity.
* **Sign-Off Verification:**

  * **DRC (Design Rule Check):** Checks that layout follows fabrication rules.
  * **LVS (Layout vs. Schematic):** Verifies layout matches circuit schematic.
  * **Static Timing Analysis (STA):** Ensures the circuit meets timing constraints.

**Flow Diagram Example:**

* **RTL → Synthesis → Floor Planning → Power Planning → Placement → CTS → Routing → Sign-Off**
* Each step ensures the design progresses from high-level logic to physically manufacturable silicon.

---

## **3. Get Familiar with Open Source EDA Tools**

This section introduces OpenLane directory structure, project preparation, running synthesis, and analyzing results.

**OpenLane Directory Structure:**

* Organized into project folders, scripts, configuration files, and design outputs.

**Design Preparation Steps:**

* Create new project, select PDK, import RTL files.
* Verify project structure before synthesis.

**Lab Work – Commands and Steps:**

1. **Navigate OpenLane Environment:**

   ```bash
   cd openlane
   ```
2. **Clone Repository and Enter Project Directory:**

   ```bash
   git clone <repo-link>
   cd <project-folder>
   ```
3. **Launch OpenLane Interactive Mode:**

   ```bash
   ./flow.tcl -interactive
   ```
4. **Run Synthesis:**

   ```bash
   run_synthesis
   ```
5. **View Synthesis Reports:**

   ```bash
   less run/PicoRV32A/report/synthesis/*.rpt
   ```
6. **Check Cell Statistics (Flop Ratio):**

   ```bash
   # Flop Ratio = Number of D flip-flops / Total number of cells
   # Example:
   1613 / 14876 ≈ 0.1084 → 10.84% DFF
   ```
7. **Open Library Files:**

   * `.LEF` files for standard cell definitions.

**Flop Ratio Calculation:**

* **Flop Ratio** = Number of D flip-flops ÷ Total number of cells
* **Percentage of DFFs** = Flop Ratio × 100
* Example: 1613 ÷ 14876 ≈ 10.84%

**Characterization of Results:**

* Open synthesized outputs and LEF files.
* Verify design correctness and timing.

---

## **Key Takeaways (Day 1)**

* Learned **chip packages, standard library cells, and hardware fundamentals**.
* Understood **RISC-V architecture and PicoRV32A core**.
* Hands-on experience with **OpenLane project setup, synthesis, and report analysis**.
* Gained knowledge of **RTL to GDS2 flow**, including **synthesis, floor planning, power planning, placement, CTS, routing, and sign-off**.
* Learned verification methods like **DRC, LVS, and STA**.

---
Directory Structure Setup:
<img width="959" height="517" alt="Screenshot (63)" src="https://github.com/user-attachments/assets/81a11c50-083b-4455-87ee-cb1ca6aa30c8" />

synthesis:

<img width="942" height="516" alt="Screenshot (64)" src="https://github.com/user-attachments/assets/900ba906-db92-418b-a581-755a014d7d48" />

successful synthesis:

<img width="971" height="509" alt="Screenshot (65)" src="https://github.com/user-attachments/assets/dd910ab6-b728-4225-b0ec-0d6d76a0f7a9" />


