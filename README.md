# Beginner-SoC-Physical-Design-Warkshop
This repository contains all the information included in the beginner SoC/physical design using open- source EDA tools conducted by Kunal Ghosh - [(VLSI System Design)](https://www.vlsisystemdesign.com/). This workshop helped me gain hands-on experience of tools that are used in physical design flow.
<br/>

# Table of Contents
1. **Day-1**
    * IC Design Components and Terminologies
    * RISC-V
    * Physical Design Flow and Open source EDA tools
    * Lab Exercises 
2.**Day-2**
    * Floorplan
    * Placement
    * Cell Design Flow
    * Lab Exercises 
3. **Day-3**
    * ngspice Simulation
    * Euler's Path and Stick Diagram
    * Post-Layout extraction and Simulation
    * CMOS Fabrication Process
    * Lab Exercises
4. **Day-4**
    * Timing modelling using delay tables
    * Clock Tree synthesis
    * Timing analysis with real and ideal clocks
    * Lab Exercises
5. **Day-5**
    * Routing
    * DRC
    * Parasitic extraction and the SPEF Format
    * Lab Exercises
   
<br/>

# Day-1
# IC Design Components and Terminologies
**IC** or **Integrated circuit** is basically an electronic circuit consisting of large number of transistors, resistors and capacitors etc inside a single semiconductor chip. They come in a variety of packages and sizes. Some of the commonly used ICs include Timer 555 ic, 741 operational amplifiers, 78xx series of voltage regulators and 74xx series of logic gates.<br/>
**SOC - System on Chip** is also a type of IC which has the capabilities of a computer built inside the chip. It typically consists of a CPU, memory , input and output ports, analog IPs and other units integrated within itself.SoCs can be applied to any computing task. They are typically used in mobile computing such as tablets, smartphones, smartwatches and netbooks as well as embedded systems.<br/>
**Core** is the section of the chip where the fundamental logic of the design is placed. **Die**   contains the core, it is the small semiconductor material specimen on which the fundamental circuit is fabricated. IC's are fabricated on a single 9 inch or 12 inch diameter silicon wafer, which contains hundreds of mirror images of the fundamental logic. This wafer is then cut into small pieces, each piece has similar functionality of the fundamental logic called Die. **Pad cells** surround the rectangular metal patches where external bonds are made. input,output and power pad cells are commonly used pad cells.
<p align="center">
<img src="https://github.com/BidyenduGhoshal/Beginner-SoC-Physical-Design-Warkshop/blob/main/Image/Day%201/TH1.png" 
alt="alt text" width = 553 height = 358  >
<p/>

<p align="center">
<img src="https://github.com/BidyenduGhoshal/Beginner-SoC-Physical-Design-Warkshop/blob/main/Image/Day%201/TH1_2.png" 
alt="alt text" width = 553 height = 358  >
<p/>

## Package Types ##
Integrated circuits are put into protective packages to allow easy handling and assembly onto printed circuit boards and to protect the devices from damage. A very large number of different types of package exist. Through-hole packages, Surface mount, Chip carrier, Pin grid arrays, Flat packages and ball grid array are some of the most common packages. In this workshop we deal with a variant of Flat package called QFN-48. ( Quad Flat No leads with 48 pins)
<p align="center">
<img src="https://github.com/BidyenduGhoshal/Beginner-SoC-Physical-Design-Warkshop/blob/main/Image/Day%201/TH2.png" 
alt="alt text"  >
<p/>

<p align="center">
<img src="https://github.com/BidyenduGhoshal/Beginner-SoC-Physical-Design-Warkshop/blob/main/Image/Day%201/TH2_2.png" 
alt="alt text" width = 553 height = 358  >
<p/>

## Foundry IPs and Macros
PLLs, DAC, ADC and SRAMs come under the category of foundry IP. They have to be manually designed or need some human interference (or intelligence) essentially to define and create them. IPs(Intellectual property) are targeted on a particular technology nodes. Macros are basically digital blocks which are made up of purely digital logic.

# RISC-V
The RISC V is an open source specification of an Instruction Set Architecture (ISA).x86 is commonly used ISA for personal computers and ARM architecture is commonly used for Mobile Phones .But these are proprietary standards Unlike most other ISA designs, the RISC-V ISA is provided under open source licenses that do not require fees to use, which provides it a huge edge over other commercially available ISAs. It is a simple, stable, small standard base ISA with extensible ISA support, that has been redefining the flexibility, scalability, extensibility, and modularity of chip designs. 

## Assembly language - Bridge between software and hardware
Applications and softwares running in our laptops and PCs are written in a variety of languages like C, C++, Python and Java etc. These different languages cannot be directly processed and implemented by the hardware (Microprocessor Core). They need to be converted into form that is understandable by the core. This is where the ISA or the instruction set architecture (assembly language) comes into the picture. Special programs called the compiler and assembler are used to convert the instructions in different languages to the target assembly language. The compiler converts the code into assembly language, which in our case is the RISC-V ISA and the assembler takes care of converting the different instructions in textual format into binary representations which are understood by the microprocessor and executed.

## PicoRV32 and PicoSOC (FPGA Flavour)
with the advent of an open standard ISA like RISC-V, there are many people who have created their own microprocessor cores which are documented in the RISC-V github handle found at [RISC-V Cores List](https://github.com/riscv/riscv-cores-list). PicoRV32 is one such implementation of the RISC-V ISA created by clifford wolf. [PicoRV32](https://github.com/cliffordwolf/picorv32) is a CPU core that implements the RISC-V RV32IMC Instruction Set. It can be configured as RV32E, RV32I, RV32IC, RV32IM, or RV32IMC core, and optionally contains a built-in interrupt controller. [PicoSOC](https://github.com/cliffordwolf/picorv32/blob/master/picosoc/picosoc.v) is an example SOC created using PicoRV32. PicoSOC is a type of SOC flavour which is more suited towards **FPGA** implementation and contains a less-detailed description of memory and other Components since these are resources already available inside the FPGA.
<p align="center">
<img src="https://github.com/BidyenduGhoshal/Beginner-SoC-Physical-Design-Warkshop/blob/main/Image/Day%201/TH3.png" 
alt="alt text" width = 516 height = 452 >
<p/>

## Raven (ASIC Flavour)
It is an ASIC implementation of the PicoSoC-PicoRV32. The core was previously proven with an FPGA implementation and Raven is the first SoC built with it.The system integrator is Tim Edwards, another champion in the open source domain. Raven includes analog IPs like PLLs (Clock Multipliers), ADCs, DACs comparators and some other blocks as well.[Efabless](https://efabless.com/) has successfully bench-tested the Raven at 100MHz, and based on simulations the design should be able to operate at up to 150MHz.  
<p align="center">
<img src="https://github.com/BidyenduGhoshal/Beginner-SoC-Physical-Design-Warkshop/blob/main/Image/Day%201/TH4.png" 
alt="alt text"  width = 450 height = 338 >
<p/>


# Physical Design Flow and Open source EDA tools
A typical back-end flow of chip design is explained in the below section. For each step, different open-source tools were used. **Qflow** is a complete tool chain for synthesizing digital circuits starting from verilog source and ending in physical layout for a specific target fabrication process.The tool chain is basically a collection of steps ordered in a particular sequence that makes it easier to analyse the design as it propagates through the steps. The steps are ordered in a sequential-order i.e, they can only be executed one after the other from top to bottom.

## Logic Synthesis
Logic synthesis is the process of translating and mapping RTL code written in HDL (such as Verilog or VHDL) into technology specific gate level representation. [Yosys](http://www.clifford.at/yosys/) is a framework for Verilog RTL synthesis. It currently has extensive Verilog-2005 support and provides a basic set of synthesis algorithms for various application domains.

## Floorplanning
The second step in the physical design flow is floorplanning. Floorplanning is the process of identifying structures that should be placed close together, and allocating space for them in such a manner as to meet the sometimes conflicting goals of available space (cost of the chip), required performance, and the desire to have everything close to everything else.Floor planning is done using an opensource tool called [graywolf](https://github.com/rubund/graywolf) as a part of Qflow.
## Placement
 Placement is the process of automatically assigning correct positions to predesigned cells on the chip with no overlapping such that some objective function is optimized. Placement is design state after Floorplanning and before routing. Placement also is done using [graywolf](https://github.com/rubund/graywolf) as a part of Qflow.
## Clock Tree Synthesis
Clock Tree Synthesis is a process which makes sure that the clock gets distributed evenly to all sequential elements in a design.
The goal of CTS is to minimize the skew and latency. Clock Tree Synthesis also is done using [graywolf](https://github.com/rubund/graywolf) as a part of Qflow.
## Routing
Routing is the next stage after clock tree synthesis which involves making connections as per the netlist generated earlier. This includes interconnections between standard cells, macros and the Pads present in the chip boundary. Routing is done with the help of an opensource tool called [Qrouter](http://opencircuitdesign.com/qrouter/).
## Static Timing Analysis
Static Timing Analysis (STA) is one of the techniques to verify design in terms of timing.The STA will validate whether the design could operate at the rated clock frequency, without any timing violations. Some of the basic timing violations are setup violation and hold violation. STA is done using the help of [Opentimer](https://github.com/OpenTimer/OpenTimer).

## Layout Viewer
[Magic](http://opencircuitdesign.com/magic/) is the open source tool used for viewing and creating physical layouts for circuits after SPICE simulation. After the completion of the layout, The tool also has a facility of extracting parasitics and other delays in SPICE format which can further be simulated using spice tools.
## Pre-Layout and Post-Layout SPICE simulation
[ngspice](http://ngspice.sourceforge.net/) the open source spice simulator for electric and electronic circuits. It is used to create a netlist, view waveforms and calculate a rough estimate of the timing parameters. It is used both in the pre-layout and the post-layout stages for running SPICE simulations.post-layout spice simulation is done after extracting the post layout spice file using magic layout viewer.

## Installation Guide
The details of installation are well documented in the README file of https://github.com/kunalg123/vsdflow. One can install all the tools easily by following all the steps given there instead of going through the hassles of installing each tool individually.</br>
List of tools installed using vsdflow
|Tool Name | Function |
|---|---|
|Yosys | RTL Synthesis|
|blifFanout | High fanout net (HFN) synthesis|
|graywolf | Placement|
|qrouter | Detailed routing|
|magic  | VLSI Layout tool|
|netgen  | LVS|
|OpenTimer and OpenSTA | Static timing analysis tool|

 For installing **ngspice** for windows or Mac OS, it is already available as a precompiled version [here](http://ngspice.sourceforge.net/download.html). For installing ngspice in linux environments, [Synaptic package manager](https://geek-university.com/linux/synaptic/) needs to be installed first. ngspice can then be installed using the package manager.

## Lab Exercises
### Exp 1:
<br/>
NOTE - If you are already inside Lab, then no need to do below steps

Click on VSD IAT, Go to "Lab Instances". Then under "Links", click on the "link" icon. Click bottom left, System tools > LXTerminal.

Now type the command "yosys". What do you see next?
<br/>
<p align="center">
<img src="https://github.com/BidyenduGhoshal/Beginner-SoC-Physical-Design-Warkshop/blob/main/Image/Day%201/Lab%201.png" 
alt="alt text"  >
<p/>
<br/>
### Exp 2:
<br/>
NOTE - If you are already inside Lab, then no need to do below steps

Click on VSD IAT, Go to "Lab Instances". Then under "Links", click on the "link" icon. Click bottom left, System tools > LXTerminal.

Type "which sta" and what path do you see the sta tool to be linked to?
<br/>
<p align="center">
<img src="https://github.com/BidyenduGhoshal/Beginner-SoC-Physical-Design-Warkshop/blob/main/Image/Day%201/Lab%202.png" 
alt="alt text"  >
<p/>
<br/>
### Exp 3:
<br/>
NOTE - If you are already inside Lab, then no need to do below steps

Click on VSD IAT, Go to "Lab Instances". Then under "Links", click on the "link" icon. Click bottom left, System tools > LXTerminal.

Type below command from current working directory

git clone https://github.com/kunalg123/vsdflow.git
What is the last line you see ?
<br/>
<p align="center">
<img src="https://github.com/BidyenduGhoshal/Beginner-SoC-Physical-Design-Warkshop/blob/main/Image/Day%201/Lab%203.png" 
alt="alt text"  >
<p/>
<br/>
### Exp 4:
<br/>
NOTE - If you are already inside Lab, then no need to do below steps

Click on VSD IAT, Go to "Lab Instances". Then under "Links", click on the "link" icon. Click bottom left, System tools > LXTerminal.

Type below commands:

cd vsdflow
./vsdflow spi_slave_design_details.csv
ls -ltr outdir_spi_slave/
Here you will see lot of files created.

Now type below command

ls -ltr outdir_spi_slave | wc
This will give you 3 numbers in one row, where 1st number represents number of files

What are the number of files you see?

Hint - The keyword "total 320 which is the first line you see when you type "ls -ltr outdir_spi_slave" is NOT a file
<br/>
<p align="center">
<img src="https://github.com/BidyenduGhoshal/Beginner-SoC-Physical-Design-Warkshop/blob/main/Image/Day%201/Lab%204.png" 
alt="alt text"  >
<p/>
<br/>
### Exp 5:
<br/>
NOTE - If you are already inside Lab, then no need to do below steps

Click on VSD IAT, Go to "Lab Instances". Then under "Links", click on the "link" icon. Click bottom left, System tools > LXTerminal.

Type below commands

cd outdir_spi_slave
qflow display spi_slave
It will open 2 windows "layout1" and "tkcon"

On "tkcon" window, type "box".

What is the output area in microns?
<br/>
<p align="center">
<img src="https://github.com/BidyenduGhoshal/Beginner-SoC-Physical-Design-Warkshop/blob/main/Image/Day%201/Lab%205.png" 
alt="alt text"  >
<p/>
<br/>
### Exp 6:
<br/>
NOTE - If you are already inside Lab, then no need to do below steps

Click on VSD IAT, Go to "Lab Instances". Then under "Links", click on the "link" icon. Click bottom left, System tools > LXTerminal.

Type below command

cd
cd vsdflow
mkdir my_picorv32
cd my_picorv32
mkdir source synthesis layout
cp ~/vsdflow/verilog/picorv32.v source/.
qflow gui &
Select below options in gui

Technology = osu018
Verilog source file : picorv32.v
Verilog module : picorv32
Click on Set Stop

Then run labs till synthesis as shown in Labs Video

What is the % ratio of flipflop/total logic ?
<br/>
<p align="center">
<img src="https://github.com/BidyenduGhoshal/Beginner-SoC-Physical-Design-Warkshop/blob/main/Image/Day%201/Lab%206.png" 
alt="alt text"  >
<p/>
<br/>

# Day 2 #

# Floorplan

## Defining the width and height of the core
Consider a netlist consisting of D Flip flops and logic gates. While representing the circuit as a logic diagram, different elements take different shape based on the logic functionality of the element. But in reality, all the logic gates and flipflops are rectangular or square in shape when realised as a physical layout. each of these rectangular or square blocks have a predefined area based on the target technology, in our case it is the osu018 library. The sum of all the areas of these blocks determine the dimensions (width x height) of the chip. <br/>

## Utilization Factor 
Utilization factor is a measure of the area occupied by the standard cells, macros and other blocks with respect to the total area of the ship. it is synonymous with packing density. Utilization factor is given by the formula (Area occupied by netlist)/(Total area of the core).100% Utilization factor indicates that there is no empty space and the netlist completely fills the core. A utilization factor of 50% means that half of the core is empty and the other half is occupied by the netlist elements

## Floor Planning and Pre-Placed Cells
Consider a large logic block which has some blocks which are redundant. this large block can be separated into smaller blocks and this is an example of pre-placed cells. Memory, Clock-gating cell, Comparator and Mux are some of the other preplaced cells. The arrangement of these IP's in a chip is referred to as floorplanning. These IP’s/blocks have user-defined locations, and hence are placed in chip before automated placement-and-routing and are called as pre-placed cells. Automated placement and routing tools places the remaining logical cells in the design onto chip

## Noise Margins
Consider a Complex circuit powered from the source Vdd and GND. During switching operation, the circuit demands switching  current i.e. peak current. Now, due to the presence of Rdd and Ldd, there will be a voltage drop across them and the voltage at Node 'A' would be Vdd' instead of Vdd. If Vdd' goes below the noise margin, due to Rdd and Ldd, the logic '1' at the output of circuit wont be detected as logic '1' at the input of the circuit following this circuit. These are referred to as noise and it is expected that the noise levels be within certain margins, otherwise the output node would go into an undefined state. There are two different noise margins commonly defined.NMl (NOISE MARGIN low) = Vil – Vol. NMh (NOISE MARGIN high) = Voh – Vih. Noise margin is the amount of noise that a CMOS circuit could withstand without compromising the operation of circuit.

## Decoupling Capacitors
To overcome the problem of Noise margins, pre-placed cells are surrounded by Decoupling capacitors. The decoupling capacitor acts as a charge storage bank and satisfies the current and voltage levels expected at the input terminals of the logic cells. Thus, the problem of voltage drop and noise levels affecting the logic levels are eliminated through the use of Decoupling Capacitors. 

## Power Planning
Consider a scenario where a driver block drives multiple signals as inputs to a load block. Say all the inputs are switching and there are a lot of nodes which are discharging from Vdd to 0 and charging from 0 to Vdd at the same time. if too many nodes charge from 0 to Vdd using the same source, then Voltage droop phenomenon occurs in which the Vdd dips to a lower value. Similarly, if too many nodes charge from Vdd to 0 using the same source, then Ground bounce phenomenon occurs in which the gnd node moves to a higher value. To avoid this issue, Vdd and gnd(Vss) metal layers form a grid like pattern over the core. Now each node would charge or discharge through different sources and the discrepancies are avoided.

## Pin Placement
Pins are basically contacts grown between the core and die borders. Clk pins have wider contacts compared to other input/output pins to decrease the resistance in it's path, since Clk is routed throughout the core.

# Placement

## Library 
The next step involved is binding the netlist with physical cells. A library is a collection of standard cells obtained from the foundry for a particular technology node. Each element of the netlist is mapped to a cell in the library. library consists of cells of the different functionalites like AND , XOR and D FlipFlops etc. In addition to this, it also contains different sizes of the same functional gates - representing higher drive strengths.

## Placement of the library cells
now the binded physical cells need to be placed inside the core. the placement is determined by the position of the input pins and the frequency of operation of the block as well. Consider a netlist operating at a very high frequency, naturally to preserve the high frequency of operation the cells must be placed very close to each other.other netlists can have regular gaps between them to avoid timing violations. if the distance between the Input/Output pins to the input or output of the netlist is more, then to preserve the signal integrity - buffers/repeaters can be added at even spacings along the path of the netlist. 


# Cell Design Flow
As seen earlier, Libraries contain standard cells which are binded to the netlist generated. The libary contains cells with different logic functions, different sizes and different threshold voltages. Apart from this, custom cells can also be created for which the process is mentioned below.

## Inputs or Pre-requisites to the Design Flow

### Process Design Kits (PDKs)
A process design kit (PDK) is a set of files used within the semiconductor industry to model a fabrication process for the design tools used to design an integrated circuit. The PDK is created by the foundry defining a certain technology variation for their processes. It is then passed to their customers to use in the design process.

### Design rule checking (DRC)
Design rules are a series of parameters provided by semiconductor manufacturers that enable the designer to verify the correctness of a mask set. Design rules are specific to a particular semiconductor manufacturing process. A design rule set specifies certain geometric and connectivity restrictions to ensure sufficient margins to account for variability in semiconductor manufacturing processes, so as to ensure that most of the parts work correctly.

### Layout Versus Schematic (LVS)
A successful design rule check (DRC) ensures that the layout conforms to the rules designed/required for faultless fabrication. However, it does not guarantee if it really represents the circuit you desire to fabricate. This is where an LVS check is used. LVS checking software recognizes the drawn shapes of the layout that represent the electrical components of the circuit, as well as the connections between them. This netlist is compared by the "LVS" software against a similar schematic or circuit diagram's netlist.

### SPICE Models
A SPICE model is a text-description of a circuit component used by the SPICE Simulator to mathematically predict the behavior of that part under varying conditions. The foundry has to provide this model which characterises the performance of NMOS and PMOS devices. behavior refers to the current and voltage equations that accurately model the working of the device. Using SPICE Models is the industry standard way to simulate circuit performance prior to the prototype stage 

### Library and User-Defined specs
Library and User-Defined Specifications include parameters like Cell-height, Supply-voltage, Metal-layers, Pin locations and the gate length etc.

## Design Steps

### Circuit Design
The circuit is first designed as a spice netlist in ngspice and the different waveforms are plotted. Using the information from the waveforms, we can tweak our design paramaters such as the transistor width etc, to obtain the required timing parameters. once the spice netlist is done , the next step involved is designing the layout for the netlist.
### Layout Design
The layout design is done through Magic layout tool , and it is designed in such a way that the DRC rules are satisfied. Before layout is designed, an intermediate step called stick diagram could help to visualise the rough layout. **stick diagrams** are a means of capturing topography and layer information using simple diagrams. Stick diagrams convey layer information through colour codes (or monochrome encoding). Acts as an interface between symbolic circuit and the actual layout. stick diagrams can be drawn in any order (the order of the inputs), but designing using **Euler's path** results in less congestion and a neat layout. An Euler path, in a graph or multigraph, is a walk through the graph which uses every edge exactly once. The order of the inputs obtained from the Euler's Path is then used to design the layout. 
### Cell Characterization
Cell characterization is a process of analyzing a circuit using static and dynamic methods to generate models suitable for chip implementation flows. Cell characterization typically takes cell design extracted as spice circuit and spice technology models. Characterization software like guna from Paripath, analyzes this information to 
1. acquire or recognize cell’s function,
2. generates stimulus appropriate to determine characteristic (like delay, transition time etc),
3. simulates it using circuit simulator,
4. gather simulations output to measure characteristic and
5. finally writes this data into a standard like libertyTM, veriog or IBIS.
To conclude Cell characterization, Softwares like GUNA take the spice model as inputs and create **timing, noise and power** .libs and functions .

## Timing Characterization
Slew low (Time for 20% of Vdd), Slew high (Time for 80% of Vdd) , Input and Output are also time characterized for both rising and falling transitions. This can be manually done by viewing the waveforms in ngspice and obtaining the difference between the coordinates in the input and output waveforms.
|Timing Threshold Definitions| Percentage of Vdd|
|--|--|
|slew_low_rise_thr|20%|
|slew_high_rise_thr|80%|
|slew_low_fall_thr|20%|
|slew_high_fall_thr|80%|
|in_rise_thr|50%|
|in_fall_thr|50%|
|out_rise_thr|50%|
|out_fall_thr|50%|
## Propagation Delay and Transition Time
Propagation Delay is a measure of time taken by the signal to pass through a gate. It is calculated with the help of parameters like in_rise_thr ,in_fall_thr, out_rise_thr and out_fall_thr. The propagation delay is basically the time difference between the input crossing 50% and the output crossing the 50% point. The propagation delay is expected to be positive and if it comes out to be negative - it means that the transistors are not properly sized or the threshold voltage setting is faulty. This is an important factor in circuit design that must be kept in consideration before designing a logic circuit in spice </br>
Transistion time is the time needed for a signal to pass from 10% to 90% or from 20% to 80% of its final state. The delay of a cell can be deduced from the standard cell library, it is a function of input transition time and output capacitance load.The input and output slew can be found using ngspice waveforms by locating the 20% and 80% points and subtracting them

## Lab Exercises

### Exp 1:
Go to Day 2 labs Your placement runs must have finished by now

Type below command

cd
cd vsdflow/my_picorv32
qflow display picorv32 &
This will open layout and tkcon window In the layout window, select whole chip using below steps

Take cursor to bottom left
Left mouse click
Take cursor to top right
Right mouse click
Press Shift+i
This will select the whole layout Now in tkcon window, type below command

box
What is the area of you chip in microns?

<br/>
<p align="center">
<img src="https://github.com/BidyenduGhoshal/Beginner-SoC-Physical-Design-Warkshop/blob/main/Image/Day%202/L1.png" 
alt="alt text"  >
<p/>
<br/>

<br/>
<p align="center">
<img src="https://github.com/BidyenduGhoshal/Beginner-SoC-Physical-Design-Warkshop/blob/main/Image/Day%202/L2.png" 
alt="alt text"  >
<p/>
<br/>

<br/>
<p align="center">
<img src="https://github.com/BidyenduGhoshal/Beginner-SoC-Physical-Design-Warkshop/blob/main/Image/Day%202/L3.png" 
alt="alt text"  >
<p/>
<br/>

<br/>
<p align="center">
<img src="https://github.com/BidyenduGhoshal/Beginner-SoC-Physical-Design-Warkshop/blob/main/Image/Day%202/L4.png" 
alt="alt text"  >
<p/>
<br/>

<br/>
<p align="center">
<img src="https://github.com/BidyenduGhoshal/Beginner-SoC-Physical-Design-Warkshop/blob/main/Image/Day%202/L5.png" 
alt="alt text"  >
<p/>
<br/>

<br/>
<p align="center">
<img src="https://github.com/BidyenduGhoshal/Beginner-SoC-Physical-Design-Warkshop/blob/main/Image/Day%202/L6.png" 
alt="alt text"  >
<p/>
<br/>

<br/>
<p align="center">
<img src="https://github.com/BidyenduGhoshal/Beginner-SoC-Physical-Design-Warkshop/blob/main/Image/Day%202/L7.png" 
alt="alt text"  >
<p/>
<br/>

<br/>
<p align="center">
<img src="https://github.com/BidyenduGhoshal/Beginner-SoC-Physical-Design-Warkshop/blob/main/Image/Day%202/L8.png" 
alt="alt text"  >
<p/>
<br/>

<br/>
<p align="center">
<img src="https://github.com/BidyenduGhoshal/Beginner-SoC-Physical-Design-Warkshop/blob/main/Image/Day%202/L9.png" 
alt="alt text"  >
<p/>
<br/>

<br/>
<p align="center">
<img src="https://github.com/BidyenduGhoshal/Beginner-SoC-Physical-Design-Warkshop/blob/main/Image/Day%202/L10.png" 
alt="alt text"  >
<p/>
<br/>


# Day 3

## ngspice Simulation
Inverter model was created as a spice deck and simulations were run on the model.ngspice was used to obtain the transfer characteristics(Input voltage vs Output Voltage) and transient analysis (Input and Output Voltage vs Time). Different delay parameters were calculated using the plots obtained from ngspice. 

## Euler's Path and Stick Diagram
As discussed earlier, Euler's Path provides an improved stick diagram and subsequently the layout. Euler's path provides the order in which the inputs are to be arranged in the final layout. A non-euler based design and euler path based design for a same logic function was performed and it proved that using Euler's path provides lesser congestion between the metal layers and more optimised layout.

## Post-Layout extraction and Simulation
The layout can be drawn manually through Magic layout tool or can be automated with the help of scripts written in tcl.Once the layout is complete, the input nets are labelled by clicking on the gates followed by post layout extraction. Post layout extraction of SPICE model gives more insights regarding the parasitics that occur after creating the layout. the spice deck is run and the plots are looked again to verify that there is some difference between the pre-layout and the post layout simulations.

## CMOS Fabrication Process
16-Mask CMOS Process summary: 
* Selecting a substrate. 
* Creating active region for transistors
* N-Well and P-Well formation
* Formation of ‘gate'
* Lightly doped drain(LDD) formation
* Source and drain formation
* Steps to form contacts and interconnects(local)
* Higher level metal formation

## Lab Exercises

### Exp 1:
Go to labs and open terminal

Type below commands in terminal

cd
git clone https://github.com/kunalg123/ngspice_labs.git
cd ngspice_labs
cat inv.spice
What is the width of PMOS transistor?

<br/>
<p align="center">
<img src="https://github.com/BidyenduGhoshal/Beginner-SoC-Physical-Design-Warkshop/blob/main/Image/Day%203/L1.png" 
alt="alt text"  >
<p/>
<br/>

### Exp 2.1:
Go to labs, open terminal

Type below commands

cd
cd ngspice_labs
cat inv.spice
What is the width of NMOS transistor?

### Exp 2.2:
Go to labs, open terminal

Type below commands

cd 
cd ngspice_labs
cat inv.spice
What is Wp/Wn ratio?

<br/>
<p align="center">
<img src="https://github.com/BidyenduGhoshal/Beginner-SoC-Physical-Design-Warkshop/blob/main/Image/Day%203/L2.png" 
alt="alt text"  >
<p/>
<br/>

### Exp 3:
Go to labs, open terminal

Type below commands

cd
cd ngspice_labs
ngspice inv.spice
There will be terminal like below

ngspice 1 ->
On the above ngspice terminal, type below commands

run
setplot dc1
plot out in
This will open a plot with CMOS VTC and Blue 45 degree line

Click on the intersection of Blue line and CMOS VTC.

Go to terminal

What does "x0" value lies between?

<br/>
<p align="center">
<img src="https://github.com/BidyenduGhoshal/Beginner-SoC-Physical-Design-Warkshop/blob/main/Image/Day%203/L4.png" 
alt="alt text"  >
<p/>
<br/>

### Exp 3:
Go to labs, open terminal

Type below command

leafpad inv.spice
Edit the width of PMOS to 0.75u

Press Ctrl+s to save the file and exit the file

Repeat experiment given in D3SK1 - MCQ8

Where does the switching threshold lies between?

<br/>
<p align="center">
<img src="https://github.com/BidyenduGhoshal/Beginner-SoC-Physical-Design-Warkshop/blob/main/Image/Day%203/L5_1.png" 
alt="alt text"  >
<p/>
<br/>

<br/>
<p align="center">
<img src="https://github.com/BidyenduGhoshal/Beginner-SoC-Physical-Design-Warkshop/blob/main/Image/Day%203/L5_2.png" 
alt="alt text"  >
<p/>
<br/>

### Exp 4:
Go to labs, open terminal

Open file called "inv_tran.spice" using below command

leafpad inv_tran.spice
Change PMOS width to 0.75u, Save and Close

Type below commands for transient simulations

ngspice inv_tran.spice
ngspice 1 -> run
ngspice 1 -> setplot tran1
ngspice 1 -> plot out in
What is the rise delay? Refer to Lesson 4 of D3SK1 to learn how to calculate rise delay

<br/>
<p align="center">
<img src="https://github.com/BidyenduGhoshal/Beginner-SoC-Physical-Design-Warkshop/blob/main/Image/Day%203/L6.png" 
alt="alt text"  >
<p/>
<br/>

### Exp 5.1:
Go to labs, type below commands

cd
cd ngspice_labs
ngspice fn_prelayout.spice
ngspice 1 -> run
ngspice 1 -> setplot tran1
ngspice 1 -> plot out 1.25
What is the value of X0 at the intersection of horizontal blue line and middle rising waveform?

Hint - Click on the intersection
### Exp 5.2:
Repeat all steps in D3SK2 - MCQ1

What is the value of X0 at intersection of horizontal blue line and middle falling waveform ?
### Exp 5.3:
Pulsewidth is defined by the X0 value obtained in "D3SK2 - MCQ2" - "D3SK2 - MCQ1"

What is the pulsewidth ?

<br/>
<p align="center">
<img src="https://github.com/BidyenduGhoshal/Beginner-SoC-Physical-Design-Warkshop/blob/main/Image/Day%203/L7(1-5).png" 
alt="alt text"  >
<p/>
<br/>

### Exp 6:
Go to labs, open terminal

Type below commands

cd
cd ngspice_labs
magic -T min2.tech
This will open magic layout window and tkcon window

Go to tkcon window and type below command

source draw_fn.tcl
In layout window, how many nsubstratecontact and how many polysilicon strips you observe?

<br/>
<p align="center">
<img src="https://github.com/BidyenduGhoshal/Beginner-SoC-Physical-Design-Warkshop/blob/main/Image/Day%203/L8.png" 
alt="alt text"  >
<p/>
<br/>

### Exp 7:
Go to labs, open terminal

Type below command

cd
cd ngspice_labs
magic -T min2.tech fn_postlayout.mag &
What is the area of this design?

<br/>
<p align="center">
<img src="https://github.com/BidyenduGhoshal/Beginner-SoC-Physical-Design-Warkshop/blob/main/Image/Day%203/L9.png" 
alt="alt text"  >
<p/>
<br/>

# Day 4

## Timing modelling using delay tables
delay tables are tables which represent the delay of a gate as a function of input slew and output capacitance. As discussed before many standards cells of different sizes would be present in the library. to calculate the propagation delay across these cells, the delay table is used. if a value for a particular output load and an input slew is not present in the table, it is usually extrapolated from the nearby values present in the table. A typical delay table looks like the one given below
**Delay Table for CBUF'1'**
|Input Slew\Output Load|10fF|30fF|50fF|70fF|90fF|110fF|
|---|---|---|---|---|---|---|
|20ps|x1|x2|x3|x4|x5|x6|
|40ps|x7|x8|x9|x10|x11|x12|
|60ps|x13|x14|x15|x16|x17|x18|
|80ps|x19|x20|x21|x22|x23|x24|
## Clock Tree synthesis
Clock Tree synthesis is the next step after placement. Clock signals cannot be routed in the same way other data signals are routed because all the sequential elements in a particular stage expect the clock edges at the same time, in other words the clock skew should be zero.To achieve this clocks are routed in a h-tree fashion, instead of directly connecting the clk pins to the clk input of the sequential elements. Also at every level of the clock path , the load driven must be same inorder to ensure synchronous working of all the cells. this is maintained by using same type of buffer cells throughout a particular stage.

## Timing analysis with real and ideal clocks
Static timing analysis determines whether there is any setup or hold violations in the circuit. With ideal clocks, the analysis becomes easy, since the clk arrival time is definite and not subject to change. but in real scenarios, clk signals have noise and some uncertainity associated with it due to a variety of factors. Clock shielding can be done to prevent the noise impact on the clk signals.


## Lab Exercises

### Exp 1.1:
Open terminal and Type below commands

cd
git clone https://github.com/kunalg123/ngspice_labs
cd ngspice_labs
cat inv_tran.spice
What is the input rise slew and fall slew ?

### Exp 1.2:
Open terminal and Type below commands

cd
cd ngspice_labs
cat inv_tran.spice
ngspice inv_tran.spice
What is the output load and rise delay ?

<br/>
<p align="center">
<img src="https://github.com/BidyenduGhoshal/Beginner-SoC-Physical-Design-Warkshop/blob/main/Image/Day%204/L1.png" 
alt="alt text"  >
<p/>
<br/>

### Exp 2.1:
Go to labs Open below file using "leafpad" or "less" or "vim" - whichever you are comfortable with)

/usr/local/share/qflow/tech/osu018/osu018_stdcells.lib
Look between line numbers 21 to 24 What is the value of "slew_upper_threshold_pct_fall" ?

### Exp 2.2:
Go to labs Open the below file using leafpad or vim or less - whichever you are comfortable with

/usr/local/share/qflow/tech/osu018/osu018_stdcells.lib
Look for lines between 25 to 28

What is the value of output_threshold_pct_rise ?
### Exp 2.3:
Go to labs Open the below file using leafpad or vim or less - whichever you are comfortable with

/usr/local/share/qflow/tech/osu018/osu018_stdcells.lib
Look for line number 43

What are the 2 variables of "delay_template_5x5"?
### Exp 2.4:
Go to labs Open the below file using leafpad or vim or less - whichever you are comfortable with

/usr/local/share/qflow/tech/osu018/osu018_stdcells.lib
Look for line number 2943

The delay table below line number 2943 is for which cell ?
### Exp 2.5:
Which delay template is used for INVX1?

Hint Go to labs Open the below file using leafpad or vim or less - whichever you are comfortable with

/usr/local/share/qflow/tech/osu018/osu018_stdcells.lib
Look for line number 2983

<br/>
<p align="center">
<img src="https://github.com/BidyenduGhoshal/Beginner-SoC-Physical-Design-Warkshop/blob/main/Image/Day%204/L2-6.png" 
alt="alt text"  >
<p/>
<br/>

### Exp 3:
Go to labs

Type below command

cd
cd vsdflow/my_picorv32
leafpad picorv32.sdc
Type below lines in the file picorv32.sdc file which you have just opened above

create_clock -name clk -period 2.5 -waveform {0 1.25} [get_ports clk]
Save and close the above file

Now type below command

leafpad prelayout_sta.conf
Type below lines in prelayout_sta.conf file which you have just opened above

read_liberty /usr/local/share/qflow/tech/osu018/osu018_stdcells.lib
read_verilog synthesis/picorv32.rtlnopwr.v
link_design picorv32
read_sdc picorv32.sdc
report_checks
Save and close the above file

Now type below command

sta prelayout_sta.conf
What is the SLACK value you see?

<br/>
<p align="center">
<img src="https://github.com/BidyenduGhoshal/Beginner-SoC-Physical-Design-Warkshop/blob/main/Image/Day%204/L7.png" 
alt="alt text"  >
<p/>
<br/>

### Exp 4.1:
Repeat all steps in D4SK2 - MCQ11

NOTE - If you have already done that, then you will see below 'sta' terminal like below

%
Type below command

report_checks -digits 4
What is the data arrival time?
### Exp 4.2:
Repeat all steps in D4SK2 - MCQ12

What is data required time ?

<br/>
<p align="center">
<img src="https://github.com/BidyenduGhoshal/Beginner-SoC-Physical-Design-Warkshop/blob/main/Image/Day%204/L8-9.png" 
alt="alt text"  >
<p/>
<br/>

### Exp 5.1:
Perform all steps in D4SK2 - MCQ11

You are now at below "sta" terminal

%
Type below command in above terminal

set_propagated_clock [all_clocks]
report_checks
What is the SLACK value after clock propagation ?
### Exp 5.2:
Perform all steps in D4SK4 - MCQ2

What is launch clock network delay?

Hint - Scroll a little up at the beginning of report after "report_checks" command. There is line after "clock clk (rise edge)"
### Exp 5.3:
Perform all steps in D4SK4 - MCQ3

What is the capture clock network delay?

Hint - Look in the same report for the line "clock clk (rise edge)"

<br/>
<p align="center">
<img src="https://github.com/BidyenduGhoshal/Beginner-SoC-Physical-Design-Warkshop/blob/main/Image/Day%204/L10-12.png" 
alt="alt text"  >
<p/>
<br/>

### Exp 6:
Perform all steps in D4SK4 - MCQ3

Type below command

report_checks -path_delay min -digits 4
What is library hold time and hold slack? (type number as it is you see in timing report)

<br/>
<p align="center">
<img src="https://github.com/BidyenduGhoshal/Beginner-SoC-Physical-Design-Warkshop/blob/main/Image/Day%204/L13.png" 
alt="alt text"  >
<p/>
<br/>

# Day 5

## Routing
One of the most popular routing algorithm called Lee's algorithm or maze routing was explained during the workshop. The process starts from the driver and all the surrounding grids to the driver cell are numbered 1. then all the adjacent cells to cells numbered 1 are numbered as 2 and this process is repeated until the target grid is reached. now, there may be multiple paths between the source to the destination, but the one with the least number of bends is preferred .

## DRC 
Some rules regarding the metal layers need to be followed carefully to avoid DRC failure. Consider a pair of wires in the same metal layer,they are expected to maintain a minimum width, pitch and spacing between them. Two wires from different source to different destination should not intersect in the same metal layer. they have to be separated into different metal layers to avoid shorting. while doing so, rules like the specified Via Width and Via Spacing must be maintained.

## Parasitic extraction and the SPEF Format
**Parasitic Extraction** is calculation of the parasitic effects in both the designed devices and the required wiring interconnects of an electronic circuit: parasitic capacitances, parasitic resistances and parasitic inductances, commonly called parasitic devices, parasitic components, or simply parasitics. The major purpose of parasitic extraction is to create an accurate analog model of the circuit, so that detailed simulations can emulate actual digital and analog circuit responses. Digital circuit responses are often used to populate databases for signal delay and loading calculation such as: timing analysis; power analysis; circuit simulation; and signal integrity analysis. Analog circuits are often run in detailed test benches to indicate if the extra extracted parasitics will still allow the designed circuit to function.

**SPEF Format**
Standard Parasitic Exchange Format (SPEF) is an IEEE standard for representing parasitic data of wires in a chip in ASCII format. Non-ideal wires have parasitic resistance and capacitance that are captured by SPEF. These wires also have inductance that is not included in SPEF.


## Lab Exercises

### Exp 1:
Go to Day 5 labs

Open terminal

Type below commands

cd
cd vsdflow/my_picorv32
qflow route picorv32
qflow sta picorv32
qflow backanno picorv32
leafpad log/sta.log
What is the pre-layout frequency?

Hint - Search for case-sensitive keyword "MHz"

<br/>
<p align="center">
<img src="https://github.com/BidyenduGhoshal/Beginner-SoC-Physical-Design-Warkshop/blob/main/Image/Day%205/L1_1.png" 
alt="alt text"  >
<p/>
<br/>

<br/>
<p align="center">
<img src="https://github.com/BidyenduGhoshal/Beginner-SoC-Physical-Design-Warkshop/blob/main/Image/Day%205/L1_2.png" 
alt="alt text"  >
<p/>
<br/>

### Exp 2:
If you have completed above MCQ, then open the below file

log/post_sta.log
What is post-layout frequency?

<br/>
<p align="center">
<img src="https://github.com/BidyenduGhoshal/Beginner-SoC-Physical-Design-Warkshop/blob/main/Image/Day%205/L2.png" 
alt="alt text"  >
<p/>
<br/>


# Acknowledgements:
* Kunal Ghosh, Co-founder (VSD Corp. Pvt. Ltd)
