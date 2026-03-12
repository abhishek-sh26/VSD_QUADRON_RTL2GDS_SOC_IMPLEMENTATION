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

## WEEK-2 Toolchain Mastery and ORFS Execution (Cloud to Local)

### The objective of Week-2 was to gain practical experience executing the RTL-to-GDS flow using OpenROAD Flow Scripts (ORFS) both in GitHub Codespaces (cloud) and local Linux environment. This week focused on understanding the physical design toolchain, installing OpenROAD locally, running the complete flow, and developing Unix debugging skills required for ASIC physical design.

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
#### Made some changes for getting error:
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
  

</details>


---

</details>


## WEEK3 Block-Level Verification of VSDSquadron SoC

### The objective of Week-3 is to understand the block-level verification flow of the VSDSquadron SoC before moving to full-chip integration.

</details>
<details>
<summary><strong>PHASE-1 — Standalone Block Verification </strong></summary>

## In this phase, the following tasks were performed:
	 •	Clone the VSDSquadron SoC repository
	 •	Setup the verification environment
	 •	Run standalone block verification tests
	 •	Understand the Makefile-based verification flow
	 •	Execute the SPI Master test
	 •	Analyze the simulation output
	 •	Understand how PASS / FAIL is determined

## 1. Repository Setup
### The first step was cloning the official repository.

     git clone https://github.com/vsdip/vsdsquadron-soc
     cd vsdsquadron-soc

  

### Switch to the required branch used in the workshop:
     git checkout add-vsdsquadron-soc-folders


## 2. Install Required Tools

### The standalone verification flow requires:
	
•	RISC-V compiler
•	Verilog simulator
•	Make utility

### Install them using:
      sudo apt update
      sudo apt install iverilog gcc-riscv64-unknown-elf make

### Verify installation:
#### iverilog -V
- **output** : Icarus Verilog version 12.0 (stable) ()
 
#### riscv64-unknown-elf-gcc --version
- **output** : riscv64-unknown-elf-gcc (13.2.0-11ubuntu1+12) 13.2.0

## 3. Navigate to Verification Directory
### Standalone verification tests are located in:
     cd caravel_mgmt_soc_litex/verilog/dv/tests-standalone

### List available tests:
     ls

- **output** :
  debug  generated  gpio_mgmt  irq  Makefile  mem  run-all.sh  run.sh  spi_master  timer  uart

<img width="999" height="49" alt="standalone tests list" src="https://github.com/user-attachments/assets/2576d616-3d9f-4d2a-bc4b-95f440dff5f9" />

## Each directory represents a standalone hardware block verification test.

## 4. Fix Repository Path Expectations

### The Makefiles in the verification environment expect the repository to exist in the following location:
     /home/vsduser/
  
### the repository was cloned in the local system at:
     /home/abhishek/Desktop/vsdsquadron-soc

### To resolve this path mismatch, symbolic links were created so that the Makefiles could locate the required design files.

### Commands used:
sudo mkdir -p /home/vsduser

     sudo ln -s /home/abhishek/Desktop/vsdsquadron-soc/caravel_mgmt_soc_litex /home/vsduser/caravel_mgmt_soc_litex

       sudo ln -s /home/abhishek/Desktop/vsdsquadron-soc/caravel /home/vsduser/caravel

## 5. Fix Missing SRAM Model

### While running the simulation, the following error occurred:
     sky130_sram_2kbyte_1rw1r_32x512_8.v: No such file or directory

### This happened because the SRAM model required for simulation was not present in the repository.

### To resolve this issue, a placeholder SRAM model file was created.

#### Command used:
     mkdir -p /home/abhishek/Desktop/vsdsquadron-soc/caravel_mgmt_soc_litex/verilog/cvc-pdk
     touch /home/abhishek/Desktop/vsdsquadron-soc/caravel_mgmt_soc_litex/verilog/cvc-pdk/sky130_sram_2kbyte_1rw1r_32x512_8.v

### This allowed the Verilog simulator to compile the design hierarchy successfully.


## 6. Navigate to Standalone Tests
     cd /home/abhishek/Desktop/vsdsquadron-soc/caravel_mgmt_soc_litex/verilog/dv/tests-standalone

	 set GCC_PATH=/usr/bin
	 echo 'export GCC_PATH=/usr/bin' >> ~/.bashrc

## 7. Run SPI Master Test (Task-1)
     cd spi_master
  
### Clean previous build files:
     make clean
    
### Run the simulation:
     make

## 8. Simulation Output

### Monitor: Test SPI Master (RTL) Passed

<img width="1077" height="147" alt="spi masted passed" src="https://github.com/user-attachments/assets/7bf103fe-4514-4d2e-8fd9-0dfcfc690cc6" />

## Generated Files (SPI Master Test)

### After running the SPI Master standalone verification test, the following output files were generated in the test directory:

#### File Name                                           Description

- spi_master.hex                  Memory initialization file generated from the compiled firmware and loaded into the simulated memory.
- spi_master.lst                  Disassembled firmware instructions generated using objdump.
- RTL-spi_master.vcd              Waveform dump file generated during simulation for signal analysis.


## These files confirm that the firmware compilation and RTL simulation were executed successfully as part of the standalone verification flow.

## To veiw waveform of the output install gtkwave
      sudo apt install gtkwave
  
## after installaton completed successfully veiw output with following command :
      gtkwave RTL-spi_master.vcd spi_master.gtkw

<img width="1212" height="198" alt="gtkwave spi_master" src="https://github.com/user-attachments/assets/ba08dd26-c460-4e89-beb2-ea91c7167f4d" />

<img width="1280" height="800" alt="spi master o:p wave" src="https://github.com/user-attachments/assets/fdfc0bd2-e78f-4dd3-b2a5-c3fb0c967f94" />

## Task 2 — Understand the Verification Flow
### The standalone verification tests are controlled by a Makefile-based simulation flow.
### When the make command is executed, the Makefile automatically performs a sequence of steps to compile the firmware, prepare the simulation environment, run the simulator, and determine the PASS/FAIL result.

### The overall verification flow is described below.

## Step 1 — Firmware Compilation

### The firmware program (spi_master.c) is compiled using the RISC-V cross compiler.
### This step generates the compiled firmware executable:
    - spi_master.elf
- The .elf file contains the machine instructions that will run on the simulated processor.
### This spi_master.elf cannot seen in lists because the Makefile deletes it automatically at the end of the run.

## Step 2 — ELF to HEX Conversion

### The compiled ELF file is converted into a memory initialization file.
### The .hex file is used to load the firmware into the simulated memory during the simulation process.

## Step 3 — Verilog Compilation
### The RTL design files and the verification testbench are compiled using the Icarus Verilog simulator.

### This step compiles all Verilog source files and generates the simulation executable:

    spi_master.vvp
### Makefile automatically deletes intermediate files after simulation, including:
    
	spi_master.elf
    spi_master.vvp


## Step 4 — Simulation Execution

### The compiled simulation is executed using the Verilog runtime simulator:

    vvp spi_master.vvp

## During this stage:

- •	The firmware (spi_master.hex) is loaded into memory.
- •	The testbench starts the simulation.
- •	The processor executes the firmware instructions.
- •	Hardware signals are simulated cycle-by-cycle.

### A waveform file is also generated:
   
	RTL-spi_master.vcd

## Step 5 — PASS / FAIL Detection

### The testbench monitors specific debug registers and conditions to determine whether the test passed or failed.

### PASS output Example:

    Monitor: Test SPI Master (RTL) Passed

### FAIL output Example:

    Monitor: Timeout, Test SPI Master (RTL) Failed


## Verification Flow Summary (SPI Master Standalone Test)

### The standalone verification flow is controlled by a Makefile-based build system.
### When the make command is executed, several steps are performed sequentially to compile firmware, prepare the simulation environment, run the simulator, and determine the verification result.

## Overall Verification Flow

### The complete verification flow can be summarized as:

    make
      ↓
    Firmware Compilation (spi_master.c → spi_master.elf)         [Temporary File]
      ↓
    ELF to HEX Conversion (spi_master.elf → spi_master.hex)      [Permanent File]
      ↓
    Verilog Compilation (iverilog → spi_master.vvp)              [Temporary File]
      ↓
    Simulation Execution (vvp spi_master.vvp)
      ↓
    Waveform Generation (RTL-spi_master.vcd)                     [Permanent File]
      ↓
    Disassembly Generation (spi_master.lst)                      [Permanent File]
      ↓
    Cleanup of Temporary Files (spi_master.elf, spi_master.vvp removed)

	
## Importance of Standalone Block Verification for Physical Design Engineers

### In modern System-on-Chip (SoC) development, the design process follows several stages starting from RTL design to final silicon fabrication. One of the most important early steps in this process is functional verification of the RTL design. The standalone verification tests provided in the VSDSquadron SoC repository are intended to validate the functionality of individual hardware blocks before the design moves to the physical implementation stage.

### Although Physical Design engineers mainly work on the implementation of the RTL into silicon layout, it is still essential for them to understand the functional verification flow. This ensures that the design they are implementing physically is functionally correct and ready for synthesis and layout.


## Role of Verification Before Physical Design

### In the ASIC design flow, the general sequence of stages is:

    RTL Design → Functional Verification → Synthesis → Floorplanning → Placement → Clock Tree Synthesis → Routing → Signoff → Tapeout


</details>

<details>
<summary><strong>PHASE-2 - Running All Standalone Tests </strong></summary>

## In Phase-2, the objective is to execute verification tests for all hardware blocks present in the tests-standalone directory. Each block is tested independently using the same Makefile-driven verification flow used for the SPI Master test in Phase-1.

## 1. Navigate to Standalone Test Directory

#### First, navigate to the directory containing all standalone tests.

    cd /home/abhishek/Desktop/vsdsquadron-soc/caravel_mgmt_soc_litex/verilog/dv/tests-standalone

### List all the available tests:
    ls

#### Output:
    debug
    gpio_mgmt
    irq
    mem
    spi_master
    timer
    uart

## 2. Running Individual Block Tests

### For each block, the following steps were executed:

- 1.	Navigate to the block directory
- 2.	Clean previous build files
- 3.	Run the simulation using make
 
### commands used:
   
	make clean
    make 
	
#### The simulation output prints whether the test passes or fails.

#### In PHASE-1 we already performed the test for spi_master in this phase we test the remaining standalone tests modules 

    debug
    gpio_mgmt
    irq
    mem
    timer
    uart

#### in the directory of tests-standalone 

    cd /home/abhishek/Desktop/vsdsquadron-soc/caravel_mgmt_soc_litex/verilog/dv/tests-standalone

#### go to the indivisual standalone tests module and very wheather the tests fails/pass 
     set GCC_PATH=/usr/bin
	 echo 'export GCC_PATH=/usr/bin' >> ~/.bashrc

### cd gpio_mgmt
        make clean
        make
#### Result:
    Monitor: Test Mgmt GPIO (RTL) Passed
	
<img width="1102" height="145" alt="gpio passed" src="https://github.com/user-attachments/assets/a8378a24-ad86-4544-97ea-ed6fd1d8d76c" />

#### waveform 
    gtkwave RTL-gpio_mgmt.vcd gpio_mgmt.gtkw
	
<img width="1213" height="756" alt="gpio mgmt wave" src="https://github.com/user-attachments/assets/528a093c-fc89-4594-bc9a-804624c23a40" />


### cd ../mem
   
	make clean
    make

#### Result: 
     Monitor: Test MEM (RTL) [byte w word r] passed
	 
<img width="1040" height="146" alt="mem passed" src="https://github.com/user-attachments/assets/ef17762a-de98-4c2e-a8da-af694300da19" />

#### waveform 

	   gtkwave RTL-mem.vcd mem.gtkw
   
<img width="1214" height="756" alt="mem waveform" src="https://github.com/user-attachments/assets/fd1d09f9-51f5-4785-bc19-26cee77ee643" />


### cd ../uart

    make clean
    make

#### Result:
    Monitor: Test UART (RTL) passed
	
<img width="1052" height="148" alt="uart passed" src="https://github.com/user-attachments/assets/21e99c81-0063-4c09-a3cc-acb66845ead3" />

#### waveform 

      gtkwave RTL-uart.vcd uart.gtkw

<img width="1217" height="755" alt="uart waveform" src="https://github.com/user-attachments/assets/f14e6b05-c5df-45c1-b3ac-ed9d2778ea2b" />

### cd ../timer

    make clean
    make

#### Result:
    
	Monitor: Timeout, Test GPIO (RTL) Failed

<img width="1048" height="181" alt="timer failed" src="https://github.com/user-attachments/assets/6fe6703c-c986-4800-994c-b105881100c9" />


### cd ../irq

	make clean
	make
#### Result:
    Monitor: Timeout, Test IRQ (RTL) Failed  
	
<img width="1030" height="188" alt="irq failed" src="https://github.com/user-attachments/assets/3d6a6cc1-7128-423b-9460-c27f9156c3c3" />


### cd ../debug
   
	make clean
    make

#### Result:

Monitor: Timeout, Test Debug (RTL) Failed

<img width="1033" height="174" alt="debug failed " src="https://github.com/user-attachments/assets/34016d37-fe4e-4941-b950-e63ebf32bd6f" />

#### waveform 

	 gtkwave RTL-debug.vcd debug.gtkw

<img width="1215" height="750" alt="debug waveform" src="https://github.com/user-attachments/assets/023533d9-1b7e-40e3-ba4d-dd6822b11a1f" />



## Reasons for Tests PASS
- Firmware executed correctly and interacted properly with the hardware block.
- RTL hardware module responded correctly to control signals.
- Testbench detected the expected behavior of the module.
- Simulation finished within the allowed time without timeout.

### Tests passed modules successfully : spi_master, gpio_mgmt, mem, uart


## Reasons for Tests FAIL
- Simulation timeout occurred because the expected event did not happen.
- Firmware–hardware interaction did not complete properly.
- Processor configuration mismatch for some modules.
- Standalone environment limitations, where some blocks require full system integration.
	
### Tests failed modules : timer, irq, debug


## 

+------------+--------+
| tests-standalone |status (sky130)|
+------------+--------+
| spi_master | PASS   |
| gpio_mgmt  | PASS   |
| mem        | PASS   |
| uart       | PASS   |
| timer      | FAIL   |
| irq        | FAIL   |
| debug      | FAIL   |
+------------+--------+



</details>

<details>
<summary><strong>PHASE-3 - Caravel Integrated Tests </strong></summary>


## Objective

### The objective of Phase-3 is to verify the functionality of VSDSquadron SoC blocks within the Caravel integrated environment. In this phase, the hardware modules are tested together with the Caravel management system to ensure proper communication and system-level functionality before full SoC implementation.

### 1. Navigate to the Caravel test directory

    cd ~/Desktop/vsdsquadron-soc/caravel_mgmt_soc_litex/verilog/dv/tests-caravel

#### Check available test folders:

 <img width="1102" height="152" alt="cd ls caravel tests" src="https://github.com/user-attachments/assets/8f8d6fe4-f193-40eb-b6d2-c6d6a932fce5" />

 #### The actual caravel tests should be run 
    user_pass_thru
    uart
    sysctrl
    sram_exec
    spi_master
    pullupdown
    pll
	pass_thru_fix
	pass_thru
	mem
	hkspi_power
	gpio_mgmt
	hkspi

	




