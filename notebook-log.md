Created for reproducibility homework.
Dataset: Octopodiae rhodopsin gene sequences 
  Downloaded from: GenBank
  Format: FASTA (.fasta)
  Gene: rhodopsin
  Number of sequences: 9
  Type: Assembled gene sequences
  Stage: No QC needed (pre-assembled)
Dataset Description: 
  This dataset contains pre-assembled rhodopsin gene sequences from 9 species of the Octopodiae family of Cephalapods. It will be used to build a phylogenetic tree based on rhodopsin gene sequences. I first read the article "Genus-level phylogeny of cephalopods using molecular markers: current status and problematic areas" (https://pmc.ncbi.nlm.nih.gov/articles/PMC5813590/) and used their phylogenetic tree of Octobrachia to identify the organisms in the Octopediae family. I then input the names of these organisms into GenBank and downloaded the FASTA files of their rhodopsin gene sequences. 
Combining Data: 
  Combined my downloaded FASTA files from GenBank into one file using the following commands 
	ls *.fasta 
	cat *.fasta > combined.fasta
Downloading ClustalW: 
	downloaded: clustalw-2.1-macosx.dmg
	conda install -c bioconda clustalw
Alignment: 
  I aligned my combined.fasta with Clustal W.
	clustalw -ALIGN -INFILE=combined.fasta -OUTFILE=octopediae-aligned.fasta -OUTPUT=FASTA 
	Edited octopediae-aligned.fasta to include species names instead of GenBank identification numbers as sequence headers.
Distance-based Methods: 
  I created a Neighbor-Joining tree using the R software alongside RStudio.
	install.packages("adegenet", dep=TRUE)
	install.packages("phangorn", dep=TRUE)
	library(ape)
	library(adegenet)
	library(phangorn)
	dna <- fasta2DNAbin(file="/Users/lillymager/Desktop/PhyloProject/PhyloSequences/octopediae-aligned.fasta")
	D <- dist.dna(dna, model="TN93")
	tre <- nj(D)
	tre <- ladderize(tre)
	plot(tre, cex=.6)
	title("NJ tree of Octopediae rhodopsin gene")
Parsimony-based Method: 
  I created a maximum parsimony tree using the R software alongside RStudio.
	library(ape)
	library(adegenet)
	library(phangorn)
	dna <- fasta2DNAbin(file="/Users/lillymager/Desktop/PhyloProject/PhyloSequences/octopediae-aligned.fasta")
	dna2 <- as.phyDat(dna)
	tre.ini <- nj(dist.dna(dna,model="raw"))
	plot(tre.pars, cex=0.6)
	title("Parsimony Tree")
Maximum Likelihood: 
  I created a maximum likelihood tree of my data using the IQtree software with bootstrapping, and then plotted the tree using the R software alongside RStudio
	downloaded: iqtree-3.0.0-macOS-arm
	cd iqtree-3.0.0-macOS-arm 	
	ls
	cd bin
	./iqtree3 -s /Users/lillymager/Desktop/PhyloProject/PhyloSequences/octopediae-aligned.fasta -bb 1000 -alrt 1000

	library(ape)
	setwd("~/Desktop/PhyloProject/PhyloSequences")
	tree <- read.tree("octopediae-aligned.fasta.contree")
	plot(tree)
	nodelabels(tree$node.label, frame= "none", cex= 0.8)
	title("Bootstrapped ML tree")
