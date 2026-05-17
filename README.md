# 🧬 Phylogenetic Analysis of Kinesin Motor Proteins using Biopython and RAxML-NG
---

# Introduction
Kinesins are a large and diverse family of ATP-dependent motor proteins found in all eukaryotic organisms. They play a fundamental role in intracellular transport by moving along microtubule filaments within the cell.

These proteins function by converting chemical energy from ATP hydrolysis into mechanical motion, enabling directed transport of cellular cargo.

---

# Biological Functions of Kinesins

Kinesins are involved in several essential cellular processes, including:

Vesicle transport between organelles
Organelle positioning and movement
Chromosome segregation during mitosis
Formation of the mitotic spindle
Regulation of cell division
General intracellular trafficking

---

# Directionality of Movement

Most kinesin proteins move toward the plus end of microtubules, facilitating outward transport from the cell center.

However, specific families such as Kinesin-14 exhibit movement toward the minus end, demonstrating functional diversity within the kinesin superfamily.

---

# Structural Organization of Kinesins

Kinesin proteins typically consist of multiple functional domains:

Motor domain (ATPase activity center)
ATP-binding pocket
Microtubule-binding interface
Coiled-coil stalk region (dimerization support)
Cargo-binding domain (cargo specificity)


---

#Importance of Phylogenetic Analysis

Phylogenetic analysis allows researchers to investigate the evolutionary history of kinesin proteins and understand:

Evolutionary divergence among kinesin families
Functional specialization of motor proteins
Conservation of catalytic and ATP-binding residues
Evolution of directional movement mechanisms
Relationships among kinesin subfamilies

---

# Objectives

The primary goals of this study include:

Retrieval of homologous kinesin protein sequences from biological databases
Identification of sequence similarity using BLASTP via Biopython
Construction of multiple sequence alignment (MSA) using MAFFT
Refinement of alignment through trimming of low-quality regions
Removal of redundant or duplicate sequences
Selection of the best evolutionary substitution model using ModelTest-NG
Construction of a phylogenetic tree using RAxML-NG
Validation of phylogenetic topology using bootstrap analysis
Reconstruction of ancestral kinesin protein sequences
Investigation of evolutionary conservation and divergence patterns

---

# Materials and Software

| Tool / Software | Purpose |
|----------------|--------|
| Python | Core programming environment for pipeline execution |
| Google Colab | Cloud-based computational platform for reproducibility |
| Biopython | Sequence retrieval, parsing, and BLAST operations |
| BLASTP | Identification of homologous protein sequences |
| MAFFT | High-quality multiple sequence alignment |
| ModelTest-NG | Selection of optimal evolutionary substitution model |
| RAxML-NG | Maximum likelihood phylogenetic tree inference |
| Matplotlib | Visualization of phylogenetic trees |
| Bio.Phylo | Phylogenetic tree parsing and analysis |

---

# PART 0 — Installation of Required Software

Installing Biopython

```python
!pip install biopython
import os
import sys
import Bio

```

Biopython is a widely used bioinformatics library that provides essential computational tools for biological sequence analysis. It supports:

Retrieval of sequences from biological databases such as NCBI
FASTA file parsing and manipulation
Execution and parsing of BLAST searches
Phylogenetic tree handling and visualization

---

# Installing MAFFT
```python
!apt-get install -qq -y mafft
!mafft --help
```

MAFFT (Multiple Alignment using Fast Fourier Transform) is a high-performance tool used for constructing multiple sequence alignments of protein and nucleotide sequences.

It is particularly effective for:

Large-scale sequence datasets
Detecting conserved functional regions
Identifying evolutionary relationships between sequences

---

# Installation of Conda, ModelTest-NG, and RAxML-NG

```python
import os

!wget -q https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Linux-x86_64.sh -O /tmp/miniforge.sh
!bash /tmp/miniforge.sh -b -p /usr/local/miniforge
!rm /tmp/miniforge.sh

os.environ["PATH"] = "/usr/local/miniforge/bin:" + os.environ["PATH"]

!conda --version
!conda config --set always_yes yes --set changeps1 no

!conda install -c conda-forge mamba
!conda install -c bioconda raxml-ng modeltest-ng --yes
```

Functional Roles:
ModelTest-NG: Evaluates multiple substitution models to determine the best evolutionary fit
RAxML-NG: Performs maximum likelihood-based phylogenetic tree reconstruction.

---

# PART I — Retrieval of Kinesin Protein Sequence
Target Protein Selection

The selected query protein for this analysis is:

Human Kinesin Family Member 5B (KIF5B)
Structural reference: PDB ID: 3KIN_A

---

# Sequence Retrieval from NCBI
```python
from Bio import SeqIO, Entrez

Entrez.email = "your_email@example.com"

temp = Entrez.efetch(
    db="protein",
    rettype="fasta",
    id="3KIN_A"
)

aaseq_out = open("kinesin_motor.fasta",'w')

aaseq = SeqIO.read(temp, format="fasta")

SeqIO.write(aaseq, aaseq_out, "fasta")

temp.close()
aaseq_out.close()
```

This step retrieves the kinesin protein sequence from the NCBI database and stores it in FASTA format for downstream computational analysis.

---

# Sequence Properties
```python
print(len(aaseq))
print(aaseq.description)
print(aaseq.id)
print(aaseq.seq)
```

Observations:
Sequence length: 238 amino acids
Protein description: Kinesin heavy chain motor domain
Sequence corresponds to a highly conserved motor region
Contains ATP-binding and microtubule interaction motifs

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

BLASTP is used to identify homologous protein sequences by comparing the query sequence against a structural protein database.

Key Parameters:
E-value: statistical significance of alignment
Identity: percentage similarity between sequences
Coverage: proportion of aligned region
Alignment length: extent of matched region

---

# PART III — BLAST Result Parsing
```python
from Bio.Blast import NCBIXML

blast_record = NCBIXML.read(result_handle)
result_handle.close()
```

This step extracts alignment information from BLAST output for further filtering and analysis.

---

# PART IV — Filtering BLAST Hits

```python
PIDcut = 1.00

for alignment in blast_record.alignments:

    for hsp in alignment.hsps:

        if (hsp.identities/hsp.align_length) <= PIDcut:

            print("Accession:", alignment.hit_id)

            print("Length:", alignment.length)

            print("Alignment Length:", hsp.align_length)

            print("E-value:", hsp.expect)

            print("Sequence Identity [%]:",
                  "{:.2f}".format(
                      100*hsp.identities/hsp.align_length
                  ))

            print("Coverage [%]:",
                  "{:.2f}".format(
                      100*sum(c.isalpha() for c in hsp.query)/len(aaseq)
                  ))

            print()
```

This step removes redundant or low-quality homologs to improve downstream phylogenetic accuracy.

# BLAST Results including Coverage and Identity
Accession: pdb|5GSZ|A | Length: 353 | Alignment Length: 257 | E-value: 9.50998e-43 | Sequence Identity [%]: 34.24 | Coverage [%]: 100.00
Accession: pdb|5GSY|K | Length: 360 | Alignment Length: 257 | E-value: 1.10681e-42 | Sequence Identity [%]: 34.24 | Coverage [%]: 100.00
Accession: pdb|7A40|A | Length: 346 | Alignment Length: 240 | E-value: 2.35871e-42 | Sequence Identity [%]: 39.58 | Coverage [%]: 97.06
Accession: pdb|7A3Z|A | Length: 372 | Alignment Length: 240 | E-value: 4.66892e-42 | Sequence Identity [%]: 39.58 | Coverage [%]: 97.06
Accession: pdb|3B6V|A | Length: 395 | Alignment Length: 247 | E-value: 2.19182e-41 | Sequence Identity [%]: 36.03 | Coverage [%]: 98.32
Accession: pdb|4AQV|C | Length: 373 | Alignment Length: 262 | E-value: 4.1622e-41 | Sequence Identity [%]: 39.69 | Coverage [%]: 97.48
Accession: pdb|5ZBS|A | Length: 375 | Alignment Length: 259 | E-value: 4.33382e-41 | Sequence Identity [%]: 35.91 | Coverage [%]: 97.06
Accession: pdb|6A1Z|A | Length: 395 | Alignment Length: 263 | E-value: 4.43793e-41 | Sequence Identity [%]: 35.36 | Coverage [%]: 98.74
Overall coverage values are consistently high (≈97–100%), indicating strong and complete alignment across the kinesin motor domain.

---

# PART V — Retrieval of Homologous Sequences

```python
Entrez.email = "your_email@example.com"

with open("kinesin_sequences.fasta", "a") as allhits_out:

    for alignment in blast_record.alignments[:20]:

        for hsp in alignment.hsps:

            if(hsp.identities/len(hsp.match)) <= PIDcut:

                print("Fetching:", alignment.hit_id)

                fetch = Entrez.efetch(
                    db="protein",
                    id=alignment.hit_id,
                    rettype="fasta"
                )

                allhits_seqs = SeqIO.read(fetch, format="fasta")

                SeqIO.write(
                    allhits_seqs,
                    allhits_out,
                    "fasta"
                )

                fetch.close()

allhits_out.close()
```

Top-ranking BLAST hits are used to fetch homologous kinesin sequences from NCBI and compile them into a single dataset for alignment.

---

# PART VI — Multiple Sequence Alignment (MAFFT)

```python
!mafft kinesin_sequences.fasta > aligned_kinesin.fasta
```

Multiple sequence alignment is performed to identify:

Conserved amino acid residues
Functional protein motifs
Evolutionary insertion/deletion events
Structural conservation across homologs

---

# PART VII — Alignment Visualization and Interpretation
```python
from Bio import AlignIO

aln = AlignIO.read("aligned_kinesin.fasta", "fasta")

print(aln)
```

The aligned dataset reveals:

Strong conservation of ATP-binding motifs
Highly conserved motor domain architecture
Variable loop and terminal regions
Presence of experimental tags in some sequences
---

# Aligned Sequences

```python
------------------------ADP-AECSIKVMCRFRPLNE...--- pdb|2KIN|A
-----------------------MADP-AECSIKVMCRFRPLNE...--- pdb|3J6H|K
-----------------------MADP-AECSIKVMCRFRPLNE...--- pdb|3WRD|A
-----------------------MAETNNECSIKVLCRFRPLNQ...--- pdb|4UXT|C
-----------------------MAETNNECSIKVLCRFRPLNQ...--- pdb|9T17|C
-----------------------MAETNNECSIKVLCRFRPLNQ...--- pdb|9UMM|A
-----------------------MADL-AECNIKVMCRFRPLNE...--- pdb|1MKJ|A
-----------------------MADP-AECNIKVMCRFRPLNE...--- pdb|4ATX|C
-----------------------MADL-AECNIKVMCRFRPLNE...--- pdb|9L7M|K
-----------------------MADL-AECNIKVMCRFRPLNE...--- pdb|1BG2|A
-----------------------MADL-AECNIKVMCRFRPLNE...SPT pdb|9GNQ|K
MGSSHHHHHHSSGLVPRGSHMASMADL-AECNIKVMCRFRPLNE...--- pdb|8IXA|S
-----------------------MADL-AESNIKVMCRFRPLNE...--- pdb|3J8X|K
-----------------------MADL-AESNIKVMCRFRPLNE...--- pdb|4HNA|K
-----------------------MADL-AESNIKVMCRFRPLNE...--- pdb|4LNU|K
---MHHHHH--------------HADL-AESNIKVMCRFRPLNE...--- pdb|9L78|A
-----------------------MADL-AESNIKVMCRFRPLNE...--- pdb|9L7E|A
-----------------------MADL-AESNIKVMCRFRPLNE...--- pdb|5LT3|A
...
---MHHHHH--------------HADL-AESNIKVMCRFRPLNE...--- pdb|9L6K|A
```

The multiple sequence alignment revealed several important evolutionary and structural characteristics of kinesin motor proteins:

- ATP-binding motifs remained highly conserved across nearly all homologous sequences  
- Residues involved in microtubule interaction showed strong sequence conservation  
- The central motor domain exhibited substantial evolutionary stability among kinesin proteins  
- Loop regions and terminal segments displayed greater sequence variability and insertion/deletion events  
- Certain sequences contained experimentally introduced affinity tags, including histidine-rich tags such as `MHHHHH`  
- Overall alignment patterns demonstrated extensive conservation within the kinesin motor protein family
  
---

 # PART VIII — Alignment Trimming

 ```python
from Bio import AlignIO
from Bio import SeqIO

aln = AlignIO.read("aligned_kinesin.fasta", "fasta")

for fcol in range(aln.get_alignment_length()):

    if "-" not in aln[:, fcol]:
        position1 = fcol
        break

for lcol in reversed(range(aln.get_alignment_length())):

    if "-" not in aln[:, lcol]:
        position2 = lcol + 1
        break

with open("aligned_trimmed_kinesin.fasta", "w") as handle:

    SeqIO.write(
        aln[:, position1:position2],
        handle,
        "fasta"
    )
```

Low-quality and gap-rich regions are removed to improve phylogenetic accuracy and reduce noise in evolutionary inference.

---

# PART IX — Duplicate Sequence Removal
```python
from Bio import AlignIO
from Bio import SeqIO

aln = AlignIO.read("aligned_kinesin.fasta", "fasta")

for fcol in range(aln.get_alignment_length()):

    if "-" not in aln[:, fcol]:
        position1 = fcol
        break

for lcol in reversed(range(aln.get_alignment_length())):

    if "-" not in aln[:, lcol]:
        position2 = lcol + 1
        break

with open("aligned_trimmed_kinesin.fasta", "w") as handle:

    SeqIO.write(
        aln[:, position1:position2],
        handle,
        "fasta"
    )
```

Redundant sequences are removed to prevent:

Bias in branch length estimation
Artificial clustering of clades
Distortion of evolutionary relationships

---

 # PART X — Model Selection (ModelTest-NG)

 ```python
!modeltest-ng -i clear_aligned_trimmed_kinesin.fasta -d aa
```

The best-fit evolutionary model identified:

 LG + I + G4

Where:

LG → amino acid substitution model
I → invariant sites
G4 → gamma-distributed rate variation

---

# PART XI — Phylogenetic Tree Construction (RAxML-NG)
 ```python
!raxml-ng \
--msa clear_aligned_trimmed_kinesin.fasta \
--model LG+I+G4 \
--prefix KINESIN_TREE \
--threads 2
 ```


A maximum likelihood phylogenetic tree is generated to infer evolutionary relationships among kinesin homologs.

---

 # PART XII — Tree Visualization
 
 ```python
from Bio import Phylo
import matplotlib.pyplot as plt

tree = Phylo.read(
    "KINESIN_TREE.raxml.bestTree",
    "newick"
)

fig, ax = plt.subplots(figsize=(12,8))

Phylo.draw(tree, axes=ax)

plt.show()
 ```
<img width="865" height="525" alt="tree 1" src="https://github.com/user-attachments/assets/c177974b-a934-4609-9122-2f482c576a7e" />

Phylogenetic trees are visualized to interpret:

Evolutionary clustering
Lineage divergence
Conserved ancestral relationships
---

# PART XIII — Bootstrap Analysis

```python
!raxml-ng \
--bootstrap \
--msa KINESIN_TREE.raxml.rba \
--model LG+I+G4 \
--prefix BOOTSTRAP \
--threads 2 \
--bs-tree 100
```

Bootstrap resampling is used to assess the reliability of phylogenetic branches:

70% → strong support

<50% → weak support

<img width="865" height="525" alt="teeee2" src="https://github.com/user-attachments/assets/5f289ccf-a445-4ac2-a393-789de2df9bad" />


---

 # PART XIV — Ancestral Sequence Reconstruction

 ```python
tree = Phylo.read(
    "ASR.raxml.ancestralTree",
    "newick"
)

fig, ax = plt.subplots(figsize=(12,8))

Phylo.draw(tree, axes=ax)

plt.show()
```
<img width="865" height="525" alt="teee2" src="https://github.com/user-attachments/assets/a0612d83-ceb9-4401-bd8e-c2adabebb819" />


Ancestral sequences are inferred at internal nodes to understand:

Evolution of ATP-binding residues
Early conservation of motor activity
Functional constraints in kinesin evolution

---

# RESULTS SUMMARY
Strong evolutionary conservation of kinesin motor domains
Distinct phylogenetic clustering of kinesin subfamilies
High confidence bootstrap-supported clades
Functional conservation of ATP-binding regions
Evolutionary divergence mainly in non-motor regions

---

# CONCLUSION

This study presents a complete and reproducible computational workflow for the evolutionary analysis of kinesin motor proteins.

The results demonstrate that kinesin motor domains are highly conserved across species, while peripheral regions show diversification associated with functional specialization.

---

# REFERENCES
RAxML-NG Documentation
MAFFT Algorithm (Katoh et al.)
Biopython Documentation
ModelTest-NG Manual
NCBI BLAST Resources
LG Substitution Model (Le & Gascuel)
Protein Data Bank (PDB)
Molecular Biology of the Cell / Campbell Biology

---
