---
layout: tutorial_page
permalink: /epidemiology_2018_PGA_1
title: Genomic Epidemiology
header1: Pathogen Genomic Analysis I
header2: SNVPhyl
image: /site_images/CBW_wshop-epidem_map-icon.png
home: https://bioinformaticsdotca.github.io/epidemiology_2018
description: Pathogen Genomic Analysis 1
author: Gary Van Domselaar
modified: July 10th, 2018
---

# Haiti Cholera Dataset
----

The data for this lab is a set of whole genome sequencing data from a number of *V. Cholerae* strains from the [outbreak of cholera in Haiti][haiti-cholera] beginning in 2010 as well as a number of other *V. cholerae* strains included for comparison.  This data was previously published in <http://mbio.asm.org/content/4/4/e00398-13.abstract> and <http://mbio.asm.org/content/2/4/e00157-11.abstract> and is available on NCBI's [Sequence Read Archive](http://www.ncbi.nlm.nih.gov/sra/).  You can view a table of the data at [metadata.tsv][].

Please use the [SNVPhyl][] phylogenomics pipeline to construct a whole genome phylogeny of this data. You may find the input data under `~/CourseData/IDGE_data/module2/VCholerae_SNVPhyl`. The reference file should be `reference/2010EL-1786-Haiti-2010.fasta` and the fastq files should be in `fastq/`. This data was reduced to an average coverage of 12X to speed up execution, so please use a minimum coverage of 4X.  The pipeline can be run with the command `/usr/local/snvphyl-galaxy-cli/bin/snvphyl.py`. This should take ~20 minutes to complete.

```bash
python /usr/local/snvphyl-galaxy-cli/bin/snvphyl.py --deploy-docker --fastq-dir ~/CourseData/IDGE_data/module2/VCholerae_SNVPhyl/fastq/ --reference-file ~/CourseData/IDGE_data/module2/VCholerae_SNVPhyl/reference/2010EL-1786-Haiti-2010.fasta --min-coverage 4 --output-dir ~/workspace/VCholerae_SNVPhyl_output
```

### Command arguments

* `--deploy-docker` launch a [Docker](https://www.docker.com/what-docker) container containing the SNVPhyl software
* `--fastq-dir` the directory to the sequence reads (in fastq format)
* `--reference-file` the reference genome file the reads will be aligned to (in fasta format)
* `--min-coverage` the minimum coverage (number of overlapping reads) needed to identify a variant in the genome
* `--output-dir` the directory to store the output files

When finished, the output files should all be in `~/workspace/VCholerae_SNVPhyl_output`.

## Questions

*Note: Precomputed results are in `~/CourseData/IDGE_data/module2/VCholerae_SNVPhyl/example-output` if needed.*

1. Examine the produced phylogenetic tree in `~/workspace/VCholerae_SNVPhyl_output/phylogeneticTree.newick`. To visualize this tree, you can upload the file to <http://phylo.io/>. You will first need to rename the file to end in *nwk* with the command `mv phylogeneticTree.newick phylogeneticTree.nwk`. You can then download the file by opening a web browser and navigating to <http://XX.oicrcbw.ca>, where XX is your student ID. You can then navigate to `VCholerae_SNVPhyl_output`.

   Given how the genomes group together along with the geographic information and dates, what can you infer about the most likely origin of the introduction of cholera into Haiti?

   For your information, you can find a larger phylogenetic tree in [Figure 1 from "Population Genetics of Vibrio cholerae from Nepal in 2010: Evidence on the Origin of the Haitian Outbreak"][pop-vc-f1].

# Salmonella Heidelberg Dataset
----

The data for this exercise is a set of whole genome sequencing data from a number of *Salmonella heidelberg* strains.    

We will be using the [SNVPhyl][] phylogenomics pipeline to construct a whole genome phylogeny from the read data and we will examine more closely some of the output files SNVPhyl produces. The files required for this exercise are:

* Reference: `~/CourseData/IDGE_data/module2/SHeidelberg_SNVPhyl/reference/S_HeidelbergSL476.fasta` 
* Reads Directory: `~/CourseData/IDGE_data/module2/SHeidelberg_SNVPhyl/fastq`

These read files have been reduced to an average coverage of 10X to speed up execution. As a result, please set the `--min-coverage` parameter to `4` for the SNVPhyl run. Once SNVPhyl has finished running, please answer the following questions.

## Questions

*Note: Precomputed results are in `~/CourseData/IDGE_data/module2/SHeidelberg_SNVPhyl/example-output` if needed.*

1. Examine the output file `filterStats.txt` to see a summary of the number of SNVs identified, and the number removed due to the SNV filtering steps in SNVPhyl. How many SNV sites were used to generate the phylogeny? How many were removed?

2. Examine the table of identified SNVs (`snvTable.tsv`) and take a look at the positions and identified bases which have a status of **filtered-invalid**. Can you think of reasons why these positions were filtered by SNVPhyl?

3. Examine the summary of positions examined by SNVPhyl (`vcf2core.tsv`). Definitions of each column are given [here](http://snvphyl.readthedocs.io/en/latest/user/output/#core-positions). What is the the reference genome length (hint: run `cut -f 1,2 vcf2core.tsv | column -s $'\t' -t`?  What is the **Percentage of all positions that are valid, included, and part of the core genome** for **all** contigs (that is, the percent of the reference genome passing all of SNVPhyl's filters, try running `cut -f 1,7 vcf2core.tsv | column -s $'\t' -t`)? Can you think of reasons why this number is not 100%?

[SNVPhyl]: https://snvphyl.readthedocs.io
[pop-vc-f1]: http://mbio.asm.org/content/2/4/e00157-11/F1.expansion.html
[haiti-cholera]: http://en.wikipedia.org/wiki/2010%E2%80%9313_Haiti_cholera_outbreak
[metadata.tsv]: metadata.tsv

