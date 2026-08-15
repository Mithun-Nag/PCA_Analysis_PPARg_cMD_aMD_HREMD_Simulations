# 📊 Principal Component Analysis of PPARγ cMD, aMD, and HREMD Simulations

This repository contains the trajectory processing, principal component analysis (PCA), and conformational landscape analysis workflows used to compare **conventional molecular dynamics (cMD)**, **accelerated molecular dynamics (aMD)**, and **Hamiltonian replica exchange molecular dynamics (HREMD)** simulations of the PPARγ ligand binding domain.

The simulations include conformations derived from **6ONI**, **6ONJ**, and **3PRG**, with apo, ligand-bound, peptide-bound, and holo conditions where applicable.

## 📁 Repository Structure

```text
.
├── 1_cMD.ipynb
├── 2_aMD.ipynb
├── 3_aMD_cMD.ipynb
├── 4_cMD_aMD_6ONI_HREMD.ipynb
├── 5_cMD_aMD_6ONJ_HREMD.ipynb
├── 6_cMD_aMD_3PRG_HREMD.ipynb
│
└── Additional_Analysis/
    ├── cMD_Analysis.ipynb
    ├── aMD_Analysis.ipynb
    └── aMD_cMD_Analysis.ipynb
```

## 📓 Main Analysis Workflow

### 1️⃣ `1_cMD.ipynb`

Processes the complete **cMD dataset** for PCA analysis.

The workflow:

* extracts **Cα atoms from PPARγ LBD residues 203-477**
* concatenates the three replicate trajectories for each simulation condition
* concatenates the apo, peptide-bound, ligand-bound, and holo conditions for the 6ONI- and 6ONJ-derived systems
* generates global 6ONI and 6ONJ trajectories
* concatenates the **6ONI, 6ONJ, and 3PRG** datasets to generate the complete cMD trajectory
* performs PCA for individual systems, grouped systems, and the complete cMD conformational landscape

### 2️⃣ `2_aMD.ipynb`

Applies the corresponding trajectory processing and PCA workflow to the complete **aMD dataset**.

The workflow generates replicate-combined, system-level, and global aMD trajectories and defines the complete **aMD conformational landscape**.

### 3️⃣ `3_aMD_cMD.ipynb`

Constructs the common **cMD-aMD conformational space**.

The complete cMD trajectory is concatenated with the complete aMD trajectory, followed by PCA of the resulting global trajectory. This provides a single PCA basis for directly comparing conformational sampling between cMD and aMD.

### 4️⃣ `4_cMD_aMD_6ONI_HREMD.ipynb`

Processes the **6ONI HREMD dataset** and integrates it with the global cMD-aMD trajectory.

The workflow includes:

* organization of the **64 demultiplexed HREMD trajectories**
* periodic boundary correction and trajectory centering
* generation of an Amber-compatible protein topology
* conversion of the demultiplexed trajectories to **Amber NetCDF**
* extraction of PPARγ Cα coordinates
* generation of full and **stride-20** HREMD trajectories
* concatenation of the 64 Hamiltonian scaling states
* PCA and PC1-PC2 density analysis of the HREMD ensemble
* concatenation of the downsampled 6ONI HREMD trajectory with the global cMD-aMD trajectory
* recalculation of PCA to define a global **cMD-aMD-6ONI HREMD conformational space**
* visualization of the progression of individual HREMD trajectories through the global PCA landscape

### 5️⃣ `5_cMD_aMD_6ONJ_HREMD.ipynb`

Performs the corresponding HREMD processing and global PCA analysis for the **6ONJ-derived HREMD simulations**, generating a global **cMD-aMD-6ONJ HREMD conformational space**.

### 6️⃣ `6_cMD_aMD_3PRG_HREMD.ipynb`

Performs the corresponding HREMD processing and global PCA analysis for the **3PRG-derived HREMD simulations**, generating a global **cMD-aMD-3PRG HREMD conformational space**.

## 🔬 Additional Analysis

The `Additional_Analysis/` directory contains plotting and secondary analysis notebooks used to characterize and visualize the simulation datasets.

### `cMD_Analysis.ipynb`

Contains structural and conformational analyses of the cMD simulations, including:

* radius of gyration
* backbone RMSD
* per-residue RMSF
* solvent-accessible surface area (SASA)
* replicate-level PCA scree plots
* replicate PC1-PC2 projections with temporal progression
* replicate-combined PCA analyses
* all-system PCA projections
* PC1-PC2 kernel density estimation
* condition-separated PCA visualization

### `aMD_Analysis.ipynb`

Performs the corresponding analyses for the aMD simulations, including:

* radius of gyration
* backbone RMSD
* per-residue RMSF
* SASA distributions
* replicate and combined PCA scree analyses
* PC1-PC2 temporal projections
* all-system PCA density analysis
* condition-separated conformational landscape visualization

### `aMD_cMD_Analysis.ipynb`

Directly compares cMD and aMD sampling within the **global cMD-aMD PCA space**.

The notebook includes:

* PCA scree and cumulative variance analysis
* separate visualization of cMD and aMD sampling
* condition-specific coloring of apo, ligand-bound, peptide-bound, and holo simulations
* PC1-PC2 KDE density analysis of the global conformational landscape

## 🔄 HREMD Downsampling

The HREMD datasets contain substantially more frames than the cMD and aMD datasets. Inclusion of all HREMD frames therefore strongly weighted the global PCA toward the densely sampled HREMD ensemble and obscured the underlying cMD-aMD conformational distribution.

For the global cMD-aMD-HREMD analyses, the HREMD trajectories were therefore downsampled by retaining **every 20th frame**, while all cMD and aMD frames were retained. PCA was then recalculated independently for the 3PRG, 6ONI, and 6ONJ datasets. This preserves the global cMD-aMD landscape while allowing barrier-crossing events sampled by HREMD to be visualized.

## ⚙️ Software and Analysis Tools

The workflows use:

* **Amber 24**
* **AmberTools 25**
* **CPPTRAJ**
* **GROMACS 2023.5**
* **PLUMED 2.9.2**
* **Python**
* **NumPy**
* **SciPy**
* **Pandas**
* **Matplotlib**

Large trajectory processing and PCA calculations were performed using HPC resources and **SLURM**.

## 📊 PCA Analysis

PCA was performed on the **Cα atoms of PPARγ LBD residues 203-477** using CPPTRAJ.

Separate PCA spaces were generated for:

1. complete cMD sampling
2. complete aMD sampling
3. global cMD-aMD sampling
4. global cMD-aMD-6ONI HREMD sampling
5. global cMD-aMD-6ONJ HREMD sampling
6. global cMD-aMD-3PRG HREMD sampling

These analyses provide a common framework for comparing conventional, accelerated, and replica exchange sampling of the PPARγ conformational landscape.

