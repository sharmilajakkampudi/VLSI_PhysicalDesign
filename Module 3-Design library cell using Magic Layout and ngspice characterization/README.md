# MODULE 3
# Design Library Cell using Magic Layout and ngspice Characterization

---

## 1. CMOS Inverter ngspice Simulations

A CMOS inverter is a basic digital circuit made using one PMOS and one NMOS transistor. The gates of both transistors are connected to the input, and their drains are connected together to form the output.

The CMOS inverter performs the NOT operation.

When the input is LOW, the PMOS is ON and the NMOS is OFF. Therefore, the output is HIGH.

When the input is HIGH, the PMOS is OFF and the NMOS is ON. Therefore, the output is LOW.

Truth table:

| Input | Output |
|-------|--------|
| 0 | 1 |
| 1 | 0 |

ngspice is used to simulate the electrical behavior of the CMOS inverter.

The main characteristics studied are:

- Voltage Transfer Characteristic (VTC)
- Switching threshold voltage
- Static behavior
- Dynamic behavior
- Rise time
- Fall time
- Propagation delay
- Power consumption

---

## 2. SPICE Deck Creation for CMOS Inverter

SPICE stands for Simulation Program with Integrated Circuit Emphasis.

A SPICE deck is a text file that describes the circuit and contains the instructions required for simulation.

A SPICE deck generally contains:

- Component connectivity
- Component Values
- Identify nodes
- Name nodes
- Device models
- Simulation commands
- Output commands
- `.end`

A MOS transistor is generally written as:

Example:

Model Descriptions and Netlist Descriptions



M1 out in vdd vdd pmos w=0.375u L=0.25u

M2 out in 0 0 nmos w=0.375u L=0.25u

cload out 0 10f

Vdd vdd 0 2.5

Vin vin 0 2.5

Simulation conditions are :

```

.op

.dc Vin 0 2.5 0.05

```





## 3. SPICE Simulation of CMOS Inverter

ngspice is an open-source circuit simulator used to simulate the CMOS inverter.

The SPICE deck is given as input to ngspice.

The simulation can be performed using:

ngspice filename.spice






DC analysis is used to obtain the Voltage Transfer Characteristic of the CMOS inverter.

Transient analysis is used to study the output response with respect to time.

---

## 4. Switching Threshold Vm

The switching threshold voltage is represented by Vm.



Spice waveform  Wn=Wp=0.325u,Ln,p=0.25u (Wn/Ln=Wp/Lp=1.5)

It is the input voltage at which the input voltage and output voltage are equal.

At the switching threshold:

VIN = VOUT = Vm

The switching threshold represents the point at which the CMOS inverter changes its logic state.

Vm depends on the relative strength of the PMOS and NMOS transistors.

Transistor sizing can affect:

- Switching threshold
- Noise margin
- Propagation delay
- Power consumption
- Voltage transfer characteristic

---

## 5. Static and Dynamic Simulation of CMOS Inverter

### Static Simulation

Static simulation studies the steady-state behavior of the CMOS inverter.

```

Vm=R.Vdd/(1+R)

where R=Kp.Vdsatp/Kn.Vdsatn

```

### Dynamic Simulation

Dynamic simulation studies the response of the inverter with respect to time.

A time-varying input signal is applied.

The important parameters are:

- Rise time
- Fall time
- Propagation delay
- Dynamic power

Rise time is the time required for the output to change from LOW to HIGH.

Fall time is the time required for the output to change from HIGH to LOW.

Propagation delay is the time difference between an input transition and the corresponding output transition.

The two propagation delays are:

tpLH - Low-to-High propagation delay

tpHL - High-to-Low propagation delay

Average propagation delay:

tp = (tpLH + tpHL) / 2

---

# 6. Git Clone of vsdstdcelldesign

<img width="1852" height="867" alt="image" src="https://github.com/user-attachments/assets/b888aded-b951-433e-880e-b5bbc6cc75e9" />

The commands used are:

```

git clone https://github.com/nickson-jose/vsdstdcelldesign.git

for simulation

magic -T sky130.tech sky130_inv.mag &

```


# 7. Inception of Layout – CMOS Fabrication Process

Layout is the physical representation of an integrated circuit.

It represents the different layers and geometries required to manufacture the circuit.

The CMOS fabrication process creates:

- Active regions
- N-well
- P-well
- Gate
- Source
- Drain
- Local interconnect
- Metal layers

The general fabrication flow studied in this module is:

Active region formation
→ N-well and P-well formation
→ Gate formation
→ LDD formation
→ Source and drain formation
→ Local interconnect formation
→ Higher-level metal formation

---

# 8. Creation of Active Regions

The active region is the semiconductor area where the source, drain and channel of a MOS transistor are formed.

The active region is also commonly called the diffusion region.

For NMOS:

- NMOS is formed in P-type material.
- Source and drain are N+ doped regions.

For PMOS:

- PMOS is formed inside an N-well.
- Source and drain are P+ doped regions.

The active region defines where the transistor can be formed.

---

# 9. Formation of N-Well and P-Well

A well is a semiconductor region with a specific type of doping.

CMOS technology uses:

- N-well
- P-well

### N-Well

N-well is used to form PMOS transistors.

The PMOS source and drain are formed using P+ diffusion inside the N-well.

The N-well is normally connected to VDD through a well tap.

### P-Well

P-well is used to form NMOS transistors.

The NMOS source and drain are formed using N+ diffusion inside the P-well.

The P-well is normally connected to GND through a well tap.

The wells provide the required body region for the MOS transistors.

---

# 10. Formation of Gate Terminal

The gate terminal controls the conduction of a MOS transistor.

The gate is formed by placing polysilicon over the active region with gate oxide between them.

The intersection of polysilicon and active region forms the transistor channel.

In a CMOS inverter, the same input signal is connected to:

- PMOS gate
- NMOS gate

Therefore:

VIN → PMOS gate
VIN → NMOS gate

The gate controls whether the transistor is ON or OFF.

---

# 11. Lightly Doped Drain (LDD) Formation

LDD stands for Lightly Doped Drain.

LDD regions are lightly doped regions placed between the channel and heavily doped source/drain regions.

The main purpose of LDD is to reduce the electric field near the drain.

LDD helps to reduce hot-carrier effects and improves transistor reliability.

The general structure is:

Source
→ LDD
→ Channel
→ LDD
→ Drain

---

# 12. Source and Drain Formation

Source and drain are heavily doped regions of a MOS transistor.

For NMOS:

- Source = N+ region
- Drain = N+ region

For PMOS:

- Source = P+ region
- Drain = P+ region

The source and drain provide the terminals through which current flows in the MOS transistor.

---

# 13. Local Interconnect Formation

Local interconnect is used to connect nearby transistor terminals.

It provides connections between:

- Diffusion
- Gate
- Contacts
- Metal layers

A simplified connection is:

Diffusion
→ Contact
→ Local Interconnect
→ Metal

Local interconnect reduces routing complexity and provides short-distance connections between device regions.

---

# 14. Higher-Level Metal Formation

Metal layers are used to connect different devices and cells.

Common metal layers include:

- Metal1
- Metal2
- Metal3
- Higher metal layers

Metal1 is generally used for local routing.

Higher metal layers are used for longer connections and global routing.

Different metal layers are connected using vias.

Basic structure:

Metal1
→ Via
→ Metal2
→ Via
→ Metal3

Higher-level metals can also be used for:

- Power distribution
- Clock distribution
- Long-distance routing
- Signal routing

---

# 15. Sky130 Basic Layers

Sky130 is an open-source 130 nm process design kit.

Important layers used in Sky130 layout include:

- N-well
- Diffusion
- Tap
- Poly
- Contact
- Local interconnect
- Metal1
- Metal2
- Via

Each layer represents a physical part of the semiconductor fabrication process.

The layout of a CMOS inverter is created using these layers.

---

# 16. LEF

LEF stands for Library Exchange Format.

LEF describes the physical information of standard cells.

It contains information such as:

- Cell dimensions
- Cell height
- Cell width
- Pin locations
- Pin layers
- Routing information
- Obstructions

LEF provides an abstract physical view of a standard cell for physical design tools.

SPICE mainly represents the electrical behavior of the cell, while LEF represents the physical abstract information.

---

# 17. Standard Cell Layout

A standard cell is a pre-designed and characterized circuit block used in digital IC design.

Examples of standard cells include:

- Inverter
- Buffer
- NAND
- NOR
- AND
- OR
- XOR
- Flip-flop

A standard cell generally contains:

- Layout
- Pins
- Power connections
- SPICE netlist
- Physical dimensions
- Characterization data
- LEF

The CMOS inverter is used as an example of a standard cell in this module.

---

# 18. Extra SPICE Netlist

After creating the layout, an extracted SPICE netlist can be generated from the layout.

The extracted netlist represents the electrical circuit corresponding to the physical layout.

The extracted circuit can be simulated using ngspice.

This allows the designer to verify that the physical layout behaves correctly.

Basic flow:

Layout
→ Extraction
→ Extracted SPICE netlist
→ ngspice simulation

---

# 19. Sky130 Technology File

A technology file contains information required by the layout tool to understand a particular fabrication technology.

For Sky130, the technology information includes:

- Layer definitions
- Layer names
- Layer properties
- Design rules
- Connectivity
- Extraction information
- DRC rules

The technology file allows Magic to interpret the Sky130 layout correctly.

---

# 20. Final SPICE Deck Using Sky130 Technology

The final SPICE deck uses the actual Sky130 device models.

The Sky130 model files contain process-specific transistor parameters.

The main devices used for CMOS inverter simulation are:

- NMOS
- PMOS

Using Sky130 model files gives a more realistic simulation of the inverter for the selected technology.

The flow is:

Sky130 PDK
→ Model files
→ SPICE deck
→ ngspice
→ Simulation

---

# 21. Characterization of CMOS Inverter Using Sky130 Models

Characterization is the process of determining the electrical performance of a standard cell.

The CMOS inverter can be characterized for:

- Switching threshold
- Rise time
- Fall time
- Propagation delay
- Power
- Input capacitance
- Output behavior

Timing characterization includes:

tpLH
tpHL
Rise time
Fall time

Power characterization includes:

- Static power
- Dynamic power
- Leakage power
- Short-circuit power

Characterization data is important when the cell is used in larger digital circuits.

---

# 22. Magic Layout Tool

Magic is an open-source VLSI layout tool.

Magic is used for:

- Creating layouts
- Editing layouts
- Viewing layers
- Design Rule Checking
- Circuit extraction
- Layout verification

The layout is created using different layers defined by the technology file.

Magic must be loaded with the appropriate Sky130 technology information.

---

# 23. DRC

DRC stands for Design Rule Check.

DRC checks whether the physical layout follows the manufacturing rules of the selected technology.

DRC checks include:

- Minimum width
- Minimum spacing
- Enclosure
- Overlap
- Contact rules
- Via rules
- Poly rules
- Metal rules
- Diffusion rules

A layout must be DRC clean before it can be considered physically valid.

---

# 24. Sky130 PDK

PDK stands for Process Design Kit.

A PDK contains the information required to design circuits for a particular semiconductor manufacturing process.

A PDK can contain:

- Technology files
- Design rules
- Device models
- Layer information
- SPICE models
- Extraction rules
- DRC rules
- LVS rules
- Standard-cell information

The Sky130 PDK is used with tools such as Magic and ngspice.

---

# 25. Loading Sky130 Technology Rules in Magic

Magic requires technology information to understand the layers and design rules of Sky130.

The technology file provides information about:

- Layer definitions
- Layer properties
- Design rules
- Connectivity
- Extraction

A typical Magic command is:

magic -T <technology-file>

After loading the technology, the Sky130 layout can be created and checked using Magic.

---

# 26. DRC Rules

Design rules specify the minimum physical requirements that must be followed during layout.

Important types of rules are:

### Width Rule

Defines the minimum width of a layer.

```text
Actual width ≥ Minimum required width

```

## Spacing Rule

Defines the minimum distance between two shapes.

Actual spacing ≥ Minimum required spacing


## Enclosure Rule

Defines how much one layer must surround another layer.

## Overlap Rule

Defines the required overlap between two layers.

> These rules ensure that the layout can be manufactured correctly.

---

## 27. `poly.9` DRC Error

`poly.9` is a specific design-rule check associated with the polysilicon layer in the Sky130 technology rules.

When this DRC error occurs, the layout geometry must be examined to determine why the poly rule is being violated.

**Correct approach:**

DRC error → Identify rule → Understand rule condition → Examine geometry → Modify layout → Run DRC again


> The DRC marker should not simply be ignored or removed.

---

## 28. Poly Resistor Spacing to Diffusion and Tap

Polysilicon can be used to create a resistor structure.

The poly resistor must maintain the required spacing from other layers such as:

- Diffusion
- Tap

If the required spacing is not maintained, a DRC violation occurs.

The spacing requirement is determined by the Sky130 technology rules.



---

## 29. DRC Error as a Geometrical Construct

A DRC rule is fundamentally a geometrical condition. The DRC engine checks relationships between physical shapes in the layout.

**Examples:**

| Rule Type | Description |
|-----------|-------------|
| Width     | A layer must have sufficient width. |
| Spacing   | Two shapes must have sufficient distance between them. |
| Enclosure | One layer must surround another layer by the required amount. |
| Overlap   | Two layers must overlap by the required amount. |

**Therefore, a DRC error can be understood as:**

Layout geometry → Rule condition → Violation


> Understanding the geometry behind a DRC error helps in fixing the layout correctly.

---

## 30. Missing or Incorrect DRC Rules

A technology file may contain an incorrect or missing design rule.

- An **incorrect rule** can generate a false DRC error.
- A **missing rule** can allow an invalid layout to pass DRC.

Therefore, the DRC rules must be checked carefully.

**General debugging process:**

1. Create the layout.
2. Run DRC.
3. Observe the DRC result.
4. Identify the rule.
5. Understand the geometrical condition.
6. Compare the rule with the required technology specification.
7. Identify whether the rule or layout is incorrect.
8. Correct the rule or layout.
9. Run DRC again.
10. Verify the result.

# Module 3 Conculsion


Module 3 covered the complete CMOS standard-cell design flow using ngspice, Magic, and the Sky130 PDK. The CMOS inverter was simulated using SPICE to study VTC, switching threshold, static and dynamic characteristics, delay, and noise margins.

The module also covered CMOS fabrication steps such as active-region, well, gate, LDD, source-drain, interconnect, and metal formation. Magic was used for layout creation and DRC verification, including analysis of width, spacing, enclosure, and poly-related errors.

Overall, the module provided an understanding of the complete flow from CMOS circuit simulation to layout, DRC verification, extraction, and standard-cell characterization.

