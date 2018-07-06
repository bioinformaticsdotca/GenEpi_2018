---
layout: tutorial_page
permalink: /genomic_epidemiology_2017_PGA_1
title: Genomic Epidemiology
header1: Pathogen Genomic Analysis I
header2: SNVPhyl
image: /site_images/CBW_wshop-epidem_map-icon.png
home: https://bioinformaticsdotca.github.io/genomic_epidemiology_2017
description: Pathogen Genomic Analysis 1
author: Gary Van Domselaar
modified: May 1st, 2017
---


# Workstation Setup
----
Before running the SNVPhyl pipeline, we will have to make sure that the Docker daemon is running on your machine.  To start the Docker service, please enter the following command:

`sudo systemctl start docker` 



# Salmonella Heidelberg Dataset
----
The data for this exercise is a set of whole genome sequencing data from a number of *Salmonella heidelberg* strains.    

We will be using the [SNVPhyl][] phylogenomics pipeline to construct a whole genome phylogeny from the raw read data and a reference genome.  The files required for this exercise are:

Reference: `~/CourseData/IDGE_data/Sheidelberg-snvphyl/S_HeidelbergSL476.fasta` 
Reads Directory: `~/CourseData/IDGE_data/Sheidelberg-snvphyl/fastq`

To speed up the execution time, these read files have been reduced to an average coverage of 10X.  As a result, please set the `--min-coverage` parameter to `4` for the SNVPhyl run. 

To run the pipeline, we will execute the main pipeline script at `/usr/local/snvphyl-galaxy-cli/bin/snvphyl.py` using the `--deploy-docker` flag to launch the dockerized version of the pipeline.  The command should take the following format:

`python /usr/local/snvphyl-galaxy-cli/bin/snvphyl.py --deploy-docker --fastq-dir [FASTQ_DIR] --reference-file [REFERENCE_FILE_LOCATION] --min-coverage 4 --output-dir /mnt/workspace/[OUTPUT_DIR_NAME]`

The output from the pipeline can be found under ~/workspace/[OUTPUT_DIR_NAME].  

1.  Examine the output file `filterStats.txt` to see a summary of the number of SNVs identified, and the number removed due to the SNV filtering steps in SNVPhyl. How many SNV sites were used to generate the phylogeny? How many were removed?

2. Examine the table of identified SNVs (`snvTable.tsv`) and take a look at the positions and identified bases which have a status of **filtered-invalid**. Can you think of reasons why these positions were filtered by SNVPhyl?

3.  SNVPhyl requires that at least 75% (`snv-abundance-ratio`) of the reads mapping as variants to a position on the isolate genome are in agreement before considering the position as a high quality single nucleotide variant.  Can you think of why this requirement makes the pipeline more robust?

[SNVPhyl]: https://snvphyl.readthedocs.io




# Haiti Cholera Dataset

The data for this lab is a set of whole genome sequencing data from a number of *V. Cholerae* strains from the [outbreak of cholera in Haiti][haiti-cholera] beginning in 2010 as well as a number of other *V. cholerae* strains included for comparison.  This data was previously published in <http://mbio.asm.org/content/4/4/e00398-13.abstract> and <http://mbio.asm.org/content/2/4/e00157-11.abstract> and is available on NCBI's [Sequence Read Archive](http://www.ncbi.nlm.nih.gov/sra/).  You can view a table of the data at [metadata.tsv][].

Please use the [SNVPhyl][] phylogenomics pipeline to construct a whole genome phylogeny of this data. You may find the input data under `~/CourseData/IDGE_data/VCholerae_SNVPhyl`. The reference file should be `reference/2010EL-1786.fasta` and the fastq files should be in `fastq/`. This data was reduced to an average coverage of 12X to speed up execution, so please use a minimum coverage of 4X.  The pipeline can be run with the command `/usr/local/snvphyl-galaxy-cli/bin/snvphyl.py`. This should take ~16 minutes to complete.

1. Examine the output file `vcf2core.tsv` for the percentage of the reference genome with sufficient read coverage and quality to be used by SNVPhyl to produce the phylogenetic tree (**Percentage of all positions that are valid, included, and part of the core genome**). Typically, values from 80-90%+ indiciate that all isolates share in common a sufficient proporition of genomic content (core genome) which do not exist in repetitive or other problematic regions. Was a sufficient proportion used for this dataset?

   *Note: You can print file contents with `cat`, but this leaves columns unaligned. To print with properly aligned columns you can use the command `column`, and you can use the command `cut` to slice out particular columns from a tabular text file. For example, `cut -f 1,2,7 [filename.tsv] | column -s  $'\t' -t`.*

2.  The `mappingQuality.txt` file lists any strains that map to less than 80% of the reference genome and is a good reference to quickly identify any "trouble" isolates that may be effecting the quality of your run.  Can you think of why two strains, each mapping to 80% of the reference genome, could potentially eliminate up to 40% of the genome from the core?    

3. Examine the output file `filterStats.txt` to see a summary of the number of SNVs identified, and the number removed due to the SNV filtering steps in SNVPhyl. How many SNV sites were used to generate the phylogeny? How many were removed?


4. Using the phylogenetic tree depicited in [Figure 1 from "Population Genetics of Vibrio cholerae from Nepal in 2010: Evidence on the Origin of the Haitian Outbreak"][pop-vc-f1].

   1. Compare the phylogenetic tree generated using SNVPhyl with the above phylogenetic tree. Are the isolates grouped in a similar way?

      *Note: Isolates labeled as __VC-XX__ or __2010EL-XXXX__ in the data for this lab are missing __VC__ and __2010EL__ in this published phylogenetic tree (e.g., VC-25 becomes 25). Some isolates in the data presented for this lab are not present in this particular published phylogenetic tree.*

   2. Compare the SNV distance matrix generated by SNVPhyl (`snvMatrix.tsv`) to the SNV (SNP) counts show in red on **Figure 1**. In particular compare the **reference (1786)** to **VC-6, VC-19, VC-26, VC-18, VC-25**.  In Figure 1, to get the total SNV count between **1786**and the other isolates, add up all the red numbers on path connecting 1786 to the other isolates.  For example, between **1786** and **18** there are *7 + 7 + 1 = 15 SNVs*. Are there any numbers that do not match? Can you think of a reason for this?

   3. Are the results you generated using SNVPhyl consistent with those of the above paper, that is that the Haitian and Nepalse isolates (particularly **VC-25** and **VC-26**) share a very recent common ancestor?

[pop-vc-f1]: http://mbio.asm.org/content/2/4/e00157-11/F1.expansion.html
[haiti-cholera]: http://en.wikipedia.org/wiki/2010%E2%80%9313_Haiti_cholera_outbreak
[metadata.tsv]: metadata.tsv
[SNVPhyl]: https://snvphyl.readthedocs.io

