# SoC Design & Planning – OpenLane Sky130 Workshop

## Overview
This repository presents my learning from the VSD SoC Design and Planning Workshop. It covers the complete ASIC RTL to GDSII design flow using open-source EDA tools.

The workshop helped me understand how a design moves step by step from RTL code to a final physical chip layout.

---

## Learning Objective
- To understand the complete RTL to GDSII design flow  
- To learn how different stages of chip design are connected  
- To explore the use of open-source EDA tools in SoC design  
- To gain basic understanding of physical design and implementation  

---

## Tools Used
- OpenLane  
- OpenROAD  
- Magic  
- NGSpice  
- Sky130 PDK  

---

## Design Flow
RTL → Synthesis → Floorplanning → Placement → CTS → Routing → GDSII 


<img width="812" height="375" alt="Screenshot (58)" src="https://github.com/user-attachments/assets/2bee478f-bf66-42c1-a5bf-52ce7b9ee962" />

This diagram shows a simplified view of the RTL to GDSII flow, highlighting key stages like synthesis, floorplanning, placement, clock tree synthesis, routing, and sign-off, which together convert a design into a manufacturable chip.

<img width="798" height="362" alt="Screenshot (57)" src="https://github.com/user-attachments/assets/43995de9-99be-4f32-9ada-24f9b6d91af6" />

## Workshop Breakdown

- *Day 1:* Introduction to chip components, RISC-V, RTL concepts, OpenLane, and Sky130 PDK
     -->Covered the basics of chip design including chip components, RISC-V architecture, RTL concepts, and introduction to OpenLane and Sky130            PDK, along with the overall RTL to GDSII flow.
- *Day 2:* Floor planning concepts, utilization, and standard cell libraries
     -->Focused on floor planning concepts such as utilization, aspect ratio, and power planning, along with understanding standard cell libraries         and their role in design.
- *Day 3:* Layout design using Magic and basic simulation using NGSpice
     -->Involved layout design using Magic, CMOS inverter concepts, and basic simulation and characterization using NGSpice.
- *Day 4:* Timing analysis, setup/hold time, and clock tree concepts
      --> Covered timing analysis concepts including setup and hold time, along with the importance of clock tree synthesis in maintaining proper            signal timing.
- *Day 5:* Complete RTL to GDSII flow and final implementation steps
      --> Brought together the complete RTL to GDSII flow, including routing, final layout generation, and understanding the steps required for             chip fabrication.


## Conclusion
This workshop gave me a clear understanding of how SoC design works in practice. I learned how each stage of the design flow contributes to building a complete chip and how open-source tools are used in real workflows.

---

## Note
This workshop highlights the growing importance of open-source EDA tools in modern chip design. It also shows how complex systems can be designed using accessible tools, making SoC design more approachable for learners and beginners.

---

## Acknowledgment
I would like to thank NASSCOM and Mr. Kunal Ghosh for organizing this workshop and providing the opportunity to learn about SoC design. I also sincerely thank all the instructors for their clear explanations and guidance throughout the sessions, which made complex concepts easier to understand.
