# MODULE 5- Final Steps for RTL2GDS Using TritonRoute and OpenSTA

## Routing and Design Rule Check (DRC)

### 1. Introduction to Maze Routing

<img width="1317" height="613" alt="image" src="https://github.com/user-attachments/assets/f720f74d-85e7-4376-ab54-12755d86d806" />


Routing is the process of establishing physical connections between different circuit elements such as standard-cell pins, macros, and I/O ports using the available metal layers and vias.

After placement, the positions of the cells are known, but the actual physical paths connecting them still have to be created. Routing determines these paths while avoiding obstacles and satisfying manufacturing design rules.

A common way to understand routing is through maze routing.

In maze routing, the routing area is represented as a grid. The router searches through this grid to find a path from a source to a destination.

Basic idea:
```
Source
   ↓
Search neighboring grid locations
   ↓
Avoid obstacles
   ↓
Reach destination
   ↓
Trace the path
   ↓
Final route
```
The obstacles may include:
- Existing wires
- Macros
- Blocked routing regions
- Design-rule restrictions
- Other physical objects

The objective is to find a valid path between the required points.

Maze routing forms the basic concept behind many routing algorithms.

---

## 2. Lee's Algorithm – Maze Routing

Lee's algorithm is a classical grid-based routing algorithm introduced by C. Y. Lee in 1961.

It finds a path between two points by expanding a search wave from the source.

### Working of Lee's Algorithm

#### Step 1 – Start from the Source

The source point is assigned an initial value.

Source = 0

#### Step 2 – Expand to Neighboring Cells

The neighboring available grid locations are assigned increasing values.

0 → 1 → 2 → 3 → 4

The value represents the distance from the source.

#### Step 3 – Continue Expansion

The wave continues to expand through available grid cells.

Blocked cells are not used.

#### Step 4 – Reach the Target

When the search reaches the target, a complete path has been found.

#### Step 5 – Backtrace

The router traces backward through the numbered cells to construct the final route.

### Important Points about Lee's Algorithm

Lee's algorithm:
- Works on a grid
- Starts from a source
- Expands through neighboring locations
- Assigns distance values
- Avoids blocked locations
- Reaches the destination
- Backtraces to obtain the route

### Advantage

It can find a shortest path according to the defined grid and movement rules.

### Limitation

For large VLSI designs, the grid can contain a very large number of locations. Therefore, exploring the grid can require considerable memory and computation.

---

## 3. DRC Clean

After routing, the physical layout must be checked to ensure that it follows the manufacturing rules.

This is done using DRC – Design Rule Check.

DRC checks physical rules such as:
- Minimum metal width
- Minimum spacing between metal wires
- Via dimensions
- Via spacing
- Metal-layer restrictions
- Minimum distances between objects
- Short circuits
- Other technology-specific geometric rules

The purpose is to make sure that the physical layout can satisfy the requirements of the manufacturing technology.

### DRC Flow
```
Detailed Routing
       ↓
DRC Check
       ↓
Violations found?
    ↙        ↘
  Yes         No
   ↓           ↓
Fix them    DRC Clean
```
### DRC Clean

DRC Clean means that no DRC violations were reported by the applicable checking process.

DRC is a verification step, not a routing algorithm.

---

## 4. Power Straps to Standard-Cell Power

<img width="815" height="458" alt="image" src="https://github.com/user-attachments/assets/a350093d-083f-4c07-93ef-c5d559ddcfb4" />


The power distribution network provides a reliable supply of:
- VDD
- GND

### Main Components

- I/O and corner pads
- Power pad connections
- Power ring
- Power straps
- Standard-cell rows
- Standard cells
- Macro cell
- Block halo
- I/O-to-core spacing

### Power Ring

A power ring is a metal structure surrounding the core area.

It helps distribute the power supply around the chip core.

### Power Straps

Power straps are wider metal connections running through the core.

They connect the power ring to the standard-cell rows and other parts of the power network.

General concept:
```
Power Pads
     ↓
Power Ring
     ↓
Power Straps
     ↓
Standard-Cell Rows
     ↓
Standard Cells
```

Both VDD and GND need appropriate power distribution.

### Standard-Cell Rows

Standard cells are normally placed in rows.

Each standard cell requires connections to the power and ground network.

The power straps provide the larger-scale distribution, while the standard-cell power connections provide power to the individual cells.

### Macro Cell

A Macro Cell, such as a RAM block, is a larger physical block than an ordinary standard cell and requires appropriate power connections and spacing.

### Block Halo

A block halo is a reserved region around a macro.

It helps maintain appropriate spacing and prevents standard cells or routing resources from being placed too close to the macro.

---

#  Power Distribution Network and Routing

## 5. Basics of Global and Detailed Routing and Configure TritonRoute

TritonRoute is a detailed-routing engine used in the OpenROAD physical-design flow.

Routing can broadly be divided into:
```
Global Routing
      ↓
Detailed Routing

```

### Global Routing

Global routing determines the approximate path that each net should take.

It does not normally determine the exact location of every wire segment.

Instead, the design is divided into routing regions, and the router determines which regions a net should pass through.

### Main Objectives of Global Routing

- Determine approximate paths
- Consider routing congestion
- Allocate routing resources
- Maintain connectivity
- Generate route guides

Basic flow:
```
Netlist
  ↓
Global Routing
  ↓
Routing Guides
  ↓
Detailed Routing
```
### Route Guides

The output of global or fast routing includes route guides.

A route guide specifies an area or region where a particular net is expected to be routed.

It acts as a guide for the detailed router.
```
Global Routing
      ↓
Route Guides
      ↓
Detailed Routing
```
---

# TritonRoute Features

## 6. TritonRoute – Honors Preprocessed Route Guides

TritonRoute performs initial detailed routing and uses the route guides generated by the earlier routing stage.

It:
- Performs initial detailed routing
- Honors the preprocessed route guides obtained after fast routing
- Attempts to route as much as possible within the route guides
- Assumes that the route guides for each net satisfy inter-guide connectivity
- Uses a proposed MILP-based panel routing scheme
- Uses intra-layer parallel routing and inter-layer sequential routing

### Initial Detailed Routing

TritonRoute takes information from the previous routing stage and creates the actual detailed physical routes.

It determines:
- Exact metal segments
- Exact routing tracks
- Via locations
- Connections to pins
- Connections between layers

### Route-Guide Honoring

The detailed router should not arbitrarily route a net anywhere on the chip.

Instead, it attempts to stay within the regions provided by the route guides.
```
Route Guides
     ↓
Routing Guidance
     ↓
Detailed Route
```

### MILP-Based Panel Routing

MILP means Mixed Integer Linear Programming.

It is an optimization technique where routing decisions can be represented using mathematical variables and constraints.

TritonRoute's methodology uses a panel-based routing framework in which routing problems are handled within smaller regions or panels.

---

## 7. Intra-Layer Parallel and Inter-Layer Sequential Panel Routing

<img width="857" height="256" alt="image" src="https://github.com/user-attachments/assets/25a763a2-b2ad-481c-9666-aef2724225fb" />


### Panel

A panel is a portion of the routing area used to organize the detailed-routing problem.

Instead of treating the entire chip as one huge routing problem, the routing area can be divided into smaller panels.

### Intra-Layer Parallel Routing

Intra-layer means within the same metal layer.

Example:

M2 Panel 1
M2 Panel 2
M2 Panel 3
M2 Panel 4

Multiple suitable panels on the same layer can be routed in parallel.

This is called intra-layer parallel routing.

The objective is to improve routing efficiency by allowing independent routing work to happen simultaneously.

### Inter-Layer Sequential Routing

Inter-layer means routing between different metal layers.

Example:
```
M1
 ↓
Via
 ↓
M2
 ↓
Via
 ↓
M3
```
The routing follows a sequential approach across layers.

General flow:
```
M2 Panels
     ↓
Parallel Routing
     ↓
M3 Panels
     ↓
Parallel Routing
     ↓
M4 Panels
```
Therefore:

Within a layer → Parallel
Between layers → Sequential

---

## 8. Inter-Guide Connectivity

A single net can have multiple route guides.

These guides may exist:
- In different physical regions
- On different metal layers
- Around different obstacles

They cannot simply remain as separate pieces. They need to be connected into one continuous electrical path.

This is called inter-guide connectivity.

Example:
```
M1 Guide
    │
   Via
    ↓
M2 Guide
    │
   Via
    ↓
M3 Guide
```
The detailed router has to create the required wires and vias so that all guide regions belonging to the same net become connected.

---

## 9. TritonRoute Method to Handle Connectivity

TritonRoute uses:
- Access Points (AP)
- Access Point Clusters (APC)

These provide possible physical locations where different routing objects can be connected.

---

## 10. Access Point (AP)

An Access Point is an on-grid point on the metal layer of a route guide that can be used to establish a connection.

An AP can be used to connect to:
- Lower-layer segments
- Upper-layer segments
- Pins
- I/O ports

### Why are Access Points Required?

Suppose a wire on M2 has to connect to a wire on M1.

The router needs to know where the connection between M2 and M1 can legally occur.

An access point provides a possible location.

M2
──────────────
      ● AP
      │
     Via
      │
M1
──────────────

---

## 11. Access Point Cluster (APC)

An Access Point Cluster is a union of access points derived from the same routing object.

An APC may contain access points derived from:
- The same lower-layer segment
- An upper-layer route guide
- A pin
- An I/O port

Conceptually:

AP1
AP2
AP3
AP4
 ↓
APC

The APC represents the possible access locations associated with that routing object.

---

## 12. Types of Connectivity

### (a) Connection to a Lower-Layer Segment

A route guide on one layer needs to connect to a segment on a lower layer.

An appropriate AP is identified and a via can be used.

Upper Layer
──────────────
      │
     Via
      │
Lower Layer
──────────────

### (b) Connection to a Pin Shape

A route needs to connect to a cell pin.

The router identifies an AP associated with the pin shape and connects the routing segment to that point.

Routing segment ───── AP → Pin

### (c) Connection to an Upper Layer

A lower-layer segment needs to connect to an upper-layer guide.

The router identifies a suitable AP and creates the required layer transition.

Upper Layer
──────────────
      │
     Via
      │
Lower Layer
──────────────

---

## 13. Handling Connectivity in TritonRoute

TritonRoute handles connectivity by determining suitable access points and connecting them.

General process:
```
Route Guides
     ↓
Identify Access Points
     ↓
Create Access Point Clusters
     ↓
Determine Possible Connections
     ↓
Connect APs/APCs
     ↓
Insert Required Vias
     ↓
Complete Net Connectivity
```
This ensures that all required parts of a net can be electrically connected.

---

## 14. Routing Topology Algorithm

Routing topology determines how the different APCs should be connected.

Suppose there are several APCs:
```
APC1       APC2
  \         /
   \       /
     APC3
       |
     APC4
```
The router needs an efficient connection structure.

---

## 15. Cost Calculation Between APCs

The algorithm considers pairs of APCs.

The cost can be represented as:

cost(i,j) ← dist(APCi, APCj)

This means that the cost between APC i and APC j is calculated using their distance.

The algorithm considers possible connections between the access-point clusters.

General process:
```
For i = 1 to n-1
    For j = i+1 to n
        Calculate cost between APCi and APCj
```
---

## 16. Minimum Spanning Tree (MST)

After calculating the costs, the algorithm constructs:

MST(APCs, COSTs)

where:
- APCs = Access Point Clusters
- COSTs = Connection costs
- MST = Minimum Spanning Tree

The MST creates a connected topology with minimum total connection cost without unnecessary cycles.

The resulting edges form the selected routing topology.

---

## 17. Why Routing Topology Optimization Is Needed

Routing topology optimization helps the router create efficient connections between APCs.

The optimization considers factors such as:
- Wire length
- Via count
- Connection cost
- Connectivity
- Routing resources

The goal is to obtain a valid and efficient routing structure rather than creating unnecessary connections.

---

## 18. TritonRoute Problem Statement

The TritonRoute problem statement can be divided into Inputs, Output, and Constraints.

### Inputs

#### 1. LEF

Library Exchange Format.

Contains physical/library information such as:
- Cell dimensions
- Pins
- Metal layers
- Via information
- Physical rules

#### 2. DEF

Design Exchange Format.

Contains design-specific physical information such as:
- Placement
- Components
- Nets
- Pins
- Physical design information

#### 3. Preprocessed Route Guides

These are route guides generated by the earlier routing stage.

### Output

The output is a detailed routing solution with optimized wire length and via count.

The final detailed routing contains:
- Metal segments
- Vias
- Net connections

### Constraints

The major constraints are:

#### 1. Route Guide Honoring

The router attempts to follow the preprocessed route guides.

#### 2. Connectivity Constraints

All required parts of a net must be connected.

#### 3. Design Rules

The physical routing must satisfy the technology-specific design rules.

---

# Module 5 Conclusion

Module 5 covered the final physical-design steps of the RTL-to-GDSII flow, including maze routing, Lee's algorithm, Design Rule Check (DRC), power distribution, global routing, and detailed routing using TritonRoute. The module also covered route guides, panel-based routing, intra-layer parallel and inter-layer sequential routing, inter-guide connectivity, access points (AP), access point clusters (APC), and routing topology optimization using cost calculation and Minimum Spanning Tree (MST).

Overall, Module 5 provided an understanding of how physical connections are created, power is distributed, routing connectivity is handled, and the final layout is checked for design-rule compliance before obtaining a clean routed design.




