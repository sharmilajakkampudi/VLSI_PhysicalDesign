# Module 1- Inception of open-source EDA, OpenLANE and Sky130 PDK
## Introduction to QFN-48 Package, chip, pads, core, die and IPs
### For Example take an arduino
<img width="487" height="426" alt="image" src="https://github.com/user-attachments/assets/1060c413-18ca-4a05-a896-ce78ca61a530" />

An Arduino board contains an IC package, and inside the package is the actual chip. The chip contains different blocks required for processing and communication.

The block diagram of these arduino board chip consists of 

- RISC-V
- SRAM
- Processor / SoC
- GPIO
- UART
- SPI
- ADC
- JTAG
- Cell, Reset
- BSP / Factory ROM
- VCC / GND

### Chip Design

<img width="1112" height="581" alt="image" src="https://github.com/user-attachments/assets/ed19c01d-8880-4510-8382-6988740150c4" />



The IC package contains the chip. The package dimensions  are 7 mm × 7 mm.The package shown s is QFN-48, which stands for Quad Flat No-leads.

The pin locations of the package are dependent on the arduino board

The package is mounted on the board and provides the connections between the chip and the external circuit.

The chip contains different pins for communication and other connections.

chip have various components some importnt components are

  - pads
  - core
  - die
 
Pads: Sends signals inside the chip

Core: A place where digital logic sits

### RISC-V SoC

<img width="1052" height="552" alt="image" src="https://github.com/user-attachments/assets/aefe3617-61ca-456c-ac16-57b330d7080d" />


Generally a typical consists of 

- Soc
- SRAM
- dac
- adc
- PLL
- SPI

These are also known as Foundary IP's all the the devices like mobiles phones etc are dependent on Foundary

The digital blocks RISCV Soc and SPI are Macro's

## Introduction to RISC-V

RISC-V is an Instruction Set Architecture (ISA). It defines the instructions that the processor understands.

A program provides instructions to perform particular operations.

RISC-V architecture can be described using RTL (Register Transfer Level).

RTL describes the hardware, while HDL (Hardware Description Language) is used to describe the hardware design.

The Flow is 

- It starts from Architecture
- Implemented Using RTL
- RTL to Layout

## From Software Applications to Hardware

The software instructions eventually need to be converted into a form that the hardware can understand.

The Basic Flow is
```

OS   --->   Compiler   --->   Assembler   --->   Hardware

```

#### OS
OS Job is to take the app and convert it into its respective assembly language and finally to binary language . The outputs of the of the os is functions are given to the compiler and are converted into instructions

The syntax of instructions are dependent on the type of the hardware

#### Compiler

The compiler converts the program into another form that can be understood by the hardware.

#### Assembler

The assembler takes the instructions and converts them into binary numbers.

#### Binary Language

The binary instructions are given to the hardware. The hardware understands the binary language, and the processor executes the required operation to produce the output.

#### RISC-V to Hardware

The conversion from a program to hardware operation can be represented as:

```

Program → Instructions → Assembler → Binary Numbers → Hardware → Processor → Operation

```

The assembler converts the instructions into binary numbers. These binary instructions are supplied to the hardware, where the processor understands and executes them.

## Introduction to all components of open-source digital aisc design

### Introduction to ASIC Design

<img width="1177" height="623" alt="image" src="https://github.com/user-attachments/assets/22bd91a0-3986-4d50-a31f-53b2fda0423b" />

ASIC stands for Application Specific Integrated Circuit.

The ASIC design flow is used for physical implementation of the design.

The Elements are
- RTL IP's
- EDA Tools
- PDK Data

### Open - Source Digital ASIC Design

Digital AISC using 

- Open Source EDA Tools
- Open Source RTL Designs
- Open Source PDK Data

#### PDK

- The design of an IC has to be tightly integrated with manufacturing.
- Process available with each company.
- There is a boundary between design technology and manufacturing technology.

PDK is an interface between the PDK and the designers

#### Foundry
- Foundry is responsible for fabrication/manufacturing.
- EDA tools are used to design the IC.

Collections of files used to model a fabrication process for the EDA tools used to design

- Process design kits
- Device models
- Digital standard cell library
- I/O Libraries

#### EDA Tools

The EDA tools:

- Simulation
- HDL simulation
- Digital placement
- Floor planning
- LVS
- Logic synthesis
- Open
- Detailed placement
- RC extraction
- RTL synthesis
- P&R
- DFT
- Global routing etc

## Simplified RTL to GDSCII Flow

Flow is:

```

RTL
 ↓
Synthesis
 ↓
Floor planning
 ↓
PDK
 ↓
Placement
 ↓
Clock Tree Synthesis
 ↓
Routing
 ↓
Sign-off
 ↓
GDSII

```

### Synthesis

Synthesis is the process to convert RTL to a circuit out of compoments from standard cell library(SCL)



The design output of RTL is a standard-cell-based circuit.


Standard cells have regular layouts.

Each has Different views/models they are

- Electrical
- Layout

### Floor and Power Planning

#### Floor Planning

Floorplanning is the process of deciding the overall physical arrangement of the design.

It Detemines
- Chip/core dimensions
- Placement of major blocks
- I/O locations
- Macro locations
- Power distribution structure

#### Power Planning

Power planning creates the power distribution network.

Important signals:

- VDD → Power
- VSS/GND → Ground

Power must reach all cells reliably.

Power planning considers:

- Power rings
- Power stripes
- Power rails

### Placement

Placement determines the physical locations of standard cells.

Two important types:


#### Global Placement

Provides an initial approximate placement of cells.

#### Detailed Placement

Refines the cell locations while satisfying physical constraints.

The objective is to achieve:

- Good timing
- Low congestion
- Small area
- Efficient routing

### Clock Tree Synthesis
- Clock Tree Synthesis is used to distribute the clock.
- Clock distribution network is created.
- The clock should reach the required sequential elements.
- Timing must be considered.

### Routing

Routing connects all placed cells according to the netlist.

Two major stages are:

#### Global Routing

Determines approximate routing paths.

#### Detailed Routing

Creates the actual geometrical routes while following design rules.

Routing must consider:

- Congestion
- Timing
- Design rules
- Signal integrity
- Wire length

### Sign-off

Sign-off is the final verification stage before manufacturing.

Important sign-off checks include:

- Timing verification
- Physical verification
- DRC
- LVS
- Power analysis
- Signal integrity

Only after the design passes required checks is it prepared for fabrication.

## Introduction to OpenLane and Strive Chipsets


OpenLane is an open-source flow for a true open-source ASIC.


It is a tool for running an open-source ASIC flow.


OpenLane Includes


- Open PDK
- Open EDA
- Open RTL

### Strive Soc Family

| SoC | Features |
|---|---|
|Strive | Sky 130 SCL + Synthesized 1Kbyte SRAM |
|Strive 2 | Sky 130 SCL + 1Kbyte Open RAM block|
| Strive 2a | Strive 2 with a Single chip module |
| Strive 3 | OSU SCL + Synthesized 1Kbyte SRAM |
| Strive 5 | Sky130 SCL + 8 KB OpenRAM blocks |
| Strive 6 | Strive 2 with DFT |

### OpenLane ASIC

Main Objective is to produce a clean GDSII with no human intervention 

Here clean means
- No LVS violations
- No DRC violations
- Timing violations

These are tuned for sky water 130nm Open PDK

Containerised 

- Functional oy=ut of the box
- Instructions to build and run natively will follow

Two Modes of Operations are

- Autonomous
- Interavtive

## Introduction to Openlane detailed ASIC design flow

### Openlane Regression Testing

- Design exploration can be used to find the best configuration.
- Regression testing can be performed.
- Scan insertion
- Automatic test pattern generation
- Test pattern compression
- Fault coverage
- Fault simulation

#### Physical Implementation

This physical implantation is also known as PnR(Place and Route)

- Floor/reset planning
- Decoupling capacitors
- Placement
- Clock Tree Synthesis
- Routing: Global and detailed

### Openlane Directory Commands

| Command | Purpose |
|---|---|
| `cd Desktop` | Moves to the Desktop directory |
| `cd work/tools` | Moves into the `work/tools` directory |
| `cd openlane_working_dir` | Opens the OpenLane working directory |
| `cd openlane` | Opens the main OpenLane directory |
| `cd designs` | Opens the directory containing OpenLane designs |
| `cd picorv32a` | Opens the PICORV32A design directory |
| `cd src` | Opens the source-code directory containing RTL files |
| `cd synthesis` | Moves into the synthesis directory; normally synthesis results are under `runs/<run>/results/synthesis` |

### 

### OpenLane Docker Commands

| Command | Purpose |
|---|---|
| `docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21` | Starts the OpenLane Docker container and mounts the current directory and PDK directory into the container |
| `pwd` | Displays the current working directory |
| `./flow.tcl -interactive` | Starts the OpenLane flow in interactive mode |
| `package require openlane 0.9` | Loads the OpenLane package, version 0.9, into the Tcl environment |
| `prep -design picorv32a` | Prepares the PICORV32A design by loading its RTL, configuration, constraints, and required files |
| `run_synthesis` | Runs the **synthesis stage**, converting the RTL design into a gate-level netlist using standard cells |


### Basic commands

| Command | Purpose |
|---|---|
| `cd` | Changes the current directory. When used alone, it takes you to your home directory |
| `ls` | Lists the files and directories in the current location |
| `pwd` | Displays the full path of the current working directory |
| `less` | Opens a file and allows you to view its contents page by page |

## Module-1 Conclusion

This module provides an understanding of the open-source ASIC design flow, starting from the basic components of a chip and progressing towards physical implementation. It covers RISC-V, RTL, EDA tools, PDK, synthesis, floor planning, power planning, placement, CTS, routing and sign-off. It also introduces OpenLane, Sky130 PDK, Strive SoCs, regression testing and OpenLane commands. Overall, the module gives a clear foundation for understanding how an RTL design is taken through the ASIC flow and converted into GDSII for fabrication.






























  
