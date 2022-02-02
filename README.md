# Readying 1000G phase5 v3 in Computerome (C2)

A code to obtain the 1000G reference data that I use for my projects in the CBMR

## Introduction to the data

The data that I am using can only be found in the Porus/Hulk servers and can only be accessed by people in NNF's CBMR. While, the 1000G data is publically available, the data that I am using is part of the Phenomics Platform from our center, hence why I cannot share it. 

Importantly. this data is characterized by: 

1) Having all variants including insertions/delections and MAF <0.01.
2) However it does not contain simpletons. 
3) Contains individuals of all ancestries. 

In this pipeline we will do the following:

a) Index the original vcf files so that we can convert them to plink files.
b) Convert them into plink files removing variants with maf <0.01 and geno < 0.1 (only using individuals of European ancestry).
c) Removing duplicates  

**IMPORTANTLY: you need your own path to where this repository is downloaded. In the code you will find ~/RAW_DATA or ~/CURATED_DATA. Please replace ~ with the path that you are going to work in.**

### STEP A. Obtaining the data and indexing it.

First we need to obtain the data by doing a symbolic link in the folder where we are going to save the 1000G data. (CODE in Step_1_linking)
Then, we index it using bcftools -tbi (full code in Step_2_indexing)

### STEP B. Converting into plink 1.9 files excluding European individuals.

To generate the plink files for only Europeans, we need a list of the European individuals. To do so we first need to download two files: igsr_populations.tsv and igsr_samples.tsv from https://www.internationalgenome.org/data-portal/population (you can find both of them in the RAW DATA folder in this github.)

You can generate the file eur_samples.only.txt if you follow the code in Step3_Generate_European_individuals_list, or you can download from the CURATED_DATA folder in this github.

With all this data you can transform the vcf files to plink1.9 format (bed/bim/fam) with only the european individuals and with the variants that pass the filters specificied above using the code in Step4_vcf_2_plink.

### STEP C. Cleaning data from duplicates

The vcf data has duplicated rows (literal duplicates, not talking about multi-allelic SNPs.) 
Cleaning the data is a bit of hassle, since remove-duplicates from plink would remove the multi-allelic SNPs, which we want to retain.
The code in Step5_cleaning_duplicates, takes the data in RAW_DATA/PLINK_DATA folder and cleans the duplicates. Be wary with the paths!!
