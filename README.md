# OPENLANE_Skywater130A
![image](https://user-images.githubusercontent.com/82043108/114485796-2ba93100-9c2a-11eb-9924-f4589250b76a.png)

WHAT IS SKYWATER PDK??

SKY130 PDK is used to design and manufacture chips that are successfully designed commercially in large quantities.it is not used for production at the current time.it is used to 
test and verify chips that are commercially designed and it is a collabration between google and skywater foundry for making it as a opensource.it is a 130 nanometer process node
and they are planning to release various process nodes in future

What is Openlane?

OPENLANE is a RTL to GDSII flow that contains yosys,magic,fault,netgen,Openroad,SPEF_extractor,Klayout for designing packages and verifying chip..

**DAY 1: Inception of open-source EDA, OpenLANE and Sky130 PDK**

First day of workshop starts with Basics of EDA tools,introduction to openlane and Skywater pdks.
Entire flow of the physical design is defined
![image](https://user-images.githubusercontent.com/82043108/114495630-07565000-9c3c-11eb-9319-4f4be2c22e73.png)
The inputs to the design flow are

* Design files 
* PDK files 

1. Design files 
 * Design rule check(DRC)
 * Layout versus synthesis check (LVS)
2. PDK files
 * .lef files
 * .tlef files
 * Libraries of standard cells

Output of the flow is GDSII file (.gds)

.gds file is given to foundry for manufacturing the chip

Stages of Openlane Flow
* Synthesis
  * yosys-Performs RTL synthesis using GTECH mapping
  * abc-maps the cells to standard cells described in PDK(Skywater130) and produce netlsit
  * OpenSTA-performs timing analysis on the resulting netlist
  * Fault-Design for test
  
* Floorplan
  * init_fp-Defines the core area for macros and rows for placement and tracks for routing
  * IOplacer-places the macro and output ports
  * PDN-Power Distribution network
  * tapcells-insert well taps and decap cells
  
* Placement(Global and Detailed)
  * RePlace-performs global placement
  * Resizer-performs optimizations on the design
  * OpenPhySyn-perform timing optimization
  * OpenDP-performs detailed placement
  
* Clock tree synthesis
  * TritonCTS-synthesizes clock distribution network

* Routing
  * FastRoute-performs global routing
  * TritonRoute-perfoms detailed routing
  * SPEF Extractor-perfoms SPEF extraction that include parasitics extraction
  
* GDSII
  * Magic-streams out the final GDSII layout
  
* Checks
  * Magic-performs DRC and antenna check
  * Netgen-performs LVS check(Layout versus synthesis)
  
Skywater uses six metal layer to perform CTS,PDN,Routing. 

LAB1

Openlane directories are defined.

* pdks
* OpenLANE_flow

1. pdks 

* Skywater_pdk-contains skywater pdks
* open_pdk-consists of open source tools
* sky130A-PDK variant that is made compatible to open source tools

sky130A contains 
* libs.tech-it contains the files specific to EDA tools
* libs.ref-technology parameters,timing parameters,.lef files and .tlef files

2. openLANE_flow has the files shown below
   
   To start the flow we use the command 
   
   ./flow.tcl -interactive (for interactive flow)
   
   To include the packages of openlane 
   
   package require openlane 0.9 
   
   To include the design
   
   prep -design picorv32a 
   
   
![image](https://user-images.githubusercontent.com/82043108/114495113-299b9e00-9c3b-11eb-8bdd-e77dea69237b.png)

So for this workshop.we used picorv32a design for synthesis
![image](https://user-images.githubusercontent.com/82043108/114495177-446e1280-9c3b-11eb-87d4-0a6c8fa1ca82.png)
 
 picorv32a contains
 * src-contains HDL files(.v files)
 * sky130A_sky130_fd_sc_hd_config.tcl(pdk specific configuration)
 * config.tcl(custom configurations)

Priority is given to sky130A_sky130_fd_sc_hd_config.tcl and then config.tcl

![image](https://user-images.githubusercontent.com/82043108/114495243-5cde2d00-9c3b-11eb-9e6a-95d083296f37.png)

.tlef files and .lef files are merged and it is named as merged.lef
merged.lef is copied to the following path

/openLANE_flow/designs/picorev32a/runs/07-04_10-55/tmp

change can be done on the fly in this openlane flow which make it more flexible to use

synthesis is done by the command

run_synthesis

So this command performs wire synthesis,abc mapping and static timing analysis

After synthesis,Slack of 1.27 is obtained.

![image](https://user-images.githubusercontent.com/82043108/114495269-67002b80-9c3b-11eb-9ddc-58d3b7b8f8fe.png)
![image](https://user-images.githubusercontent.com/82043108/114495274-6bc4df80-9c3b-11eb-8ff6-cb3254d6d83f.png)

**DAY 2 : Good floorplan vs bad floorplan and introduction to library cells**

Floorplan stage performs the following process
* Die area
* Core utilization
* aspect ratio
* Input and Output cell
* power distribution network
* Macroplacememt

Floorplan is performed using the command

run_floorplan

OpenLANE_flow directory contains "configuration" which contains variables required for each stage

Examples:
    * FP_CORE_UTIL gives the ratio of area occupied by netlist to the total are of core
    * FP_ASPECT_RATIO gives the aspect ratio (Height/Width) 

![image](https://user-images.githubusercontent.com/82043108/114551653-4fe42c80-9c81-11eb-94af-15f8d92cd9b0.png)
 
floorplan.tcl contains the default parameters used by openlane for floorplan

![image](https://user-images.githubusercontent.com/82043108/114551341-e3692d80-9c80-11eb-936b-1927067c3faf.png)

After floorplan .def file is created in the path /results/floorplan

Inside the def file,the core area,die area and other parameters related to floorplan can be viewed
![image](https://user-images.githubusercontent.com/82043108/114551870-946fc800-9c81-11eb-9941-05eadb894ed3.png)


![image](https://user-images.githubusercontent.com/82043108/114551591-40fd7a00-9c81-11eb-8d0c-d667272586fe.png)

![image](https://user-images.githubusercontent.com/82043108/114551707-65595680-9c81-11eb-8139-bc585eca0ac8.png)

magic tool is opened 

![image](https://user-images.githubusercontent.com/82043108/114552069-cbde7480-9c81-11eb-8a6e-10d3acb330f7.png)

The side tab defines the different layers

green tick in drc checck box defines that there is no error

capacitor cells and tapcells are viewed

To select a layer move the cursor over the layer and press s

To make the layout fit to screen press s and v

Next step is placement
1. global(guide for detailed placement)
2. detailed (legalization)

Legalization tells whether the standard cells are overlaped or not

Placement is performed using the command

run_placement

It uses HPWL-half perimeter wire length to perform placement
during the placement process the OVFL(overflow) must decrease


    
