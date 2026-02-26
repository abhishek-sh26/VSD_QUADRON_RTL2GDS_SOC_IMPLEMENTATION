#VSD_QUADRON_RTL2GDS_SOC_IMPLEMENTATION

Initial step to my workshop on RTL to GDSII flow using OpenLane and Sky130 PDK. It includes both theoretical understanding and hands-on implementation.

#**Introduction**
what is RTL2GDS?
RTL-to-GDS is the complete VLSI design lifecycle that transforms a high-level digital description into a manufacturable physical layout.
<img width="812" height="467" alt="RTL2GDS flow" src="https://github.com/user-attachments/assets/c90b3edf-0191-4064-97c7-dd83d1ece8b9" />

**The flow includes** 
. Synthesis
. Floorplan
. Placement
. CTS
. Routing

#**what is openlane?**
OpenLane is an open-source ASIC design flow that automates the RTL to GDSII process using Sky130 PDK. It provides an easy way to perform synthesis, floorplanning, placement, routing, and timing analysis.

#**Tools used by openlane**
Yosys    : synthesis of RTL
ABC      : mapping netlist
Magic    : layout
Netgen   : SPICE exratraction and simulation
OpenSTA  : Static timing analysis
OpenROAD : Floorplanning,Placement,CTS and routing


#**open lane flow**
<img width="1032" height="731" alt="Openlane flow" src="https://github.com/user-attachments/assets/70e08f67-2f8e-47f3-8ffc-d5233afc523b" />

#**What is Sky130 PDK?**
Sky130 is an open-source Process Design Kit (PDK) provided by Google and SkyWater. It contains all the required files to design and manufacture ICs at 130nm technology node.

**It includes:**
.Standard cell libraries
.Design rules (DRC)
.Layout vs schematic rules (LVS)
.SPICE models

#**invoking openlane**
<img width="1182" height="334" alt="openlane tool invoke " src="https://github.com/user-attachments/assets/7e33f8ad-0d6c-4c46-9a3b-ad51b1b3e402" />

#once after invoking the openlane the tool is ready to perform and the 3 main steps should be followed every time to invoke the tool

.openlane                      : navigates to openlane directory.
.make mount                    : Mounts your current OpenLane directory into the container. (without this openlane cannot be accessed)
../flow.tcl -interactive       : Runs a TCL script (flow.tcl) step-by-step control instead of running everything automatically.
.package require openlane 0.9  : TCL checks if OpenLane package is available, It activates OpenLane commands inside the TCL shell

#**next step start preparing design** 
.prep -design picorv32a        :Initializes design environment
                              -Loads RTL files
                              -Loads config (config.tcl)
                              -Sets environment variables
                              -Creates a runs/ directory
<img width="977" height="237" alt="prep -design" src="https://github.com/user-attachments/assets/cff4095c-34ae-4f91-b4e3-c1837e53336f" />

.run_synthesis                :Converts RTL → Gate-level netlist
                              -Uses Yosys
                              -Maps logic to standard cells (Sky130)
<img width="1176" height="153" alt="synthesis run" src="https://github.com/user-attachments/assets/e8467a18-194f-46b5-bc30-8d035c076ca5" />

Output:
.Netlist (.v) <img width="865" height="529" alt="gate netlist 1" src="https://github.com/user-attachments/assets/1a67a600-e6b4-4889-895d-7e1e5a08c604" />
              <img width="278" height="620" alt="gate netlist 2" src="https://github.com/user-attachments/assets/13c2d8fc-aefe-42a2-a022-d84d8a1e9c26" />

.Area report   <img width="731" height="97" alt="area report" src="https://github.com/user-attachments/assets/6cb4e325-7ced-4656-8c84-97733e739a56" />

.Timing report <img width="759" height="583" alt="synth timing report" src="https://github.com/user-attachments/assets/8fd5db51-871a-4767-8b7d-cad2b4683a37" />

.power report <img width="860" height="360" alt="synth power report" src="https://github.com/user-attachments/assets/51e10566-9d46-4fc0-8aaf-eeb2363d6001" />

**some synthesis strategy to modify**

set ::env(SYNTH_STRATEGY) 1
set ::env(SYNTH_SIZING) 1
set ::env(SYNTH_BUFFERING) 1
set ::env(SYNTH_MAX_FANOUT) 4
**rerun synthesis to take effect**



.run_floorplan                :Defines chip layout structure
                              -Sets die area & core area
                              -Places IO pins
                              -Defines utilization
  <img width="1035" height="182" alt="floorplan-run" src="https://github.com/user-attachments/assets/f3921ba2-5b7f-4ade-82ea-db9dfe629d4a" />
core utilization 50%
aspect ratio (height/width) 1
shape Rectangle

Output:
Floorplan DEF file
**Layout view**
to veiw layout view 
1). go to port section and select 6080 port it will opens vnc window 
<img width="1232" height="230" alt="port_selection" src="https://github.com/user-attachments/assets/00c56023-6a00-44df-bd91-9947920a7c03" />
2). open terminal and type commands 
<img width="1280" height="267" alt="layout launch" src="https://github.com/user-attachments/assets/83955c55-c1d3-4530-b486-d048546cd4a4" />
3). it will opens tkcon window and layout veiw window read the lef and def files in the tkcon window
<img width="855" height="512" alt="lef read" src="https://github.com/user-attachments/assets/9eb69531-8172-4e3f-a2b7-ccc108ddbee9" />
<img width="480" height="185" alt="fp def read" src="https://github.com/user-attachments/assets/47d1d088-613e-414c-9dfd-b7624de2ee04" />
after running lef and def file the layout is ready to
**Final layout of floorplan**
<img width="1259" height="698" alt="Screenshot 2026-02-23 at 6 03 23 PM" src="https://github.com/user-attachments/assets/23fb6138-dbf0-478d-81ff-5498a01c8db9" />



.placement 






