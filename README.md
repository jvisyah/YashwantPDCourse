# YashwantPDCourse
Notes for PD Course from Kunal Ghosh

ASIC design flow

Synthesis
	mapping of lib std cells to RTL code, tool - yosys
Floorplanning
	Planning die area of IC based on gate level netlist cells and core utilization %. Pin locations and macro locations (pre-placed cells) on die.
Powerplanning
	Define pg grid structure. Global placement->optimal placement of instnces according to timing constraint, can be invalid placement. Detailed placement- >legalizing global placement.
CTS (clock tree synthesis)
	placing clock network conneting sequential cells and clock instnces.
Routing
	Signal routing between input and output data pins of instances.
Signoff
	physical verification - DRC,LVS. timing verification - STA, CDC. fault check - DFT.


Synthesis of picv32a design
Using picrv32a design for synthesis

Task: To find FF ratio in design-> FF ratio: 1613/14876=0.1084 (10.8%)

checking config.tcl in design/picrv32a/src dir.
![image](https://github.com/user-attachments/assets/4841c8ac-31d1-407d-bb4e-1d3e5ea63c6a)

preparing design in openlane
![image](https://github.com/user-attachments/assets/8d48f2f5-495e-4878-9973-7cc2de6cbe68)

running synthesis (command: run_synthesis)
![image](https://github.com/user-attachments/assets/4579139a-1d29-462f-b43f-8d07b0690ffb)


verilog vs gate level netlist
![image](https://github.com/user-attachments/assets/bc6ad5f3-9116-40df-9264-be10c1a86f66)

synthesis report
![image](https://github.com/user-attachments/assets/cf7f8654-29a8-437b-aa42-152c9fe419b3)


Day - II (Floor Planning)

Define Width and Height of Core and Die ?

![image](https://github.com/user-attachments/assets/b1a16439-dbc7-4d1f-9886-57ba6dabffeb)

Utilization Factor = (Area Occupied by Netlist) / (Total Area of Core)


![Untitled picture](https://github.com/user-attachments/assets/e9b7bb37-cd23-4406-93cb-8553bbbbd5d8)


Aspect Ratio -



If Aspect Ratio = 1 (Die is a Square)
If Aspect Ratio > or <1 (Die will be Rectangle)




** LAB**

In OpenLANE, we have a lot of switches (variables/options/settings) for each step (synthesis, floorplan, placement, etc) to adjust the flow that is present in README.md inside the configuration dir of OpenLANE.

![image](https://github.com/user-attachments/assets/d2fbde0a-4461-4a63-aa9b-b5ed2e38b65f)


README.md:

![image](https://github.com/user-attachments/assets/b5af01d6-d7c3-4f5f-8eb3-d41e08a80452)


![image](https://github.com/user-attachments/assets/402d4573-c1c5-4e05-8baf-0b48bedb2b9d)


Where are the switches set?

In the .tcl file of the specific stage, Ex: floorplan.tcl file in configuration dir is used for setting switches for floorplan.

less floorplan.tcl

![image](https://github.com/user-attachments/assets/2ee3dbbc-733e-46b1-8d41-b013d0120eb6)




floorplan.tcl has the lowest priority.

priority order: floorplan.tcl < config.tcl (openlane/designs/picorv32a) < pdk specific config file (openlane/designs/picorv32a) [Day1, point3]

In openlane flow, Horizontal metals (FP_IO_HMETAL) and Vertical metals (FP_IO_VMETAL) are 1 more than what is specified in the config file.

Ran the floorplan stage using command run_floorplan.

![image](https://github.com/user-attachments/assets/4158506e-ccd1-469e-8753-da3085f8347f)



Netlist Binding -

![image](https://github.com/user-attachments/assets/5f90b837-a600-4150-ba8a-69f09fc2211a)

In library different flavours of same types of cells are present (as we go from left to right cell of the cells are increasing).
Larger the size of cell -> Lesser will be the resistance path -> faster would be the cell



Placement -




Day IV (Prelayout timing analysis)

![image](https://github.com/user-attachments/assets/c1306fbb-f922-4a41-9430-a6ba8dd820f4)

Table in the left and right are the delay tables for different buffer cells. It captures the delay according to input slew and output load.


Setup Timing Analysis -

![image](https://github.com/user-attachments/assets/6618e886-ee71-41ce-a53c-4850fc97d4bc)

Theta (O) is the combinational scenario. 

![image](https://github.com/user-attachments/assets/27c867cb-79c4-4afb-9aa4-4eeffd2ba9df)

The internal of the flop can be represented by 2 MUX's.
As highlighted there will be sometime that will be spend by data to reach to Qm and this time needs to be subtracted from clock period and refered as setup time.


![image](https://github.com/user-attachments/assets/4b3d4274-1f0c-44f6-99d9-3e160dfbd2fe)

Clock Jitter -
![image](https://github.com/user-attachments/assets/aa9849da-96ba-4010-9cc6-5d27fb71e606)

Jitter is modelled using one more parameter "Uncertainity"



Identifying timing paths of design with single clock

![image](https://github.com/user-attachments/assets/3bdb8a8c-3efe-4a66-b4f3-4c40d7257ef8)


Steps to optimize synthesis to reduce setup violations





CTS (CLock Tree Synthesis) -

![image](https://github.com/user-attachments/assets/424164d9-cc75-4d26-93e6-960e507bac63)
Drawing a basic clock tree to connect the clock pins of all the flops. Due to difference in clock net lengths ther will be mor delay introduced in case of longer clock nets and will result in skew. Ideally it is expoected to have 0 skew.


![image](https://github.com/user-attachments/assets/46756c42-f1c3-4cf0-a561-6a7229726230)
Using H-Tree approach to generate a Clock tree.


![image](https://github.com/user-attachments/assets/681e32c2-e4db-4acf-a773-e5aff0953c71)

Another problem is that at the otehr end of clock network (highlighted in above pic) the transition time would be so large because of huge delay across the path that it will cause a timing issue and signal integrity issue.
To avoid this we need to repeaters.



