# Flourishing Fungi

[![DOI](https://zenodo.org/badge/756989103.svg)](https://zenodo.org/doi/10.5281/zenodo.10655178)

### Bioinformatic Pipeline for Fungi ITS Amplicon Sequences on the Alabama Supercomputer Authority computing cluster ASAX
### Author: Zachary Noel
Date: Feb. 9, 2024

*******Please read and understand all the following steps*******


#### Overview
This bash script serves as a bioinformatic pipeline for processing Fungi ITS amplicon 
sequences. The pipeline includes steps for creating a samples list, stripping primers, 
generating statistics, filtering and trimming, dereplication, clustering, chimera removal, 
taxonomy assignment using SINTAX and NBC, and finally, mapping.

#### Creating a Samples.txt File
The first section of the script generates a samples.txt file by extracting unique sample 
names from the specified directory path ($1). It removes specific substrings such as 
"_R1_001.fastq.gz" and "_R2_001.fastq.gz" from the file names.

#### Stripping Primers
The script trims adapter sequences from the forward fastq files for each sample using the 
cutadapt tool. The primers are removed, and the trimmed sequences are saved in the 
"trimmed" directory.

#### Generating Statistics
This section loads the "vsearch" module, concatenates all trimmed fastq files into one 
file, and uses "vsearch" to generate statistics for the concatenated file. The statistics 
include information such as the number of reads, mean quality scores, etc.

It may be important to evaluate the script after this point since not all sequencing 
runs will produce the same quality reads. So you need to read the stats file and decide 
where to filter. However the default is 264 bp at 1 EE. 

#### Filtering & Trimming
The script filters the concatenated "trimmed.fastq" file using vsearch, applying various 
filtering criteria. The filtered results are saved in both FASTA and FASTQ formats in the 
"filtered" directory.

#### Dereplication, Clustering, Chimera Removal
This section performs dereplication of filtered sequences, denoising, and clustering of 
OTUs based on traditional 97% identity using "usearch" and "vsearch."

#### Taxonomy Assignment - SINTAX
The script assigns taxonomy to clustered OTUs using the SINTAX algorithm and a specified 
reference database. The results are saved in tab-separated format and converted to a CSV 
file for better readability.

#### Taxonomy Assignment - NBC
This section involves executing an R script (dada2_assigntax_NBC.R) for taxonomy 
assignment using the NBC algorithm. The NBC results are combined with SINTAX results to 
create a comprehensive taxonomy file in CSV format.

Please note that the UNITE database will update periodically. Please use the most up to
date database. You will need to downloade the general release Eukaryote database, put it
in the shared directory and edit the script to point it to the right place. 

#### Mapping
The last section of the script loads necessary modules, creates directories for mapping, 
and processes each sample by converting FASTQ to FASTA format. The script aligns 
demultiplexed reads against a reference database, and the results are saved in an OTU 
table (otu_table_ITS_Fungi.txt).

Note: Before running the script, ensure that the required modules and dependencies are 
properly installed and configured. Additionally, adjust paths and parameters as needed 
for your specific setup and datasets.

******************************************************************************************

To run the script, follow these directions: 
		1. copy the archived files in fungal_pipeline.gz to your scratch directory
		2. unzip the files in the archived files fungal_pipeline.gz and edit the file 
		  path on line 32 of the fungal_pipeline.sh to the directory with 
		  your sequences, or unzip the archived files in fungal_pipeline.gz and copy your 
		  sequences to the directory called copy_sequences_here/
		3. run_script fungal_pipeline.sh, request medium node with at least 30gb memory.

***If you want to use a different UNITE database, please edit the path on line 8 of the 
dada2_assigntax_NBC.R script to the correct database. 

***To test the script, you can run it on the sequences in the test/ directory. Just 
change the file path on line 32 of the fungal_pipeline script
