# Exploring Interactive Phylogenies with Auspice (Nextstrain Tutorial)

## Overview

Auspice is an interactive web-based visualization tool developed as part of the Nextstrain project. It is designed to help researchers explore and interpret phylogenetic trees along with associated metadata such as geographic location, sampling time, and genetic mutations. Instead of viewing static evolutionary trees, Auspice allows dynamic exploration of viral evolution in real time. In this tutorial, we analyze a subsample of Zika virus sequences to understand how viral evolution, mutation patterns, and geographic spread can be visualized and interpreted using this platform.

The main strength of Auspice is that it integrates three important biological perspectives into a single interface: evolutionary relationships (phylogeny), spatial movement (map), and genetic variation (diversity panel). This combination allows researchers to study how viruses evolve over time and how they spread across populations and regions.

---

## Interface Layout

When the Zika dataset is opened in Auspice, the screen is divided into three synchronized panels that work together.

<img width="600" height="700" alt="a1" src="https://github.com/user-attachments/assets/112ae4bc-a08f-42ca-9ff6-656ada544e3e" />


The top-left panel displays the phylogenetic tree, which represents evolutionary relationships between viral samples. Each branch in the tree represents a lineage, and the structure of the tree shows how different viral strains are related through common ancestors.

The top-right panel contains a geographic map that shows the locations where each viral sample was collected. This helps in understanding how the virus spreads across different regions and countries over time.

The bottom panel is called the diversity panel. It shows the viral genome structure, including genes and mutation sites. It highlights how genetic variation is distributed across the genome and identifies regions that evolve more rapidly.

All three panels are linked, meaning any interaction in one panel automatically updates the others.

---

## Phylogeny Panel (Evolutionary Tree)

The phylogeny panel is the central component of Auspice. It displays a time-resolved evolutionary tree where the horizontal axis represents time in years. Each sequence is placed according to its sampling date, and if a date is missing, it is estimated during analysis.

The tree is colored based on metadata such as the country of origin. This allows researchers to visually identify geographic clustering of viral strains. For example, sequences from Brazil may appear in one color while those from other countries appear differently.


<img width="596" height="544" alt="a2" src="https://github.com/user-attachments/assets/9c47f80c-0036-4bac-a07f-a4df62223630" />


When hovering over branches, detailed information appears. This includes the number of descendant sequences from that branch, estimated divergence time, and mutations that occurred along that evolutionary path. For example, a mutation like M114V in the NS5 gene indicates that at amino acid position 114, methionine (M) changed to valine (V). This mutation is inherited by all descendant sequences of that branch.

The tooltip also shows confidence intervals for estimated dates, which reflect uncertainty in evolutionary timing. Additionally, the color of the branch reflects the most likely geographic location, although uncertainty may be shown using blended colors.


<img width="588" height="532" alt="a3" src="https://github.com/user-attachments/assets/b9072c6f-f042-46dc-b532-e4d4097cec0f" />


<img width="1227" height="855" alt="a4" src="https://github.com/user-attachments/assets/6bf6003d-d650-4f3f-a719-857e0adca2a4" />



Clicking on a branch allows zooming into a specific clade. When this happens, all other panels (map and diversity) automatically update to reflect only the selected subset of sequences. This makes it easier to study specific evolutionary clusters in detail.

A reset option is available to return to the full tree view.

---

## Control Panel (Tree Customization)

The control panel on the left side provides multiple ways to modify the visualization.

A date range slider allows filtering sequences based on time. When adjusted, only sequences within the selected time range are displayed, and all linked panels update accordingly.

<img width="894" height="540" alt="a5" src="https://github.com/user-attachments/assets/f3a6758e-074a-40f4-ad7d-a910a42138c3" />

The “color by” option allows users to change how the tree is visually categorized. The phylogeny can be colored by country, region, author, sampling date, or genotype. This helps reveal patterns such as geographic clustering or mutation-specific grouping.

The tree layout can also be changed. The rectangular layout is the default view, but radial and unrooted layouts are available for alternative perspectives. These layouts help visualize relationships in different structural formats.

One of the most important visualization modes is the clock layout. In this view, divergence (number of mutations from the common ancestor) is plotted against sampling time. Ideally, sequences follow a near-linear pattern, and the slope of this line represents the evolutionary rate, meaning how quickly mutations accumulate per year.

<img width="591" height="534" alt="a6" src="https://github.com/user-attachments/assets/99b6ddbd-2c83-4e77-ad0f-54cc821b5d42" />


Another important option is branch length representation. In divergence mode, branch length represents the number of mutations rather than time, allowing direct comparison of genetic differences between sequences.

---

## Map Panel (Geographic Spread)

The map panel shows the geographic distribution of viral samples. It updates dynamically based on selections made in the phylogeny panel.

A play button allows users to animate viral spread over time. This animation shows how the virus moves across regions, helping visualize transmission pathways and outbreak progression.

The map resolution can be adjusted to show either fine-scale locations or broader regional groupings. For example, switching to region-level resolution simplifies the view and highlights large-scale movement patterns.

However, it is important to note that geographic reconstructions are based on computational models. These predictions depend heavily on sampling density, available metadata, and evolutionary assumptions. Therefore, while they are useful for visualization, they should not be interpreted as exact transmission routes.

---

## Diversity Panel (Genomic Variation)

The diversity panel provides a genome-wide view of mutations across the viral sequences. It helps identify which parts of the genome evolve quickly and which remain conserved.

<img width="1221" height="300" alt="a7" src="https://github.com/user-attachments/assets/2de9a2c6-6d6b-43ea-9fc1-f83bd0abda8e" />

The top section of the panel shows mutation variability, which can be represented in two ways: entropy and events. Entropy measures how variable a position is across all sequences, while events count the number of mutation occurrences at that position. High entropy indicates strong variation and possible evolutionary pressure.

Users can switch between amino acid (AA) and nucleotide (NT) views. The amino acid view shows changes at the protein level, while the nucleotide view shows changes at the raw genetic level. Typically, the nucleotide view shows more mutations because many nucleotide changes do not alter the amino acid sequence due to genetic code redundancy.

<img width="1221" height="300" alt="a8" src="https://github.com/user-attachments/assets/993265b8-e4a9-4161-ba4e-59625c17bf15" />

Clicking on a mutation site highlights that position and zooms into the corresponding gene region. For example, selecting a high-entropy site may zoom into the NS1 gene and show how mutations are distributed in that region. The phylogenetic tree also updates to reflect variation at that site.

---

## Genotype-Based Analysis

Auspice allows users to color the phylogenetic tree based on specific genetic positions or genes. For example, selecting genotype mode for the NS1 gene enables visualization of amino acid variation at specific positions such as 324. At this site, different sequences may show different amino acids such as arginine (R), tryptophan (W), or glutamine (Q), indicating evolutionary divergence.

<img width="1223" height="303" alt="a10" src="https://github.com/user-attachments/assets/46c015ac-89c8-45ee-b0b3-6ca96a66bd74" />


Similarly, in the NS5 gene, mutations can be observed at positions like 322 and 878. These mutations can be tracked along branches of the phylogenetic tree to understand how they spread across lineages.

<img width="855" height="523" alt="a11" src="https://github.com/user-attachments/assets/3fa8be91-2cec-4109-9257-c4c1ff463d52" />

When switching to nucleotide mode, even more variation becomes visible because multiple nucleotide changes may exist without affecting amino acids.

<img width="597" height="387" alt="a12" src="https://github.com/user-attachments/assets/2c80431c-2b43-4bc8-9a33-38e7891199f7" />

Auspice also supports multi-site genotype coloring. For example, selecting positions such as 3501, 3972, and 6966 simultaneously allows visualization of combined mutation patterns across the genome. This helps in identifying complex evolutionary signatures.

<img width="863" height="533" alt="a13" src="https://github.com/user-attachments/assets/98cdadcc-3720-4b78-835d-3220f4c39824" />

---

## Conclusion

Auspice provides a powerful and integrated platform for studying viral evolution by combining phylogenetic trees, geographic mapping, and genomic diversity analysis into a single interactive system. Through the Zika virus tutorial, we can observe how viruses evolve over time, how mutations accumulate across genomes, and how these changes correlate with geographic spread.

---


The tool not only helps visualize evolutionary relationships but also provides insights into mutation hotspots, evolutionary rates, and transmission dynamics. By linking genetic data with time and location, Auspice plays an important role in modern genomic epidemiology and helps researchers better understand the behavior of emerging viral outbreaks.
