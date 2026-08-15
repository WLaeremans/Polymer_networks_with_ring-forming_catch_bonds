Code used for preprint on arxiv: "Synthetic design of force-responsive hydrogels with ring-forming catch bonds", by Wout Laeremans and Wouter G. Ellenbroek.

# 1_click_chemistry

This folder contains the code used to perform the gelation process, i.e. the formation of a polymer network through click-chemistry reactions between end-functionalized star polymers.

## System Description

The initial configuration consists of two types of star polymers, provided in the files:

- `moleculeA.data`
- `moleculeB.data`

These files contain star polymers with different end-group functionalities that can react via click chemistry.

Each star polymer consists of:

- 1 central bead
- 4 polymer arms
- 26 beads per arm (excluding the central bead)

For every simulation, 75 stars of each functionality were placed at random positions within the simulation box, resulting in a total of 150 star polymers.

## Molecular Architecture and Atom Types

The two molecule templates, `moleculeA.data` and `moleculeB.data`, describe four-arm star polymers with identical architectures but different terminal functionalities.

Each star consists of:

- One central bead (atom type **1**).
- Four arms containing **26 beads per arm** (excluding the central bead).
- Backbone beads represented by atom type **6**.

### moleculeA.data

For molecule A, the atom types have the following meaning:

| Atom type | Description |
|-----------|-------------|
| 1 | Central bead |
| 6 | Backbone bead |
| 8 | Reactive bead involved in the click reaction |
| 7 | Auxiliary bead required for the reaction topology |
| 2 | Terminal functional group |

In the visualizations presented in the manuscript:

- Type **6** is shown in **red**.
- Type **8** is shown in **blue**.
- Type **7** is shown in **yellow**.

### moleculeB.data

`moleculeB.data` has the same architecture and atom-type definitions as `moleculeA.data`, with one exception:

- The terminal functional group is atom type **3** instead of atom type **2**.

### Click Reaction

Network formation occurs through reactions between complementary end groups on molecules A and B. When a successful click reaction takes place, the reacting terminal beads are converted into atom type **4**, representing a formed crosslink between two star polymers.

This atom-type conversion is used throughout the simulations to identify reacted and unreacted chain ends and to monitor network formation.

## Gelation Protocol

Network formation is achieved through click-chemistry reactions between complementary end groups. The simulation protocol used to generate the gel closely follows the methodology described in:

> Chiara Raffaelli; Wouter G. Ellenbroek, *Soft Matter* **17** (45), 10254-10262 (2021).

Minor implementation details and simulation parameters specific to the present study are provided in the input scripts contained in this directory.

## Contents

- `moleculeA.data` – Star polymer with functionality A.
- `moleculeB.data` – Star polymer with functionality B.
- `lmp_stars.in` – LAMMPS input file used for network formation.

## Output

Executing the scripts in this folder generates independent crosslinked polymer networks that serve as the initial configurations for the subsequent stages of the simulation workflow.
