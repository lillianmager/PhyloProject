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
	Edited octopediae-aligned.fasta to include species names instead of GenBank identification 	numbers as sequence headers.

