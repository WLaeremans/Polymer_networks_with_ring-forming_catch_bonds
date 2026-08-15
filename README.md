Code used for preprint on arxiv: "Synthetic design of force-responsive hydrogels with ring-forming catch bonds", by Wout Laeremans and Wouter G. Ellenbroek.

# 1_click_chemistry

This folder contains the code used to perform the gelation process, i.e. the formation of a polymer network through click-chemistry reactions between end-functionalized star polymers.

## System Description

The initial configuration consists of two types of star polymers, provided in the files:

- `moleculeA.data`
- `moleculeB.data`

Each star polymer contains:

- 1 central bead
- 4 polymer arms
- 26 beads per arm (excluding the central bead)

For every simulation, 75 stars of each functionality were placed at random positions within the simulation box, resulting in a total of 150 star polymers.

## Atom Types

Both molecule templates have the same molecular architecture but differ in their terminal functional groups.

### moleculeA.data

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

`moleculeB.data` is identical to `moleculeA.data`, except that the terminal functional group has atom type **3** instead of atom type **2**.

### Crosslink Formation

Complementary end groups (types **2** and **3**) react during the click-chemistry protocol. Upon successful bond formation, the reacting end groups are converted to atom type **4**, representing a permanent crosslink between two star polymers.

## Gelation Protocol

The simulation protocol used to generate the gel closely follows:

> C. Raffaelli and W. G. Ellenbroek, *Soft Matter* **17**, 10254-10262 (2021).

Minor implementation details and simulation parameters specific to the present study are provided in the input scripts contained in this directory.

## Contents

- `moleculeA.data` – Star polymer with terminal group type 2.
- `moleculeB.data` – Star polymer with terminal group type 3.
- `lmp_stars.in` – LAMMPS input file used for network formation.

## Output

Executing the scripts in this folder generates three types of output files:

### Crosslink files

These files record the progress of the gelation process by tracking the number of click reactions formed as a function of simulation time. They can be used to monitor the kinetics of network formation and the final extent of reaction.

### Data files

The generated LAMMPS data files contain the complete network topology at the end of the gelation simulation. These files can be analyzed to determine structural properties of the network, such as connectivity and percolation.

### Restart files

LAMMPS restart files are written at the end of the simulation and serve as the starting point for the subsequent stages of the simulation workflow. These files preserve the complete system state, including particle positions, velocities, and network topology.

