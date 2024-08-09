# YashwantPDCourse
Notes for PD Course from Kunal Ghosh


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

image

README.md:

image

image

Where are the switches set?

In the .tcl file of the specific stage, Ex: floorplan.tcl file in configuration dir is used for setting switches for floorplan.

less floorplan.tcl

image

floorplan.tcl has the lowest priority.

priority order: floorplan.tcl < config.tcl (openlane/designs/picorv32a) < pdk specific config file (openlane/designs/picorv32a) [Day1, point3]

In openlane flow, Horizontal metals (FP_IO_HMETAL) and Vertical metals (FP_IO_VMETAL) are 1 more than what is specified in the config file.

Ran the floorplan stage using command run_floorplan.

image
