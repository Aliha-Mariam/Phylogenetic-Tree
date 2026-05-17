# Phylogenetic Analysis of Kinesin-14 Motor Proteins using Biopython and RAxML-NG

---

#  Introduction

## Biological Background of Kinesin-14 Proteins

Kinesin-14 proteins belong to a specialized subgroup of the kinesin superfamily of ATP-dependent molecular motor proteins. Unlike the majority of kinesins that move toward the plus end of microtubules, Kinesin-14 members primarily exhibit **minus-end directed motility**.

These proteins are critically involved in maintaining proper cellular organization and chromosome dynamics during cell division.

---

##  Functional Importance of Kinesin-14 Proteins

Kinesin-14 proteins participate in several essential cellular mechanisms, including:

- Mitotic spindle assembly and stabilization  
- Chromosome segregation during mitosis  
- Nuclear positioning within the cell  
- Regulation of spindle microtubule organization  
- Control of cell division processes  

Important members of the Kinesin-14 family include:

- **KIFC1** in humans  
- **HSET** in humans  
- **Ncd** in *Drosophila melanogaster*  
- **Kar3** in yeast  

---

##  Structural Characteristics

Kinesin-14 proteins possess several conserved structural regions:

- C-terminal motor domain  
- ATP-binding catalytic region  
- Microtubule-binding interface  
- Coiled-coil domain responsible for dimerization  

These conserved regions are essential for ATP hydrolysis, directional movement, and interaction with microtubules.

---

## Importance of Phylogenetic Analysis

Phylogenetic investigation of Kinesin-14 proteins helps in:

- Understanding evolutionary relationships among homologs  
- Identifying conserved functional residues  
- Investigating divergence of motor activity across species  
- Studying the evolution of minus-end directed motility  
- Exploring lineage-specific adaptations within the kinesin superfamily  

---

#  Objectives

The major objectives of this study were:

- Retrieval of homologous Kinesin-14 protein sequences  
- Identification of homologs using BLASTP  
- Construction of multiple sequence alignment (MSA)  
- Alignment refinement and trimming  
- Removal of redundant sequences  
- Evolutionary model selection using ModelTest-NG  
- Phylogenetic tree construction using RAxML-NG  
- Bootstrap-based validation of phylogenetic topology  
- Reconstruction of ancestral protein sequences  
- Interpretation of evolutionary conservation and divergence patterns  

---

# Materials and Methods

##  Software and Computational Tools

| Tool / Software | Purpose |
|----------------|--------|
| Biopython | Sequence retrieval, parsing, and biological data analysis |
| BLASTP (NCBIWWW) | Homology detection through sequence similarity searching |
| MAFFT | Multiple sequence alignment generation |
| ModelTest-NG | Selection of optimal amino acid substitution model |
| RAxML-NG | Maximum likelihood phylogenetic inference |
| Matplotlib | Visualization of phylogenetic trees |
| Bio.Phylo | Tree parsing and phylogenetic analysis |

---

#  PART 0 — Environment Setup

##  Biopython Installation

```python
!pip install biopython
```

Biopython provides computational tools for:

FASTA sequence handling
NCBI database retrieval
BLAST execution and parsing
Phylogenetic tree analysis

---

# MAFFT Installation

```python

!apt-get install -qq -y mafft
!mafft --help
```

MAFFT is a high-performance alignment tool designed for accurate multiple sequence alignment of protein and nucleotide datasets.

---

# Conda + RAxML-NG + ModelTest-NG Installation

```python
!wget -q https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Linux-x86_64.sh -O /tmp/miniforge.sh
!bash /tmp/miniforge.sh -b -p /usr/local/miniforge
!rm /tmp/miniforge.sh

import os
os.environ["PATH"] = "/usr/local/miniforge/bin:" + os.environ["PATH"]

!conda install -c bioconda raxml-ng modeltest-ng --yes
```

Functional Roles
ModelTest-NG identifies the most suitable evolutionary substitution model
RAxML-NG performs phylogenetic reconstruction using maximum likelihood optimization

---

# PART I — Sequence Retrieval
 # Query Protein Selection

The selected query sequence belongs to the Kinesin-14 family:

PDB ID: 1CZ7_A

# Retrieval of Protein Sequence using Biopython

```python
from Bio import SeqIO, Entrez

Entrez.email = "subhanajamil3@gmail.com"

temp = Entrez.efetch(
    db="protein",
    rettype="fasta",
    id="1CZ7_A"
)

aaseq_out = open("kinesin14_query.fasta",'w')

aaseq = SeqIO.read(temp, format="fasta")

SeqIO.write(aaseq, aaseq_out, "fasta")

temp.close()
aaseq_out.close()
```

The sequence was successfully retrieved from NCBI and stored in FASTA format for downstream analysis.

---

# Sequence Summary

The retrieved dataset confirmed:

Successful extraction of amino acid sequence
Retrieval of accession information
Storage of sequence annotation
Verification of sequence length and integrity

---

# PART II — BLASTP Homology Search

```python
from Bio.Blast import NCBIWWW

result_handle = NCBIWWW.qblast(
    "blastp",
    "pdb",
    aaseq.seq,
    entrez_query='eukaryota[organism]'
)
```

BLASTP was used to identify homologous Kinesin-14 proteins by comparing the query sequence against protein structure databases.

# Important BLAST Parameters
Parameter	Description
E-value	Statistical significance of alignment
Identity	Percentage similarity between sequences
Coverage	Fraction of sequence aligned
Alignment Length	Length of matched alignment region

---

# PART III — Parsing BLAST Results

```python
from Bio.Blast import NCBIXML

blast_record = NCBIXML.read(result_handle)

result_handle.close()
```

BLAST XML output was parsed to extract homologous sequence information for downstream phylogenetic analysis.

---

# PART IV — Inspection of BLAST Hits

The BLAST search identified multiple homologous Kinesin-14 proteins from eukaryotic organisms.

Retrieved information included:

Protein accession IDs
Alignment lengths
Statistical E-values
Sequence similarity metrics

Very low E-values indicated strong evolutionary conservation among homologous proteins.

---

# PART V — Filtering BLAST Results
PIDcut = 1.00

Sequence filtering was performed to:

Eliminate redundant homologs
Remove low-quality matches
Retain biologically meaningful sequences
Improve phylogenetic reconstruction accuracy

---

# PART VI — Retrieval of Homologous Sequences

The top 20 homologous protein sequences were downloaded and stored in:

kinesin14_sequences.fasta

All sequences were retrieved in FASTA format for alignment analysis.

---


# PART VII — Multiple Sequence Alignment using MAFFT
```python
!mafft kinesin14_sequences.fasta > aligned_kinesin14.fasta
```
Alignment Observations

The multiple sequence alignment revealed:

Strong conservation of kinesin motor domains
Conserved ATP-binding motifs across homologs
Increased variability in loop regions
Divergence within terminal sequence regions

---

# PART VIII — Alignment Inspection

The aligned sequences demonstrated extensive structural conservation among Kinesin-14 proteins, particularly within catalytic motor regions.

---

# PART IX — Alignment Trimming

Gap-rich and poorly aligned terminal regions were removed to improve overall alignment quality.

Benefits of Trimming
Reduction of alignment noise
Improved phylogenetic accuracy
Better evolutionary model estimation
Removal of unreliable alignment regions

---

PART X — Removal of Duplicate Sequences

Duplicate sequences were removed using a custom Python-based filtering script.

Importance of Duplicate Removal

Redundant sequences can:

Bias phylogenetic branch lengths
Distort evolutionary relationships
Artificially inflate clade support values

Removing duplicates improves reliability of phylogenetic inference.

---

 # PART XI — Evolutionary Model Selection
 
```python
!modeltest-ng -i clear_aligned_trimmed_kinesin14.fasta -d aa
```
Best-Fit Model
LG + I + G4
Component	Interpretation
LG	Amino acid substitution matrix
I	Invariant sites
G4	Gamma-distributed evolutionary rate variation

This model best represented amino acid substitution patterns observed within Kinesin-14 proteins.

---

# PART XII — Phylogenetic Tree Construction using RAxML-NG
!raxml-ng \
--msa clear_aligned_trimmed_kinesin14.fasta \
--model LG+I+G4 \
--prefix K14_TREE \
--threads 2
Phylogenetic Observations

The resulting phylogenetic tree demonstrated:

Distinct evolutionary clades
Clear separation among homologous proteins
Conservation of motor-domain evolutionary lineages

---

# PART XIII — Tree Visualization

```python
from Bio import Phylo
import matplotlib.pyplot as plt

tree = Phylo.read(
    "K14_SUPPORT.raxml.support",
  
    "newick"
)

fig, ax = plt.subplots(figsize=(12,8))

Phylo.draw(tree, axes=ax)

plt.show()
```

The tree displayed clear branching patterns and evolutionary clustering among Kinesin-14 homologs.

---

# PART XIV — Bootstrap Analysis
```python
!raxml-ng \
--bootstrap \
--msa K14_TREE.raxml.rba \
--model LG+I+G4 \
--prefix K14_BOOT \
--threads 2 \
--bs-tree 100
```
Interpretation of Bootstrap Values
Bootstrap values >70% indicate strong branch support
Lower values indicate uncertain evolutionary relationships

Bootstrap analysis increases confidence in phylogenetic topology.

---

# PART XV — Bootstrap Support Mapping
```python
from Bio import Phylo
import matplotlib.pyplot as plt

tree = Phylo.read(
    "K14_SUPPORT.raxml.support",
    "newick"
)

fig, ax = plt.subplots(figsize=(12,8))

Phylo.draw(tree, axes=ax)

plt.show()
```

Bootstrap-supported trees confirmed reliability of major evolutionary clades.

---

# PART XVI — Ancestral Sequence Reconstruction

```python
!raxml-ng \
--ancestral \
--msa clear_aligned_trimmed_kinesin14.fasta \
--tree K14_TREE.raxml.bestTree \
--model LG+I+G4 \
--prefix K14_ASR
```
Biological Significance

Ancestral sequence reconstruction demonstrated:

Conservation of ATP-binding catalytic residues
Preservation of motor-domain structural architecture
Evolutionary stability of kinesin motor function

---

# PART XVII — Final Tree Visualization
```python
from Bio import Phylo
import matplotlib.pyplot as plt

tree = Phylo.read(
    "K14_ASR.raxml.ancestralTree",
    "newick"
)

fig, ax = plt.subplots(figsize=(12,8))

Phylo.draw(tree, axes=ax)

plt.show()
```

The final ancestral tree revealed strongly supported evolutionary clusters and reliable phylogenetic topology.

# Results and Discussion

The phylogenetic analysis identified multiple distinct evolutionary clades among Kinesin-14 proteins.

Key findings included:

Strong conservation within motor domains
Divergence in coiled-coil and terminal regions
High bootstrap support validating major clades
Evidence supporting a common ancestral origin of Kinesin-14 proteins

The study further demonstrated that functional motor regions remained evolutionarily stable across species, while peripheral regions underwent adaptive diversification.

---

# Conclusion

This study successfully implemented a complete computational phylogenetic workflow for Kinesin-14 motor proteins using Biopython, MAFFT, ModelTest-NG, and RAxML-NG.

The analysis demonstrated:

Strong evolutionary conservation of kinesin motor domains
Clear phylogenetic clustering among homologous proteins
Reliable bootstrap-supported evolutionary relationships
Successful ancestral sequence reconstruction supporting functional conservation throughout kinesin evolution

---
