### PyBSASeq
A novel algorithm for BSA-Seq data analysis implemented in Python

Python 3.6 or above is required to run the script. 

#### Usage

`$ python PyBSASeq.py -i input -o output -p popstrct -f fbsize -s sbsize`

Here are the details of the options used in the script:
- input – the name of the input file (the GATK4-generated tsv file)
- output – the name of the output file
- popstrct – population structure; three choices available: F2 for an F2 population, RIL for a population of recombinant inbred lines, or BC for a backcross population
- fbsize – the number of individuals in the first bulk
- sbsize – the number of individuals in the second bulk

The default p-value for identifying ltaSNPs from the SNP data set is 0.01 (alpha), the default p-value for identifying ltaSNPs from the simulated dataset is 0.1 (smalpha). These values can be changed using the following options:

`--alpha p1 --smalpha p2`

p1 and p2 should be in the range of 0.0 – 1.0, the chosen value should make statistical sense. The greater the p2 value, the higher the threshold and the lower the false positive rate.

#### Workflow
1. SNP filtering
2. Perform Fisher's exact test using the AD values of each SNP from both bulks. A p-value will be otained, whcih can be used to identify ltaSNPs from the SNP data set. In the meantime, simulated REF/ALT reads of each SNP is obtained under null hypothesis via simulation, and again Fisher's exact test is performed using the simulated AD values, and the p-value based on the simulated AD values can be used to identify ltaSNPs from the simulated dataset (for threshold calculation). A file "COMPLETE.txt" is writen in the working directory if Fisher's exact test is successful, and the results of Fisher's exact test are saved in a .csv file. The "COMPLETE.txt" file need to be deleted if startover is desired. 
3. Threshold calculation. The results is save in the "threshold.txt" file. The "threshold.txt" file need to be deleted if startover is desired.
4. Plotting.

#### Dataset
The file [snp_final.tsv.bz2](https://github.com/dblhlx/PyBSASeq/blob/master/snp_final.tsv.bz2) contains the [GATK4](https://software.broadinstitute.org/gatk/download/)-generated SNP dataset using the sequencing data from [the work of Yang et al](https://www.ncbi.nlm.nih.gov/pubmed/23935868). The sequence reads were treated as either single-end or paired-end when aligned to the reference genome. Significantly more SNPs were identified by the latter approach; however, the results of BSA-Seq analysis were very similar. Only the tsv file generated by the former approach is included here because of the 25 Mb file size limitation. You can issue the command `python PyBSASeq.py` to test the Python script using this SNP dataset. The bz2 file should be uncompressed to the working directory before testing. It took ~5.5 hours to run the script with the above SNP dataset on a desktop computer (Intel i5-3570k CPU and 16 GB ram).

#### Other methods for BSA-Seq data analysis
BSA-Seq data analysis can be done using either [the SNP index method](https://onlinelibrary.wiley.com/doi/full/10.1111/tpj.12105) or [the smoothed G-statistic method](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1002255) as well. I implemented both in Python for the purpose of comparison: [PySNPIndex](https://github.com/dblhlx/PySNPIndex) and [PyGStatistic](https://github.com/dblhlx/PyGStatistic). The Python implementation of [the original smoothed G-statistic method](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1002255) by Magwene can be found [here](https://bitbucket.org/pmagwene/bsaseq/src/master/) (just found this site today, 6/27/2019), and the R implementation of both methods by Mansfeld can be found [here](https://github.com/bmansfeld/QTLseqr).
