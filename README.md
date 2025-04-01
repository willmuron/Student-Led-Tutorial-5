# Student-Led-Tutorial-5
# Task: Phylogenetic Tree Construction Using RAxML
# Date: April 3rd

## **Objective**
Students will:
1. Perform multiple sequence alignment using **MAFFT**.
2. Construct a phylogenetic tree using **RAxML-NG**:
   - Partitioned tree using genome regions.
   - Concatenated tree using the full genome alignment.
3. Visualize and edit tree aesthetics using **FigTree**.
4. Interpret the phylogenetic relationships among the genomes.

---

## **Software and Manuals**
### **Required Software**
1. **MAFFT**: Multiple sequence alignment.
   - [MAFFT Website](https://mafft.cbrc.jp/alignment/software/)
   - [MAFFT Manual](https://mafft.cbrc.jp/alignment/software/manual/)
2. **RAxML-NG**: Maximum likelihood phylogenetic inference.
   - [RAxML-NG GitHub](https://github.com/amkozlov/raxml-ng)
   - [RAxML-NG Manual](https://raxml-ng.vital-it.ch/#/README)
3. **FigTree**: Tree visualization and editing.
   - [FigTree Download](http://tree.bio.ed.ac.uk/software/figtree/)

---
## Install MAFFT (Be patient here, if you accidentally type the commands twice, we end up with all sorts of problems)
```
module load anaconda3
```
```
conda create -n mafft-env -c bioconda mafft
```
```
conda activate mafft-env
```
- test by typing:
```
mafft --version
```
- Output should be `v7.505 (2022/Apr/10)`

## **Dataset Description**
### Dataset: Virus Genome Sequences (Just an example, you can also use bacterial genomes, mitochondrial or chloroplast genomes as well)
We will use publicly available genome sequences of **respiratory viruses** or mitochondrial genomes as an example.  
- **Download Source**: NCBI Virus Database ([https://www.ncbi.nlm.nih.gov/labs/virus/](https://www.ncbi.nlm.nih.gov/labs/virus/)).
- For these tutorial we will focus on the:
  - Influenza virus.
- Format: FASTA.

---

## **Tasks and Deliverables**
### **Part 1: Multiple Sequence Alignment**
1. Create a directory for the analysis:
   ```bash
   myocean
   mkdir -p raxml_tutorial && cd raxml_tutorial
2. Place the downloaded viral genome FASTA files in the directory.
  - Fork repository and clone the repository.
```
git clone github-url
```
  - cd into the newly cloned repository, if you gave your forked repo a new name, the command line below must reflect that change.

```
cd Student-Led-Tutorial-5
```

4. Perform multiple sequence alignment using MAFFT:
   ```bash
   mafft --auto --reorder influenzagenomes_human.fasta > aligned_genomes.fasta
- `auto`: Automatically selects the appropriate alignment strategy.
- `reorder`: Reorders slsequences for efficient alignment.

5. Explore the alignment and discuss any differences with your partner:
```
less aligned_genomes.fasta
```
6. Exit the alignment view by typing `q`
---
# OPTIONAL
### **Part 2a: Prepare Partitioned Alignments (Use this when you have multiple genes but not the full genome)**
- Install and run partition finder

- 1. Define genome regions for partitioned analysis (e.g., coding regions, non-coding regions, or individual genes). This information is available from genbank
- 2. Create a partition file (partitions.txt) specifying regions:
   ``` text
   # Example partitions file for RAxML. These numbers are just examples and must be replaced by the coordinates in your chosen virus species.
   DNA, region1 = 1-3000
   DNA, region2 = 3001-6000
   DNA, region3 = 6001-9000

### **Part 2b: Prepare Concatenated Alignment (Use this for full-lenght genomes)**
1. Concatenated Alignment: Use the full alignment (aligned_genomes.fasta) for the concatenated tree.
---

### **Part 3: Build Phylogenetic Trees**
1. Concatenated Tree:
- Load RAxML
```
module load RAxML/8.2.9
```
- Ask for resources
```
salloc --ntasks=1 --cpus-per-task=5 --mem=10G --time=02:00:00 --job-name=interactive_raxml
```
- Run RAxML:
```
raxmlHPC -T 4 -s aligned_genomes.fasta \
   -n tree \
   -m GTRGAMMA \
   -p 12345 \
   -x 67890 \
   -# 100
```
- Summarize trees and obtain best tree with support values at each node.
```
raxmlHPC -f b \
   -t RAxML_bestTree.tree \
   -z RAxML_bootstrap.tree \
   -m GTRGAMMA \
   -n tree_with_support
```
```
raxmlHPC -T 4 \
  -s aligned_genomes.fasta \
  -n concatenated_tree \
  -m GTRGAMMA \
  -p 12345
```
- `model GTR+G`: General Time Reversible model with Gamma distribution.
- `all`: Automates tree searches and bootstrap replicates.
- `prefix`: Output file prefix.

### **Part 4: Visualize and Edit Tree Aesthetics**
1. Add, commit and push newly generated tree.
2. Open the resulting .newick tree files in FigTree:
   - Example file: covid_concatenated_tree.raxml.bestTree.
3. Customize tree aesthetics:
   - Root the tree: Select an outgroup or midpoint root.
   - Adjust branch colors and widths.
   - Annotate bootstrap values.
4. Export the edited tree as a publication-ready image:
   - File → Export PDF/PNG.

### **Part 5: Interpret Phylogenetic Trees**
1. Clade Structure & Evolutionary Relationships
- What clades are strongly supported? Check for bootstrap values > 70%
- Do clades reflect known virus lineages, strain types, or outbreak clusters?

Are the relationships expected based on known metadata?
(E.g., geography, host species, sampling time?)

2. Tree Shape & Imbalance
Does the tree show long branches leading to some samples?
→ May indicate faster evolution or more mutations.

Are some clades tight and shallow?
→ Possibly recent common ancestry or slower divergence.

3. Support Values
Highlight which branches have low bootstrap support.
→ Discuss what this uncertainty might mean (e.g., not enough divergence, alignment issues, recombination).

4. Biological Hypotheses
What could explain the grouping of certain sequences?

Shared host?

Geographic region?

Temporal sampling?
