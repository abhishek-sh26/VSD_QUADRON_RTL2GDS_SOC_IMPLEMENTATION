# VSD_QUADRON_RTL2GDS_SOC_IMPLEMENTATION
# 🚀 Week 1 Learning – RTL2GDS using OpenLane & Sky130
## Initial step to my workshop on RTL to GDSII flow using OpenLane and Sky130 PDK. It includes both theoretical understanding and hands-on implementation.

- Implemented full RTL → GDS flow
- Understood physical meaning behind each stage
- Analyzed timing, area, and power reports
- Executed flow using GitHub Codespaces


# **Introduction**
## 🔹 **what is RTL2GDS?**
### RTL-to-GDS is the complete VLSI design lifecycle that transforms a high-level digital description into a manufacturable physical layout.

<img width="812" height="467" alt="RTL2GDS flow" src="https://github.com/user-attachments/assets/c90b3edf-0191-4064-97c7-dd83d1ece8b9" />

### **The flow includes** 
- Synthesis
- Floorplan
- Placement
- CTS
- Routing

# 🔹 **what is openlane?**
OpenLane is an open-source ASIC design flow that automates the RTL to GDSII process using Sky130 PDK. It provides an easy way to perform synthesis, floorplanning, placement, routing, and timing analysis.

# 🔹 **Tools used by openlane**
- Yosys    : synthesis of RTL
- ABC      : mapping netlist
- Magic    : layout
- Netgen   : SPICE exratraction and simulation
- OpenSTA  : Static timing analysis
- OpenROAD : Floorplanning,Placement,CTS and routing


# 🔹 **open lane flow**

<img width="1032" height="731" alt="Openlane flow" src="https://github.com/user-attachments/assets/70e08f67-2f8e-47f3-8ffc-d5233afc523b" />

# 🔹 **What is Sky130 PDK?**
Sky130 is an open-source Process Design Kit (PDK) provided by Google and SkyWater. It contains all the required files to design and manufacture ICs at 130nm technology node.

## **It includes:**
- Standard cell libraries
- Design rules (DRC)
- Layout vs schematic rules (LVS)
- SPICE models

# 🔹 **invoking openlane**

<img width="1182" height="334" alt="openlane tool invoke " src="https://github.com/user-attachments/assets/7e33f8ad-0d6c-4c46-9a3b-ad51b1b3e402" />

## once after invoking the openlane the tool is ready to perform and the 4 main steps should be followed every time to invoke the tool

- **openlane**                      : navigates to openlane directory.
  
- **make mount**                    : Mounts your current OpenLane directory into the container. (without this openlane cannot be accessed)
  
- **./flow.tcl -interactive**       : Runs a TCL script (flow.tcl) step-by-step control instead of running everything automatically.
  
- **package require openlane 0.9**  : TCL checks if OpenLane package is available, It activates OpenLane commands inside the TCL shell

## **next step start preparing design** 
-  **prep -design picorv32a**        : Initializes design environment
  
                              -Loads RTL files
                              -Loads config (config.tcl)
                              -Sets environment variables
                              -Creates a runs/ directory

   
<img width="977" height="237" alt="prep -design" src="https://github.com/user-attachments/assets/cff4095c-34ae-4f91-b4e3-c1837e53336f" />


# 🔹 **what is picorv32a ?**

   ## picorv32a is a compact and efficient RISC-V based 32-bit processor core written in Verilog, commonly used in OpenLane and Sky130-based ASIC design flows for learning and implementation.

   
### Role of picorv32a in OpenLane Flow
#### Used as a sample design
#### Input RTL for:
            - Synthesis
            - Floorplanning
            - Placement
            - Routing
            

   
# 🔥 **run_synthesis**                 
  ## Converts RTL → Gate-level netlist
                              -Uses Yosys
                              -Maps logic to standard cells (Sky130)
                              
<img width="1176" height="153" alt="synthesis run" src="https://github.com/user-attachments/assets/e8467a18-194f-46b5-bc30-8d035c076ca5" />

## Output:

- Netlist (.v)
  
<img width="865" height="529" alt="gate netlist 1" src="https://github.com/user-attachments/assets/1a67a600-e6b4-4889-895d-7e1e5a08c604" />

<img width="278" height="620" alt="gate netlist 2" src="https://github.com/user-attachments/assets/13c2d8fc-aefe-42a2-a022-d84d8a1e9c26" />

- Area report

<img width="731" height="97" alt="area report" src="https://github.com/user-attachments/assets/6cb4e325-7ced-4656-8c84-97733e739a56" />

- Timing report

<img width="759" height="583" alt="synth timing report" src="https://github.com/user-attachments/assets/8fd5db51-871a-4767-8b7d-cad2b4683a37" />

- power report

<img width="860" height="360" alt="synth power report" src="https://github.com/user-attachments/assets/51e10566-9d46-4fc0-8aaf-eeb2363d6001" />

## **some synthesis strategy to modify**

- set ::env(SYNTH_STRATEGY) 1
- set ::env(SYNTH_SIZING) 1
- set ::env(SYNTH_BUFFERING) 1
- set ::env(SYNTH_MAX_FANOUT) 4
**rerun synthesis to take effect**

**To reduce slack and meet timing ECO fixing** 
replace_cell <instance> high_drive_strength_buffer /repeator
## Buffer list :
> sky130_fd_sc_hd__clkbuf_1 , sky130_fd_sc_hd__clkbuf_2 , sky130_fd_sc_hd__clkbuf_4 , sky130_fd_sc_hd__clkbuf_8

## if the timing is violating in any instance change in drive strength og buffer for that instance with high buffer


## **The power values obtained during synthesis are preliminary estimates and may vary after placement and routing due to parasitic effects.**



# 🔥 **run_floorplan**                 
## Defines chip layout structure

                              -Sets die area & core area
                              -Places IO pins
                              -Defines utilization
                              
<img width="1035" height="182" alt="floorplan-run" src="https://github.com/user-attachments/assets/f3921ba2-5b7f-4ade-82ea-db9dfe629d4a" />
  
- core utilization 50%
- aspect ratio (height/width) 1
- shape Rectangle

# Output:
- Floorplan DEF file
## **Layout view**
to veiw layout view 
### go to port section in 6080 port click on globe symbol it will opens vnc window 

<img width="1232" height="230" alt="port_selection" src="https://github.com/user-attachments/assets/00c56023-6a00-44df-bd91-9947920a7c03" />

### open terminal and type commands 

<img width="1280" height="267" alt="layout launch" src="https://github.com/user-attachments/assets/83955c55-c1d3-4530-b486-d048546cd4a4" />

### it will opens tkcon window and layout veiw window read the lef and def files in the tkcon window

<img width="855" height="512" alt="lef read" src="https://github.com/user-attachments/assets/9eb69531-8172-4e3f-a2b7-ccc108ddbee9" />

<img width="480" height="185" alt="fp def read" src="https://github.com/user-attachments/assets/47d1d088-613e-414c-9dfd-b7624de2ee04" />

###after running lef and def file the layout is ready to

# **Final layout of floorplan**

<img width="1259" height="698" alt="Screenshot 2026-02-23 at 6 03 23 PM" src="https://github.com/user-attachments/assets/23fb6138-dbf0-478d-81ff-5498a01c8db9" />



# 🔥 **run_placement**                     
##  Places standard cells inside the core  
                          -Global placement
                          -Detailed placement
                          -Optimization for timing
                          
<img width="1063" height="244" alt="placement run" src="https://github.com/user-attachments/assets/02dc2e67-c97e-4bbc-9374-c96988f0ed24" />


# Output:
- Placement DEF
- Congestion info
- layout
## **to view layout**
- in the same vnc window change the directory from floorplan to placement and type command 

<img width="1270" height="230" alt="placement layout view command " src="https://github.com/user-attachments/assets/a5f96643-edce-428a-addb-63789e4d9b91" />

- it will opens tkcon window and layout veiw window read the lef and def files in the tkcon window

<img width="797" height="513" alt="placement lef" src="https://github.com/user-attachments/assets/4e6f3966-54ea-4f03-9e2f-50c8ff15a721" />

<img width="391" height="152" alt="placement def" src="https://github.com/user-attachments/assets/b377d67f-bdf3-4588-bc4c-e9775b534adb" />

## **3).final layout view** 

<img width="1245" height="698" alt="Screenshot 2026-02-24 at 2 03 05 PM" src="https://github.com/user-attachments/assets/ac15a84f-7a0e-486b-b230-2b96cbd2b137" />

## **layout view of how std cells are actually placed**

<img width="1246" height="694" alt="Screenshot 2026-02-24 at 2 13 11 PM" src="https://github.com/user-attachments/assets/d3899686-cfe8-4f4e-8e21-2b3661f26870" />


- timing info after placement 

<img width="688" height="567" alt="placement timing report" src="https://github.com/user-attachments/assets/3e5e243a-b889-4137-915b-9f592d4ee86e" />



# 🔥 **run_cts**     
## Builds clock distribution network and ensures clock reaches all flip-flops properly 
             -inserts clock buffers
             -Balances clock delay (skew)
             
<img width="1144" height="97" alt="cts run" src="https://github.com/user-attachments/assets/f0791ec2-f6cd-4164-a66e-b8c677c956c7" />


### CTS Report Contains:
- Clock Skew                    :Difference in clock arrival time
                               -Skew = Max delay - Min delay
                               -Smaller skew = better design
- Clock Latency                 :Time taken for clock to reach flip-flops
- Number of Buffers Inserted    :Shows how many buffers were added
  Buffer list
> sky130_fd_sc_hd__clkbuf_1 , sky130_fd_sc_hd__clkbuf_2 , sky130_fd_sc_hd__clkbuf_4 , sky130_fd_sc_hd__clkbuf_8
- Transition/Slew               :Signal rise/fall quality
- Timing Report (Post-CTS)      :Updated setup & hold timing

<img width="695" height="565" alt="cts timing report" src="https://github.com/user-attachments/assets/fe124d9d-a6b9-4a37-a949-a1befa894625" />

<img width="651" height="114" alt="cts report" src="https://github.com/user-attachments/assets/c044cbdd-4dbc-4837-9f62-8ff48074ffc8" />


## Why CTS is Important
✔ Reduces clock skew
✔ Improves timing closure
✔ Ensures reliable operation

Improper clock tree design can lead to timing violations due to excessive skew or latency.

# 🔥 **gen_pdn**
<img width="1152" height="69" alt="gen pdn" src="https://github.com/user-attachments/assets/4d9c2d94-91a7-4e5c-b94d-eb97e98cc34b" />

## Generates the Power Distribution Network (PDN) for the design.
## It creates power (VDD) and ground (VSS) connections across the chip so all cells get proper supply.

### Internally it
- Creates power rings around the core
- Adds horizontal & vertical straps
- Connects:
         -- Standard cells
         -- Macros (if any)

  <img width="629" height="53" alt="pdn info 1" src="https://github.com/user-attachments/assets/867d5c9a-36f4-4130-b986-d09134798878" />

  <img width="559" height="72" alt="pdn info 2" src="https://github.com/user-attachments/assets/97633945-32fd-4f8b-a526-3cb55042dad5" />


## Why it is Important
✔ Ensures stable power delivery
✔ Prevents voltage drop (IR drop)
✔ Improves reliability of the chip


# 🔥 **run_routing**

<img width="1157" height="468" alt="routing run" src="https://github.com/user-attachments/assets/297f7bfe-ce30-4f53-8671-9d2062929f19" />


## Performs routing to connect all placed cells and clock tree with metal layers.
### Global Routing (FastRoute)
- Plans approximate paths for all nets
- Reduces congestion

### Detailed Routing (TritonRoute)
- Creates exact wire connections
- Follows Sky130 DRC rules

### Post-routing Optimization
- Fixes violations
- Improves timing (if needed)

<img width="1138" height="615" alt="Screenshot 2026-02-27 at 6 54 53 PM" src="https://github.com/user-attachments/assets/52dc4331-bfbf-4a6c-88bb-aefb6d37c7b1" />


## In OpenLane,routing is done using FastRoute and TritonRoute to connect all nets while satisfying DRC and generating parasitics.


<img width="907" height="727" alt="routing report" src="https://github.com/user-attachments/assets/78168673-1f56-4b3a-9e46-9b44c48919d4" />


## **Why Important in OpenLane**

✔ Ensures all nets are connected
✔ Generates parasitics (used for timing analysis)
✔ Prepares design for final verification (DRC/LVS)




         







