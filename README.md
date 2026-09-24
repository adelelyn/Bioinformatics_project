# Protein Side-Chain χ1 Conformation Prediction

Deep learning project for predicting protein side-chain conformations from amino acid sequence and structural information.

The project was developed as a coursework project in bioinformatics and explores Transformer-based architectures for predicting the first side-chain dihedral angle **χ1**.

## Project Overview

Protein side-chain conformations play an important role in determining protein structure and function. In this project, the χ1 angle prediction problem is formulated as a **36-class classification task**, where the full angular range is divided into 10° bins.

The main goal of the project was to investigate whether incorporating protein backbone geometry into Transformer-based models improves χ1 prediction compared with a sequence-based baseline.

The final pipeline processes protein structures, constructs local residue windows, extracts sequence and structural features, and uses them as input to Transformer-based neural networks.

## Dataset

Protein structures were collected and processed from the **Protein Data Bank (PDB)**.

The final dataset contains:

* **6,108 protein structures**
* approximately **973,000 residues with defined χ1 angles**
* **36 χ1 classes** corresponding to 10° angular bins
* local sequence windows of **65 residues** around each target residue

Residues without a defined χ1 angle, such as glycine and alanine, are excluded from the prediction loss using a target mask.

Train, validation, and test splits are performed at the protein structure level to prevent windows from the same protein from appearing in different subsets.

## Features

Several groups of features were investigated.

### Sequence features

* amino acid identity
* physicochemical residue properties
* polarity
* hydropathy
* volume
* charge
* chemical groups

### Structural features

* backbone dihedral angles **φ / ψ**
* pairwise **Cα–Cα distance matrices**
* secondary structure information
* padding masks for variable sequence boundaries

The addition of backbone geometry was the main source of improvement over the baseline model.

## Models

Several Transformer-based architectures were implemented and compared during the project.

### Baseline Transformer

The initial model uses a Transformer Encoder to process local amino acid sequence windows and predict the χ1 class of the central residue.

### Nested Transformer

A nested Transformer architecture was explored to capture interactions between different representations of the local protein environment.

### Geometry-aware Transformer

Backbone structural information was added to the model using:

* φ / ψ backbone angles
* Cα–Cα distances
* distance-aware attention

These features allow the model to use not only sequence context but also information about the local three-dimensional geometry of the protein.

## Results

Model performance was evaluated using exact χ1 classification accuracy and tolerance-based accuracy, where predictions within neighboring angular bins are considered correct.

| Model                      | Validation Accuracy (±3 bins) |
| -------------------------- | ----------------------------: |
| Baseline Transformer       |                         ~0.62 |
| Geometry-aware Transformer |                     **~0.72** |

Adding structural and geometric information improved validation accuracy by approximately **10 percentage points** compared with the baseline model.

The best-performing model combines sequence information with backbone angles and Cα distance information.

## Repository Structure

```text
Bioinformatics_project/
│
├── datasets_collections/
│   └── data collection and preprocessing
│
├── model_diagrams/
│   └── model architecture diagrams
│
├── models/
│   └── Transformer-based model experiments
│
└── README.md
```

## Technologies

The project was implemented primarily in Python.

Main tools and libraries:

* Python
* PyTorch
* NumPy
* Pandas
* BioPython
* Jupyter Notebook
* Transformer Encoder architectures
* DSSP / protein structural features

## Experimental Pipeline

The general workflow of the project is:

```text
PDB structures
      ↓
Structure parsing
      ↓
Residue and χ1 extraction
      ↓
Sequence windows (65 residues)
      ↓
Structural
```
