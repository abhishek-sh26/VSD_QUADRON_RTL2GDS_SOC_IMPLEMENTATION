# VSD_QUADRON_RTL2GDS_SOC_IMPLEMENTATION



## WEEK-1


<details>
<summary><strong> Learning – RTL2GDS using OpenLane & Sky130 </strong></summary>
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


<img width="1266" height="697" alt="routing veiw 1" src="https://github.com/user-attachments/assets/ecf03e13-32cc-4739-b213-cec6455926be" />


<img width="907" height="727" alt="routing report" src="https://github.com/user-attachments/assets/78168673-1f56-4b3a-9e46-9b44c48919d4" />


## **Why Important in OpenLane**

✔ Ensures all nets are connected
✔ Generates parasitics (used for timing analysis)
✔ Prepares design for final verification (DRC/LVS)

</details>

---

</details>

---
## WEEK2 Toolchain Mastery and ORFS Execution (Cloud to Local)

</details>
<details>
<summary><strong>PHASE-1 — ORFS Execution in GitHub Codespaces </strong></summary>

## Task 1.1 — Repository Setup

### Repository used : https://github.com/vsdip/vsd-scl180-orfs
#### Steps performed:

- Forked the repository to my GitHub account.
- Opened the repository in GitHub Codespaces.
- Allowed the devcontainer environment to build completely.
- Verified the toolchain installation inside the container.
  
<img width="638" height="71" alt="Dev build success " src="https://github.com/user-attachments/assets/49dd474c-5811-4fab-b358-62711bbe101f" />

<img width="1155" height="210" alt="setup success" src="https://github.com/user-attachments/assets/16e63a0a-0189-4c40-9a4b-44170bd08e95" />

### Terminal output :

- Openroad -version : v2.0-28075-g0f99689f45
- yosys -V  : Yosys 0.58+94 (git sha1 4011d7265, clang++ 18.1.8 -fPIC -O3)
- python3 —version : Python 3.10.12
- make —version :GNU Make 4.3  Built for x86_64-pc-linux-gnu

  <img width="934" height="297" alt="codespace versions OR" src="https://github.com/user-attachments/assets/08232824-8ad9-452a-9352-e3e58d8217a9" />

#### These commands confirm that the RTL-to-GDS toolchain is correctly installed inside the devcontainer environment.

## **Task 1.2** — Run Sky130 Testcase in Cloud
### Inside the code space :
#### Navigated to the design directory
- cd orfs/flow
#### Run the RTL-to-GDS flow:
- make

  ## what is make command and how it performs
### Make is a build automation tool used to execute a sequence of commands defined in a Makefile. It helps automate complex processes so that multiple tasks can be run with a single command.

### When the make command is executed, it reads the Makefile present in the directory. The Makefile contains rules and instructions that define how different stages of the workflow should be executed.

### In the ORFS RTL-to-GDS flow, make automatically runs the entire design process in the correct order. It calls different tools such as Yosys for synthesis, OpenROAD for physical design, and OpenSTA for timing analysis.

### Thus, instead of manually running each step, the make command executes all stages like synthesis, floorplanning, placement, clock tree synthesis, routing, and GDS generation in a single automated flow.

  
## The ORFS flow automatically performs the following stages:
- Synthesis
- Floorplanning
- Placement
- Clock Tree Synthesis
- Routing
- GDS generation
  
### Cloud Execution Results
#### Synthesis  : **make synth** 
- RTL is converted into a gate-level netlist using Yosys.

#### Floorplan : **make floorplan**
- Floorplanning defines the chip layout boundaries, placement rows, and IO positions.
#### make gui_floorplan : shows floorplan layout

<img width="1278" height="585" alt="floorplan codespace OR" src="https://github.com/user-attachments/assets/ccbe4781-c03f-4036-9682-89be87eae509" />

#### Placement : **make place**
- Placement arranges standard cells inside the chip area.

#### Make gui_place : shows placement layout 
<img width="1277" height="672" alt="placement codespace OR" src="https://github.com/user-attachments/assets/58a2e654-876f-40d4-8b90-a09077b33b79" />

#### Clock tree synthesis (CTS) :  **make cts**
 - CTS creates the clock distribution network to minimize skew and delay.

<img width="1270" height="668" alt="cts codespace OR" src="https://github.com/user-attachments/assets/ee3692ed-09bc-499e-a33a-2dfec4af74c3" />
 


#### Routing : make route
- Routing connects all nets using metal layers.

#### make gui_route :shows routed layout

<img width="1275" height="697" alt="route codespace OR" src="https://github.com/user-attachments/assets/8abdbec7-8f22-42a0-af9e-d6473aed308c" />


GDS (Codespaces): The final layout file GDSII is generated which represents the physical chip layout. 

Klayout veiw of final GDS generated in Codespaces   
<img width="1280" height="754" alt="klayout codespace OR" src="https://github.com/user-attachments/assets/20ac237a-ef05-492e-850a-4f9190e260b5" />

</details>

<details>
<summary><strong>PHASE-2 - Devcontainer Toolchain Understanding </strong></summary>

  ## **Files analyzed**

                        - .devcontainer/Dockerfile
                        - .devcontainer/install-openroad.sh

### These files define the complete RTL-to-GDS environment used inside GitHub Codespaces.

## **Task 2.1**
### Toolchain Mapping (RTL-to-GDS Flow) 

| Tool          | Installed From             | Purpose in Flow                                                                | Stage Used           |
| ------------- | -------------------------- | ------------------------------------------------------------------------------ | -------------------- |
| **OpenROAD**  | Built from source (GitHub) | Performs physical design tasks like placement, CTS, routing and GDS generation | Physical Design      |
| **Yosys**     | Open-source synthesis tool | Converts RTL (Verilog) into gate-level netlist                                 | Synthesis            |
| **TritonCTS** | Integrated with OpenROAD   | Builds clock distribution network                                              | Clock Tree Synthesis |
| **FastRoute** | OpenROAD module            | Performs global routing of nets                                                | Routing              |
| **OpenSTA**   | Integrated in OpenROAD     | Static timing analysis to check setup/hold violations                          | Timing Analysis      |
| **KLayout**   | Package manager / binary   | Used to visualize layout and GDS files                                         | Layout Viewing       |
| **Python**    | Package manager            | Used for automation scripts in the flow                                        | Flow Automation      |
| **Make**      | Linux build tool           | Executes Makefile and automates the ORFS flow                                  | Flow Control         |
| **Git**       | GitHub                     | Version control and repository management                                      | Development          |



## Task 2.2
### Flow Architecture Explanation

#### ORFS (OpenROAD Flow Scripts) automates the complete RTL-to-GDS flow using scripts and Makefiles.

#### The Makefile controls and executes each stage of the design flow in the correct order.

#### Synthesis is the first stage where RTL (Verilog) is converted into a gate-level netlist using Yosys.

#### After synthesis, the physical design stages begin in OpenROAD, including floorplanning, placement, clock tree synthesis (CTS), and routing.

#### Timing analysis is performed using OpenSTA to check timing constraints such as WNS and TNS.

#### After routing and timing verification, the final GDSII file is generated, which represents the physical layout of the chip ready for fabrication.

</details>

<details>
<summary><strong>PHASE-3 - Local Installation (Self-Owned Environment)  </strong></summary>

## ORFS Installation in Local Ubuntu machine
### Os ubuntu 22.04
<img width="1280" height="800" alt="Screenshot 2026-03-08 at 7 55 07 PM" src="https://github.com/user-attachments/assets/cfc2ad66-7c5c-4c2c-bf8f-fa3aa2e50e79" />


## Task 3.1 Install ORFS locally

### Step1 —Clone repository 
- git clone https://github.com/vsdip/vsd-scl180-orfs
  
### Step 2—enter into directory 
- cd vsd-scl180-orfs
- Verified all the files are downloaded properly
- 
#### Make —version
- Output : GNU Make 4.3
- 
  <img width="509" height="71" alt="Screenshot 2026-03-08 at 8 35 03 PM" src="https://github.com/user-attachments/assets/faab5ed8-1ec7-4c8f-ac6e-497094a3ba37" />
  
  ### Change directory to Desktop
cd Desktop

##  Install yosys locally 
### Step 1 — Clone the Repository
git clone https://github.com/YosysHQ/yosys.git

- Yosys requires additional libraries included as git submodules, so they must also be initialized. 

### Step 2 — Enter the Directory

cd yosys


### Step 3 — Build the Tool
####Compile the source code using:

- make -j$(nproc)

- This builds the Yosys synthesis tool from source.

### Step 4 — Install the Tool

- sudo make install

- This installs Yosys into the system directories so it can be used globally.

### Step 5 — Verify Installation

- yosys -V

- Output : Yosys 0.63+87

<img width="785" height="47" alt="Screenshot 2026-03-08 at 8 34 01 PM" src="https://github.com/user-attachments/assets/5fda2c1f-a8e0-467a-a221-43afcbeb4391" />

## Task 3.2 Install openroad locally 

### Step 1 — clone the repository
- git clone https://github.com/The-OpenROAD-Project/OpenROAD.git
- 
### Step 2 — Enter the Directory
- cd OpenROAD
- 
### Step 3 —Dependancy installation 
- sudo ./etc/DependencyInstaller.sh -base
- ./etc/DependencyInstaller.sh -common -local
- 
### Step 4 —OpenROAD Build 
- ./etc/Build.sh -threads=1
- 
### Step 5 —verify installation 
- openroad -version
- output : 26Q1-1752-gdc2afc9d47
- 
<img width="894" height="50" alt="Screenshot 2026-03-08 at 8 36 07 PM" src="https://github.com/user-attachments/assets/da3d6c79-23ad-4437-b4fe-a199e56e84e7" />

</details>

<details>
<summary><strong>PHASE-4 — Re-Run RTL-to-GDS Locally </strong></summary>


## After successfully installing ORFS and OpenROAD in the local Ubuntu environment, the same testcase used in the cloud environment was executed locally to verify the RTL-to-GDS flow.
### Step 1 — Navigate to ORFS flow Directory
cd vsd-scl180-orfs/orfs/flow
### Step 2 — Run RTL-to-GDS Flow
#### Made come changes for getting error:
-sed -i '/pin.*GCLK/a \        function : "(GATE * CLK)";' /home/abhishek/Desktop/vsd-scl180-orfs/orfs/flow/platforms/sky130hd/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
#### verify the changes made using grep
- grep -iB 1 "function.*GATE" /home/abhishek/Desktop/vsd-scl180-orfs/orfs/flow/platforms/sky130hd/lib/sky130_fd_sc_hd__tt_025C_1v80.lib | grep “GCLK"
  <img width="1206" height="308" alt="Screenshot 2026-03-08 at 8 44 49 PM" src="https://github.com/user-attachments/assets/de7d6ead-91fa-415f-807f-372e83449a18" />
 ## Execute the complete flow using the Makefile:
### export OPENROAD_EXE=/home/abhishek/Desktop/OpenROAD/build/bin/openroad
### export PLATFORM=sky130hd
### export DESIGN=riscv32i
### make clean_issues
## Run the flow RTL2GDS with the make command 
### make PLATFORM=sky130hd DESIGN=riscv32i

## make successfully finished 
<img width="993" height="203" alt="make success logs OR codespace" src="https://github.com/user-attachments/assets/271ba668-d123-4251-91e6-6b91b102bb8c" />


## The final Klayout veiw of the final.gds
<img width="1034" height="42" alt="Screenshot 2026-03-08 at 11 59 18 PM" src="https://github.com/user-attachments/assets/4f828aad-4214-48aa-aa41-490c06777567" />

<img width="1218" height="768" alt="Screenshot 2026-03-08 at 11 56 44 PM" src="https://github.com/user-attachments/assets/d73b6dbf-5854-46bb-bd01-74a6e4560bcb" />





</details>

<details>
<summary><strong>PHASE-5 Debugging and Unix Literacy  </strong></summary>

## 1. ls
### The ls (list) command is used to display the files and directories present in the current directory. It helps in checking the contents of folders such as designs, logs, reports, and results.
- Example: ls

## 2. cd
### The cd (change directory) command is used to move from one directory to another in the file system.
- Example: cd flow/designs

## 3. pwd
### The pwd (print working directory) command displays the current directory path. It helps verify the current location in the file system.
- Example: pwd

## 4. grep
### The grep command is used to search for specific words or patterns inside files. It is commonly used to identify errors, warnings, or timing violations in log files.
- Example: grep error log.txt

## 5. find
### The find command is used to search for files or directories within a specified path. It helps locate log files, reports, or generated outputs in the project directory.
- Example: find . -name “*.log”

## 6. cat
### The cat (concatenate) command is used to display the contents of a file directly in the terminal. It is useful for quickly viewing log or configuration files.
- Example: cat log.txt

## 7. less
### The less command is used to view large files one page at a time. It allows scrolling through large logs generated during synthesis, placement, or routing stages.
- Example: less log.txt

## 8. echo
The echo command prints text or variable values in the terminal. It is useful for displaying messages and debugging scripts.
- Example: echo RTL to GDS flow running

## 9. export
The export command is used to set environment variables in the current shell session. It helps configure tool paths so that tools like OpenROAD or Yosys can be executed from any directory.
- Example: export PATH=$PATH:/usr/local/bin

## 10. Environment Variables
### Environment variables store configuration values used by the operating system and tools. They help the system locate executables, libraries, and tool installations required for running the RTL-to-GDS flow.
- Example: echo $PATH

## 1. Navigating Directories
### Commands such as ls, cd, and pwd were used to navigate through the project directories. These commands help locate important folders such as designs, logs, reports, and results generated during the flow execution.

<img width="1217" height="229" alt="navigating " src="https://github.com/user-attachments/assets/94a645b8-c0b6-4776-b717-b31f5cc39900" />


<img width="993" height="203" alt="make success logs OR codespace" src="https://github.com/user-attachments/assets/666cb04d-9fd2-4cfd-be98-e4f90ac509e4" />


## 2. Searching Logs
### Log files generated during the RTL-to-GDS flow were inspected to identify errors and warnings. The grep command was used to search specific keywords inside log files.

<img width="1206" height="308" alt="Screenshot 2026-03-08 at 8 44 49 PM" src="https://github.com/user-attachments/assets/632bd9f0-71f6-4d98-9b4a-c705f434df5c" />


## 3. Filtering Timing Violations Using grep
### Timing reports generated during the flow were analyzed using the grep command to locate timing violations such as slack values.


<img width="974" height="323" alt=" slack timimg filtering " src="https://github.com/user-attachments/assets/38c9978d-015f-4919-a21e-54e58f4c97e0" />

## 4. Inspecting Makefiles
### The Makefile was inspected to understand how the ORFS flow orchestrates different stages such as synthesis, placement, and routing.

### cat Makefile

<img width="857" height="486" alt="cat makefile" src="https://github.com/user-attachments/assets/9388cedb-4f43-472e-836b-5fb3a7d58c98" />



  
## Conclusion

### In Week-2, the OpenROAD Flow Scripts (ORFS) were successfully executed in both GitHub Codespaces (cloud environment) and a local Ubuntu machine. This week provided hands-on experience with the complete RTL-to-GDS flow, including synthesis, floorplanning, placement, clock tree synthesis, routing, and final GDS generation.

### The toolchain used in the flow was studied by analyzing the devcontainer configuration, which helped in understanding the role of tools such as OpenROAD, Yosys, OpenSTA, TritonCTS, and FastRoute. The ORFS environment was also replicated locally by installing the required dependencies and building the OpenROAD tool from source.

### Additionally, basic Unix debugging commands such as ls, cd, pwd, grep, find, cat, and less were used to navigate directories, inspect logs, and analyze timing reports. This helped in developing debugging skills required for handling ASIC physical design flows.

### Overall, Week-2 helped build confidence in executing the RTL-to-GDS flow independently, understanding the underlying toolchain, and debugging design flows using Unix commands, which is essential for implementing complete SoC designs in the upcoming weeks.
  













