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


# 2_relaxation_with_reactions

This folder contains the code used to activate the reversible ring-opening reactions prior to the tensile test. During this stage, the initially formed network is allowed to relax while undergoing reversible bond exchange reactions.

## Overview

The simulations in this folder start from the crosslinked networks generated in `1_click_chemistry`. Reversible ring-opening reactions are activated, allowing the network topology to evolve and relax before mechanical loading.

The implementation of the reactions relies on the LAMMPS REACTER package.

## Files

The folder contains the following files:

- `lmp_relaxation.in` – Main LAMMPS input script used to perform the relaxation simulations.
- `rxn_on.map` – Reaction mapping file for locality-constrained ("on-chain") reactions.
- `rxn_off.map` – Reaction mapping file for unconstrained reactions.
- `rxn_pre_on.data_template` – Topology template before a locality-constrained reaction.
- `rxn_post_on.data_template` – Topology template after a locality-constrained reaction.
- `rxn_pre_off.data_template` – Topology template before an unconstrained reaction.
- `rxn_post_off.data_template` – Topology template after an unconstrained reaction.

## Reaction Templates and Mapping Files

The reaction mechanism is implemented using the REACTER package.

### Mapping Files

The `.map` files define the correspondence between atoms participating in a reaction. In particular, they identify the reactive groups and specify how atoms in the pre-reaction topology are mapped onto atoms in the post-reaction topology.

Two reaction schemes are considered:

- **On (`on`)**: locality-constrained reactions, referred to as *local reactions* in the manuscript.
- **Off (`off`)**: reactions without a locality constraint.

### Topology Templates

The `.data_template` files define the molecular topology before and after a reaction event.

- Files containing `pre` describe the topology before a reaction takes place.
- Files containing `post` describe the topology after a reaction takes place.

Separate template pairs are provided for locality-constrained (`on`) and unconstrained (`off`) reactions.

## Output

Executing the scripts in this folder generates three types of output files:

### bond_info Files

The `bond_info` files monitor the occurrence of reversible reactions throughout the simulation. The four columns represent:

1. Number of locality-constrained (`on`) reactions.
2. Number of unconstrained (`off`) reactions.
3. Total number of reactions.
4. Simulation time.

These files can be used to quantify the evolution of the network topology during the relaxation process.

### Data Files

LAMMPS data files containing the final network topology after relaxation are generated. These files can be used for network analysis, including:

- Percolation analysis.
- Ring statistics.
- Connectivity analysis.
- Other structural characterization of the network.

### Snapshot Files

Snapshot files containing the relaxed network configurations are generated. These snapshots serve as the input for the final stage of the simulation protocol, namely the tensile tests.


# 3_tensile_test

This folder contains the code used to perform the tensile tests on the relaxed polymer networks. The simulations start from the network configurations generated in `2_relaxation_with_reactions` and evaluate their mechanical response under uniaxial deformation.

## Overview

The tensile-test protocol consists of two consecutive phases:

### 1. Elastic Pre-Deformation

The network is deformed while all reversible ring-opening reactions are disabled. This stage probes the purely elastic response of the material and establishes the initial stress state prior to chemical adaptation.

### 2. Chemically-Driven Creep

After the initial deformation, the reversible ring-opening reactions are activated. The network is then allowed to evolve at fixed deformation, enabling stress relaxation through topology changes induced by the reaction mechanism.

This two-step protocol separates the elastic response from the subsequent chemically driven restructuring of the network.

## Files

The folder contains:

- `lmp_tensile.in` – Main LAMMPS input script used to perform the tensile-test simulations.
- `rxn_on.map` – Reaction mapping file for locality-constrained ("on-chain") reactions.
- `rxn_off.map` – Reaction mapping file for unconstrained reactions.
- `rxn_pre_on.data_template` – Topology template before a locality-constrained reaction.
- `rxn_post_on.data_template` – Topology template after a locality-constrained reaction.
- `rxn_pre_off.data_template` – Topology template before an unconstrained reaction.
- `rxn_post_off.data_template` – Topology template after an unconstrained reaction.

The reaction files are identical in purpose to those described in `2_relaxation_with_reactions` and are used through the LAMMPS REACTER package to implement the reversible ring-opening reactions.

## Output

Executing the scripts in this folder generates the following output files.

### bond_info Files

The `bond_info` files track the occurrence of reversible reactions during the tensile test. The four columns correspond to:

1. Number of locality-constrained (`on`) reactions.
2. Number of unconstrained (`off`) reactions.
3. Total number of reactions.
4. Simulation time.

These files can be used to quantify the extent of network rearrangement during deformation and creep.

### Stress-Strain Data

The simulations also generate `.csv` files containing five columns:

1. Stress.
2. \(L_x\) – Simulation box length in the x direction.
3. \(L_y\) – Simulation box length in the y direction.
4. \(L_z\) – Simulation box length in the z direction.
5. \(L_x L_y L_z\) – Simulation box volume.

These data can be used to analyze the mechanical response of the network.


# 4_analysis

This folder contains the analysis scripts used to characterize the polymer networks and reproduce the figures presented in the manuscript. The scripts process the output generated during the click-chemistry, relaxation, and tensile-test stages of the simulation workflow.

---

## analysis_before_tensile_test.ipynb

This notebook analyzes the network structure prior to the tensile tests and reproduces the corresponding figure presented in the manuscript. The analysis combines data from the network-formation stage (`1_click_chemistry`) and the network-relaxation stage (`2_relaxation_with_reactions`).

### Required Folder Structure

The notebook expects the following directory structure:

```text
data/
├── step_1_formation_by_click_chemistry/
│   ├── cross_links_c7/
│   │   ├── crosslink_c7_s1.txt
│   │   ├── crosslink_c7_s2.txt
│   │   └── ...
│   └── final_data_stars_c7/
│       ├── datatarsnetwork_c7_s1.polymer
│       ├── datatarsnetwork_c7_s2.polymer
│       └── ...
│
└── step2_relaxation_ROP/
    ├── 2bond_info_c7/
    │   ├── bond_info.reacter1.txt
    │   ├── bond_info.reacter2.txt
    │   └── ...
    └── 2final_data_stars_c7/
        ├── data_1.polymer
        ├── data_2.polymer
        └── ...
