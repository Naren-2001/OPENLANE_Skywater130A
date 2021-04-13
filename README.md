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




![image](https://user-images.githubusercontent.com/82043108/114495113-299b9e00-9c3b-11eb-8bdd-e77dea69237b.png)



 ![image](https://user-images.githubusercontent.com/82043108/114495177-446e1280-9c3b-11eb-87d4-0a6c8fa1ca82.png)
 
![image](https://user-images.githubusercontent.com/82043108/114495243-5cde2d00-9c3b-11eb-9e6a-95d083296f37.png)
![image](https://user-images.githubusercontent.com/82043108/114495269-67002b80-9c3b-11eb-9ddc-58d3b7b8f8fe.png)
![image](https://user-images.githubusercontent.com/82043108/114495274-6bc4df80-9c3b-11eb-8ff6-cb3254d6d83f.png)
