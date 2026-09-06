# Module 2-Good floorplan vs bad floorplan and introduction to library cells

##  Chip Floor planning considerations

## Utilization factor and aspect ratio

### Define Height and Width of Core and Die

1)Begin with the netlist


For Example take two flops and a simple combinational logic  between the flip flops

The two flip flops:

- Launch Flop
- Capture Flop

<img width="837" height="267" alt="WhatsApp Image 2026-09-06 at 18 05 15" src="https://github.com/user-attachments/assets/9217ee0b-ced5-48c8-9e91-04659f05ada6" />

2)Give physical dimensions like length and breadth to the gates 

<img width="847" height="277" alt="image" src="https://github.com/user-attachments/assets/7e80ceb8-5f08-4864-86a9-9f7e4fa2fe27" />

To Identify the dimensions of the chip we dimensions of the standard cells

For Example:

The rough dimensions of the standard cells are

- length=1 unit

- breadth=1 unit so the area of the standard cell is 1 square unit

The dimensions of the flip flops are

- length=1 unit

- breadth=1 unit so the area of flip flops is 1 square unit

### What are Core and Die in a chip?

#### Die 

The die is the entire piece of semiconductor material (usually silicon) on which the chip is fabricated.

#### Core

The core is the main internal area of the die where the actual digital logic and functional circuits are placed.

#### How to arrive on its dimensions?

- Place all the logical cells inside the core
- The logical cells occupies the complete area of the core

```

Utilisation Factor= Area occupied by the netlist
                    ----------------------------
                    Total Area of the Core
```

```

Aspect Ratio=Height
             ------
             Width

```

### Define Locations of Preplaced Cells

Example: 1)Take a combinational logic

<img width="533" height="272" alt="image" src="https://github.com/user-attachments/assets/dbe7264b-49fb-477a-b944-8d6ed061768e" />

2)Divide this logic into Blocks

<img width="552" height="236" alt="image" src="https://github.com/user-attachments/assets/36cb706a-233d-4550-814d-f59b5a5fb2d1" />

- output of A1 is connected to input of A5


- output of A2 is connected to input of A5


- output of A3 is connected to input of A6


- output of A4 is connected to input of A6

3)Seperate the Black Boxes as two different IP's or modules

4)Similarly there are other IP's also available for examples they are


i)memory

ii)clock gating cell

iii)compartor

iv)Mux

5)The arrangement of these cells is known as Floor Planning

6)These IP blocks have user defined locations and hence are placed in the chip before automated placement-and-routing are called pre placed cells

7)Automated placement and routing tools places the  remaining logical cells in the design onto the chip

### Surround pre-placed cells with De-coupling Capacitors

1)Consider the amount of switching current reqiured for a complex circuit 
- consider capacitance to be zero for the discussion Rdd,Rss,Ldd and Lss are well defined values
- during switching opeartion the circuit demands switching current
- Now due to the presence of Rdd and Ldd there will be voltage at Node A would Vdd
- If Vdd goes below the noise margin due to Rdd and Ldd the logic '1' at the output of the circuit wont be detected as logic '1' at the input of the circuit

<img width="932" height="586" alt="image" src="https://github.com/user-attachments/assets/c06edaad-ed99-43de-89f6-fee9e4dc6c15" />

#### Add Decoupling capacitor as the solution 

- Addition of decoupling capacitor in parallel with the circuit
- Everytime the circuit switches it draws current from Cd whereas the RL Network is used to replenish the charge into Cd

### Power Planning

Power planning is deciding how VDD and VSS should be distributed across the chip so that every circuit receives sufficient and stable power.

The basic path is:

```

Power Supply → Power Distribution Network → Driver → Load

```

The important things checked during power planning are:

- IR drop – voltage loss due to resistance
- Electromigration – excessive current in metal wires
- Ldi/dt noise – voltage fluctuation due to inductance
- Power integrity – maintaining stable VDD/VSS
- Decoupling capacitance – reducing supply fluctuations
- Proper width and spacing of power straps

The resistance and inductance between the supply and circuits can cause the actual voltage at the load to differ from the ideal VDD/VSS.

### Pin Placement

Pin placement is the process of deciding the physical locations of input, output, and clock pins on the boundary of the chip. Proper pin placement helps to reduce routing congestion, minimize wire length, and improve timing. Pins are placed based on their connectivity with internal blocks to ensure efficient signal routing.

<img width="863" height="727" alt="image" src="https://github.com/user-attachments/assets/68c2d64e-3590-402f-ab21-6602a366c3fa" />

In the given design, pins such as Din1–Din4 are input pins, Dout1–Dout4 are output pins, and Clk1, Clk2 and ClkOut are clock pins. The pins are placed around the die/core boundary according to their connectivity with the internal blocks (FlipFlopss and logic gates).

### Commands to run floor planning using OpenLane

Commads run on the configuration directory is

```

pwd

ls -ltr

less README.md

less floorplan.tcl

```

Commands to run in Open lane working directory


```

cd designs

cd picorv32a

ls -ltr

```

Commands to run in docker Openlane

```

run_synthesis

run_floorplan

```

Commands used to get the Layout diagram

```

magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &

```

Layout Diagram is 

<img width="957" height="581" alt="WhatsApp Image 2026-09-06 at 13 36 05" src="https://github.com/user-attachments/assets/e9a5c3db-3c4a-48c6-8077-8ce2fa15b44e" />

## Library  Binding and Placement

### Placement and Routing

1)Bind the netlist with physical cells


2)Placement

Placement is the process of physically arranging the standard cells and functional blocks inside the core area after floorplanning. The cells are placed based on their connectivity to achieve shorter connections, less congestion, and better timing.

<img width="1382" height="645" alt="image" src="https://github.com/user-attachments/assets/3563888b-216f-4fcb-af96-214cae4e700a" />


 FF1, logic gates (1 and 2), and FF2 are placed in rows according to their connections. DECAP1, DECAP2, DECAP3 and Blocks A, B, C are also placed inside the core. The placement is done while keeping the input/output pins and clock connections properly aligned for efficient routing.
 
 3)Optimise Placement

This is the stage where we estimate the wire length and capacitance and based on that insert repeaters

### Need for Characterisation

Library characterization is the process of analyzing standard cells to determine their timing, power, and noise characteristics under different input slew and output load conditions. These characteristics are then stored in a standard cell library and used during synthesis and physical design.


The design flow is

```

Logic Synthesis → Floorplanning → Placement → Routing

```

<img width="1192" height="450" alt="image" src="https://github.com/user-attachments/assets/ab649581-7b89-4600-92e0-82193f9c6257" />

Commands used to generate the layout is

```

run_placement

cd ../placement/

ls

magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &

```

<img width="942" height="577" alt="image" src="https://github.com/user-attachments/assets/e43c00ca-1f6a-4e74-babc-e75088f30196" />

## Cell Design and Characterisation flows

<img width="1326" height="670" alt="Screenshot 2026-09-06 221237" src="https://github.com/user-attachments/assets/9260a74c-3c01-465c-8e9a-845746312f58" />

Standara cells used are

- AND Gate
- OR  Gate
- BUFFER
- INVERTER
- DFF
- LATCH
- ICG

Inputs are


Process Design Kits(PDKS)
  - DRC
  - LVS
  - Spices models
  - libraries
  - user defined specs

Standard cell design starts with PDK inputs (DRC/LVS rules, SPICE models, specs) that define what layout geometry is legal. A transistor-level schematic is converted into PMOS/NMOS network graphs, and a common Euler path across both determines the optimal gate ordering for a compact stick diagram. This stick diagram is turned into a DRC-clean transistor layout, which is then extracted and characterized for timing and power. Repeating this across multiple functions, drive strengths, and Vt flavors builds up the full standard cell library — the fundamental building blocks that place-and-route tools stitch together to construct complete chips.

### Characterisation flow

1)Build a testbench schematic: DUT (subcircuit) + input stimulus (pulse) + supply (DC) + standard output load (cap).

2)Generate the top netlist that instantiates the DUT and stimulus, and calls the subcircuit file.

3)The subcircuit file defines the cell purely at the transistor level (PMOS/NMOS with W/L) and pulls in the process SPICE models (NMOS/PMOS .lib files) so the transistors behave like real silicon.


4)Run a .tran transient simulation — this drives the pulse through the cell and captures in1, out_inter, and out1 waveforms over time.


5)From those waveforms, extract characterization metrics: propagation delay, rise/fall time, output transition, and power — for that specific load (10fF) and supply voltage.


6)This measured data becomes one entry (one PVT corner, one load, one input slew) in the cell's .lib timing/power model — repeated across many loads, slews, and corners builds the complete characterization table used by STA/synthesis tools.

<img width="1122" height="613" alt="Screenshot 2026-09-06 224135" src="https://github.com/user-attachments/assets/c9614784-f4bb-41ef-82d0-8f58d348064d" />

## General timing characterization parameters

### Timing Characterization

Timing characterization is the process of precisely defining where on a voltage waveform a signal transition "officially" starts and ends, so that delay and slew (transition time) can be measured consistently and repeatably — rather than from the true 0%–100% edges, which are noisy and hard to pinpoint.

## Timing Threshold Definitions

| Threshold | What it marks |
|---|---|
| `slew_low_rise_thr` | Lower % point used to measure a rising edge's transition time |
| `slew_high_rise_thr` | Upper % point used to measure a rising edge's transition time |
| `slew_low_fall_thr` | Lower % point used to measure a falling edge's transition time |
| `slew_high_fall_thr` | Upper % point used to measure a falling edge's transition time |
| `in_rise_thr` | The % point on the input rising edge used as the delay reference |
| `in_fall_thr` | The % point on the input falling edge used as the delay reference |
| `out_rise_thr` | The % point on the output rising edge used as the delay reference |
| `out_fall_thr` | The % point on the output falling edge used as the delay reference |

#### Waveforms are

<img width="556" height="600" alt="image" src="https://github.com/user-attachments/assets/64c3dd63-5436-424b-b402-988a6ffeb424" />

<img width="596" height="607" alt="image" src="https://github.com/user-attachments/assets/d212d024-556c-44d3-aae7-e46f3057e513" />

<img width="580" height="608" alt="image" src="https://github.com/user-attachments/assets/ca8f8664-11d5-4942-8333-b4403704046f" />

#### Propagation Delay

Propagation delay is the time it takes for a change at a cell's input to produce the corresponding change at its output — essentially, how long the signal takes to "propagate" through the gate.

- It's the fundamental number that determines how fast a signal can move through a chain of logic gates.
- Synthesis and static timing analysis (STA) tools sum up propagation delays along every path in the design to check whether the chip meets its target clock frequency (setup/hold timing).
- It varies with output load (more capacitance → slower) and input slew (slower input edge → slower output response) — which is exactly why characterization runs this measurement across many load/slew combinations to build the full .lib delay table (as in the SPICE flow discussed earlier).

#### Transition time


Transition time (also called slew) is the time a signal takes to switch between its low and high logic levels — i.e., how "sharp" or "slow" an edge is, rather than how long it took to start (that's propagation delay).

- A slower (larger) transition time means the gate spends longer in the "linear" region where both PMOS and PFET conduct — this increases short-circuit power and propagation delay.
- It directly affects the next gate's propagation delay too, since delay depends on input slew — this is why .lib characterization tables are indexed by both output load and input transition time: delay and output slew are looked up together for every (load, input-slew) pair.
- Excessive transition time on a net is flagged as a slew violation in STA — if an edge is too slow, it can cause functional or timing issues downstream.

## Module-2 Conclusion

This module provided an understanding of the physical design flow from netlist to layout, including floorplanning, placement, power planning, pin placement, and routing. The concepts of utilization factor, aspect ratio, pre-placed cells, decoupling capacitors, and power distribution were studied to achieve an efficient floorplan. The module also introduced standard cell library binding, placement optimization, library characterization, and timing parameters such as delay and slew. Overall, these concepts show how logical designs are converted into a physically optimized and timing-aware chip layout using tools such as OpenLane and Magic.


























