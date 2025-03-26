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
## Install MAFFT
```
module load gcc
mkdir -p $HOME/software/mafft && cd $HOME/software/mafft
```
```
wget https://mafft.cbrc.jp/alignment/software/mafft-7.490-with-extensions-src.tgz
tar -zxvf mafft-7.490-with-extensions-src.tgz
cd mafft-7.490-with-extensions/core
```
- replace `your-username` in the next code lines:
```
mkdir -p /jet/home/your-username/bin
```

#### Open the vi editor to edit the Makefile
```
vi Makefile
```
- Modify lines 1 and 3 as follows, note that `your-username` must be replaced:
  - Line 1:
    -Change `PREFIX = /usr/local` To `PREFIX = /jet/home/your-username/software/mafft`
  - Line 3:
    -Change `BINDIR = $(PREFIX)/bin` to `BINDIR = /jet/home/your-username/bin`

```
make clean
make
make install
```
- replace `your-username`

```
echo 'export PATH=/jet/home/your-username/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```
- test by typing:
```
mafft --version
```
## **Dataset Description**
### Dataset: Virus Genome Sequences (Just an example, you can also use bacterial genomes, mitochondrial or chloroplast genomes as well)
We will use publicly available genome sequences of **respiratory viruses** or mitochondrial genomes as an example.  
- **Download Source**: NCBI Virus Database ([https://www.ncbi.nlm.nih.gov/labs/virus/](https://www.ncbi.nlm.nih.gov/labs/virus/)).
- Example genomes:
  - Influenza virus.
  - Respiratory syncytial virus (RSV).
  - SARS-CoV-2.
  - Other respiratory viruses of your choice.
- Format: FASTA.

---

## **Tasks and Deliverables**
### **Part 1: Multiple Sequence Alignment**
1. Create a directory for the analysis:
   ```bash
   mkdir -p raxml_tutorial && cd raxml_tutorial
2. Place the downloaded viral genome FASTA files in the directory.
3. Perform multiple sequence alignment using MAFFT:
   ```bash
   mafft --auto --reorder virus_genomes.fasta > aligned_genomes.fasta
- `auto`: Automatically selects the appropriate alignment strategy.
- `reorder`: Reorders sequences for efficient alignment.

### **Part 2a: Prepare Partitioned Alignments**
1. Define genome regions for partitioned analysis (e.g., coding regions, non-coding regions, or individual genes). This information is available from genbank
2. Create a partition file (partitions.txt) specifying regions:
   ```text
   # Example partitions file for RAxML. These numbers are just examples and must be replaced by the coordinates in your chosen virus species.
   DNA, region1 = 1-3000
   DNA, region2 = 3001-6000
   DNA, region3 = 6001-9000
### **Part 2b: Prepare Concatenated Alignment**
1. Concatenated Alignment: Use the full alignment (aligned_genomes.fasta) for the concatenated tree.

### **Part 3: Build Phylogenetic Trees**
1. Partitioned Tree:
- Run RAxML-NG for partitioned analysis:
   ```bash
   raxml-ng --all --msa aligned_genomes.fasta --model GTR+G --partition partitions.txt \
         --prefix partitioned_tree --threads 4
  ```
-`model GTR+G`: General Time Reversible model with Gamma distribution.
-`partition partitions.txt`: Specifies the partitions file.
-`all`: Automates tree searches and bootstrap replicates.
-`prefix`: Output file prefix.

2. Concatenated Tree:
- Run RAxML-NG without partitions:
   ```bash
   raxml-ng --all --msa aligned_genomes.fasta --model GTR+G \
         --prefix concatenated_tree --threads 4

### **Part 4: Visualize and Edit Tree Aesthetics**
1. Open the resulting .newick tree files in FigTree:
   - Example file: partitioned_tree.raxml.bestTree.
2. Customize tree aesthetics:
   - Root the tree: Select an outgroup or midpoint root.
   - Adjust branch colors and widths.
   - Annotate bootstrap values.
3. Export the edited tree as a publication-ready image:
   - File → Export PDF/PNG.

### **Part 5: Interpret Phylogenetic Trees**
1. Compare partitioned and concatenated trees:
   - Are there differences in topology?
   - Do bootstrap values differ significantly between trees?
2. Identify clades and their evolutionary relationships.
3. Discuss the potential influence of partitioning on the results.
