# Chemical Genomics demo

## Introduction
This repository contains demo data and code to process and analyse Chemical Genomics experiments performed in the Ehrt Schnappinger lab (https://www.ehrtschnappingerlabs.org/). The experimental setup used was developed by the Broad Institute (https://www.nature.com/articles/s41586-019-1315-z). For the purposes of this demo, the data has been encoded to mask drug and strain names. The overarching goal of this experiment was to predict mechanisms of actions of anti-tuberculosis drugs using chemical-genetic interactions.
## Details of experimental setup
Each experiment consists of taking pools of M. tuberculosis depleted of essential targets and screening them against compound libraries to determine chemical genetic interactions. An overview of the experimental setup is as follows:

1. Anti-mycobacterial drugs are pipetted at varying concentrations into 96 well plates, with each plate corresponding to one drug. For each drug, seven concentrations are used (0.125X MIC - 8X MIC) in addition to a no drug control (0.000x MIC). Each drug-MIC combination has six replicates. Details of this are recorded in the file 'HypoIII_all_drugs_encoded.csv' under the columns 'Dispensedwell', 'Dispensedrow', 'Dispensedcol'.
2. Strain pools consist of ~400 M. tuberculosis strains, each containing an inducible depletion system targeting an essential gene. These are pipetted into drug plates. Simutaneously depletion is induced.
3. After the desired incubation time, optical density of the plates is recorded. Details of this are in the file '181120_HypoIII_Ods_encoded.xlsx'
with the ODs under the column 'RawData'.
4. Strains are harvested and PCR'ed to amplify the barcode-containing region. To enable multiplexing, different p5 indexes are used for each well, and different p7 indexes are used for each plate.
5. Amplicons are submitted for Illumina sequencing. FASTQ files thus obtained are converted into counts file (code for this is not shown, since it was developed by a collaborator and remains unpublished). 
6. Each Illumina run generates a .counts file. This is a a tab delimited file containing demultiplexed counts for each p5-p7 (and therefore each plate-well) combination and for each strain in the mix. Refer to files within 'Counts_files_encoded' for reference.

## Details of code provided
The counts files act as inputs for the code provided in this repo. The file 'HypoIII-Counts_to_FC-For_git.ipynb' processes Counts_files to cleaned dataframes, while performing several steps of QC in between. Each step generates a folder under 'Analysis'. 

The files under folders in 'Analysis' act as inputs to the EDA.ipynb file. This file is used to examine the quality of the overall data and explore possible reasons for poor quality PCRs.

Finally the filtered data are fit to a logistic regression model to explore if strain depletion patterns can predict drug classes. Outputs of this are two confusion matrices and a file with the coefficients of the logistic regression classifier(found under 9-Logistic_regression/HypoIII)
