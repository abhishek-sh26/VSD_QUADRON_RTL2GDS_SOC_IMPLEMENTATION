<img width="1032" height="731" alt="Openlane flow" src="https://github.com/user-attachments/assets/c31fbeb1-395d-4326-beb9-718aa7b4532d" /># VSD_QUADRON_RTL2GDS_SOC_IMPLEMENTATION

Initial step to my workshop on RTL to GDSII flow using OpenLane and Sky130 PDK. It includes both theoretical understanding and hands-on implementation.

##Introduction
what is RTL2GDS?
RTL-to-GDS is the complete VLSI design lifecycle that transforms a high-level digital description into a manufacturable physical layout.
<img width="812" height="467" alt="RTL2GDS flow" src="https://github.com/user-attachments/assets/c90b3edf-0191-4064-97c7-dd83d1ece8b9" />

The flow includes 
. Synthesis
. Floorplan
. Placement
. CTS
. Routing

what is openlane?
OpenLane is an open-source ASIC design flow that automates the RTL to GDSII process using Sky130 PDK. It provides an easy way to perform synthesis, floorplanning, placement, routing, and timing analysis.

##Tools used by openlane
Yosys    : synthesis of RTL
ABC      : mapping netlist
Magic    : layout
Netgen   : SPICE exratraction and simulation
OpenSTA  : Static timing analysis
OpenROAD : Floorplanning,Placement,CTS and routing


open lane flow
<img width="1032" height="731" alt="Openlane flow" src="https://github.com/user-attachments/assets/70e08f67-2f8e-47f3-8ffc-d5233afc523b" />

##What is Sky130 PDK?
Sky130 is an open-source Process Design Kit (PDK) provided by Google and SkyWater. It contains all the required files to design and manufacture ICs at 130nm technology node.

It includes:
.Standard cell libraries
.Design rules (DRC)
.Layout vs schematic rules (LVS)
.SPICE models

##invoking openlane
<img width="1182" height="334" alt="openlane tool invoke " src="https://github.com/user-attachments/assets/7e33f8ad-0d6c-4c46-9a3b-ad51b1b3e402" />

##once after invoking the openlane the tool is ready to perform and the 3 main steps should be followed every time to invoke the tool
.openlane                      : navigates to openlane directory.
.make mount                    : Mounts your current OpenLane directory into the container. (without this openlane cannot be accessed)
../flow.tcl -interactive       : Runs a TCL script (flow.tcl) step-by-step control instead of running everything automatically.
.package require openlane 0.9  : TCL checks if OpenLane package is available, It activates OpenLane commands inside the TCL shell

##next step start preparing design 
.prep -design picorv32a        :Initializes design environment
                              -Loads RTL files
                              -Loads config (config.tcl)
                              -Sets environment variables
                              -Creates a runs/ directory
<img width="977" height="237" alt="prep -design" src="https://github.com/user-attachments/assets/cff4095c-34ae-4f91-b4e3-c1837e53336f" />

.run_synthesis                :Converts RTL → Gate-level netlist
                              -Uses Yosys
                              -Maps logic to standard cells (Sky130)
Output:
Netlist (.v)
Area report
Timing report




