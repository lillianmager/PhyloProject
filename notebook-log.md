Created for reproducibility homework.
DATASET: Octopodidae rhodopsin gene sequences 
	Downloaded from: GenBank
	Format: FASTA (.fasta)
	Gene: rhodopsin
	Number of sequences: 9
	Type: Assembled gene sequences
	Stage: No QC needed (pre-assembled)
DATASET DESCRIPTION: 
  This dataset contains pre-assembled rhodopsin gene sequences from 9 species of the Octopodidae family of cephalopods. It will be used to build a phylogenetic tree based on rhodopsin gene sequences. I first read the article "Genus-level phylogeny of cephalopods using molecular markers: current status and problematic areas" (https://pmc.ncbi.nlm.nih.gov/articles/PMC5813590/) and used their phylogenetic tree of Octobrachia to identify the organisms in the Octopodidae family. I then input the names of these organisms into GenBank and downloaded the FASTA files of their rhodopsin gene sequences. 

COMBINING DATA:
  Combined my downloaded FASTA files from GenBank into one file using the following commands 
	ls *.fasta 
	cat *.fasta > combined.fasta

REQUIRED SOFTWARE AND PACKAGES:
  In order to reproduce this phylogenetic analysis, ensure the following software and packages are installed. 
  1. ClustalW: 
     I) used for multiple sequence alignment 
     II) any version supporting command-line usage is acceptable 

	from this link: http://www.clustal.org/clustal2/#Download , download: clustalw-2.1-macosx.dmg
	from this link: https://www.anaconda.com/download/success , download anaconda	
	from this link: https://anaconda.org/anaconda/conda , download conda package
	to install ClustalW: conda install -c bioconda clustalw

  2. R and R Packages: 
     I) used for distance and parsimony methods 
     II) RStudio is recommended for download as well

	from this link: https://www.r-project.org/ , download: R-4.4.2-arm64.pkg
	from this link: https://posit.co/downloads/ , download: RStudio-2024.12.1-563.dmg
	to install packages: 
		install.packages("adegenet", dep=TRUE)
		install.packages("phangorn", dep=TRUE)
  3. IQ-Tree:
     I) used for maximum likelihood tree construction with bootstrap support
     II) Version: IQ-Tree 3
	
	from this link: https://iqtree.github.io/#download , download iqtree-3.0.0-macOS-arm
	 
ALIGNMENT:
  I aligned my combined.fasta with Clustal W, an efficient and commonly used method for multiple sequence alignment for further phylogenetic analysis. Through reading the article "CLUSTAL W: improving the sensitivity of progressive multiple sequence alignment through sequence weighting, position-specific gap penalties and weight matrix choice" (https://pmc.ncbi.nlm.nih.gov/articles/PMC308517/), the strengths of this software are that it introduces sequence weighting, position-specific gap penalties, dynamic weight matrices, and uses Neighbor-Joining tree building to improve alignment accuracy. The limitations of ClustalW include: the local minimum problem, errors that occur early in alignment may not be fixed, and it assumes global alignment. The main assumptions of this software include the idea that weight matrices are different at different evolutionary distances, that the optimal range of gap penalties narrows as sequences diverge, and that gaps in protein sequences do not happen with equal chance. Per dataset being aligned, ClustalW requires the user to input one weight matrix and two gap penalties. 
	downloaded: clustalw-2.1-macosx.dmg
	conda install -c bioconda clustalw
	clustalw -ALIGN -INFILE=combined.fasta -OUTFILE= octopodidae-aligned.fasta -OUTPUT=FASTA 
  Edited octopodidae-aligned.fasta to include species names instead of GenBank identification numbers as sequence headers.

DISTANCE-BASED METHOD:
  I created a Neighbor-Joining tree using the R software's ape package alongside RStudio, which determines genetic distances based off a specific model of evolution in order to build the tree. Through lecture and the class website, I know that distance methods assume that genetic distances are accurately calculated using an appropriate evolutionary model, and that the user of the software is required to insert the following parameters: the aligned sequence data, and the chosen model of evolution. One strength of this method is that it is able to produce the best tree without having to search the full tree space, allowing it to be applicable to datasets of any size. Weaknesses of this method include that the optimum tree is only an approximation, some information is lost throughout the process, and the result is very dependent on the chosen evolutionary model. 
	install.packages("adegenet", dep=TRUE)
	install.packages("phangorn", dep=TRUE)
	library(ape)
	library(adegenet)
	library(phangorn)
	dna <- fasta2DNAbin(file="/Users/lillymager/Desktop/PhyloProject/PhyloSequences/octopodidae-aligned.fasta")
	D <- dist.dna(dna, model="TN93")
	tre <- nj(D)
	tre <- ladderize(tre)
	plot(tre, cex=.8)
	title("Neighbor-Joining tree of octopodidae rhodopsin gene")

PARSIMONY-BASED METHOD: 
  I created a maximum parsimony tree using the R software alongside RStudio, which determines how much character change is needed to explain the rhodopsin gene data. Through lecture and the class website, I know that one weakness of this parsimony method is that it has been shown to produce inconsistent trees. However it also has many strengths, including the fact that it does not rely on models of evolution, it includes the minimum amount of evolutionary change required in the tree, and it is a useful method to use when computational limitations interfere with model-based methods. Parsimony based methods assume that there is independence among characters. This software requires the user to input the aligned sequence data in order to start the process. 
	library(ape)
	library(adegenet)
	library(phangorn)
	dna <- fasta2DNAbin(file="/Users/lillymager/Desktop/PhyloProject/PhyloSequences/octopodidae-aligned.fasta")
	dna2 <- as.phyDat(dna)
	tre.ini <- nj(dist.dna(dna,model="raw"))
	plot(tre.pars, cex=0.6)
	title("Parsimony Tree")

MAXIMUM LIKELIHOOD:  
  I created a maximum likelihood tree of my data using the IQ-Tree software with bootstrapping, and then plotted the tree using the R software alongside RStudio. Through reading the articles "IQ-TREE: A Fast and Effective Stochastic Algorithm for Estimating Maximum-Likelihood Phylogenie" (https://academic.oup.com/mbe/article/32/1/268/2925592?login=false) and "IQ-TREE 2: New Models and Efficient Methods for Phylogenetic Inference in the Genomic Era" (https://academic.oup.com/mbe/article/37/5/1530/5721363?login=false), I know that IQ-Tree evaluates aligned sequence data in order to determine the best fitting model of evolution needed to construct a maximum likelihood tree with branch support. Strengths of this software include: using a combination of approaches to escape local optima, systematically accounting for missing data, and the ability to bootstrap. Limitations of this software are that large data sets take longer to process and that results can vary between run, leading to low reproducibility. IQ-Tree has the following main assumptions: that phylogenetic trees are constructed using the maximum likelihood approach, with the best model of evolution having been chosen for the analysis, and that the user will rerun the software multiple times. IQ-Tree requires the user to choose the number of times the data is bootstrapped. 
	downloaded: iqtree-3.0.0-macOS-arm
	cd iqtree-3.0.0-macOS-arm 	
	ls
	cd bin
	./iqtree3 -s /Users/lillymager/Desktop/PhyloProject/PhyloSequences/octopodidae-aligned.fasta -bb 1000 -alrt 1000

	library(ape)
	setwd("~/Desktop/PhyloProject/PhyloSequences")
	tree <- read.tree("octopodidae-aligned.fasta.contree")
	plot(tree)
	nodelabels(tree$node.label, frame= "none", cex= 0.8)
	title("Maximum-likelihood tree of Octopodidae rhodopsin gene")