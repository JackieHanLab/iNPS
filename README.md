# Contents
- [Overview](#overview)
- [Environment](#environment)
- [Usage](#usage)
- [Inputs](#inputs)
- [Outputs](#outputs)
# Overview
&nbsp;&nbsp;&nbsp;&nbsp;iNPS is improved from X. S. Liu’s NPS algorithm, for high quality nucleosome positioning from MNase-seq data. Our procedure contains the following eight steps.  
&nbsp;&nbsp;&nbsp;&nbsp;(1) Generate a wave-form nucleosome profile, with the resolution of 10 bp, by extending each tag from the 5’ end by 150 bp, and taking the middle 75 bp as the enrichment of nucleosome signal. For paired-end sequencing data, the middle 50% part of each tag is taken as the enrichment of nucleosome signal.  
&nbsp;&nbsp;&nbsp;&nbsp;(2) Perform Gaussian convolution and first/second/third derivative of Gaussian convolution to smooth the nucleosome profile and find extremum/infection/most-winding points.  
&nbsp;&nbsp;&nbsp;&nbsp;(3) Distinguish each pair of inflection points as a candidate of “main” nucleosome peak or “shoulder”.  
&nbsp;&nbsp;&nbsp;&nbsp;(4) Determine whether a “shoulder” candidate should be an independent nucleosome, or the dynamic part of the adjacent “main” nucleosome peak.  
&nbsp;&nbsp;&nbsp;&nbsp;(5) Adjust the inflection borders of the preliminary nucleosome detection.  
&nbsp;&nbsp;&nbsp;&nbsp;(6) Merge the closely located nucleosome peaks as “doublets”.   
&nbsp;&nbsp;&nbsp;&nbsp;(7) Filter some nucleosome peaks with bad shapes.  
&nbsp;&nbsp;&nbsp;&nbsp;(8) Perform statistical tests to quantify the confidence level of each nucleosome. 
# Environment
&nbsp;&nbsp;&nbsp;&nbsp;iNPS was developed with python 3.2, so the python 3 environment must be installed under a Linux system. 
# Usage
1. Command line:  
`$ python3 iNPS_V1.2.2.py -i -o -c -l --s_p`
2. For help, please try:  
`$ python3 iNPS_V1.2.2.py -h`
3. Arguments for command line:

arguments    | explain
---          | ---
--version    | show program's version number and exit
-h, --help   | show help message and exit
-i, --input  | `/path/filename` a file of sequencing tags in a standard **BED format** ( chromosome <tab> start <tab> end <tab> name <tab> score <tab> strand )
-o, --output | `/path/filename` here, the name extension is unnecessary. Software will output two result files, `filename_[ChromosomeName].like_b ed` and `filename_[ChromosomeName].like_wig`, to record coordinates and profiles of detected nucleosomes respectively. The chromosome name will be added as suffix in the file names. If your detect nucleosomes on multiple chromosomes, for each chromosome, software will output two result files `filename_[ChromosomeName].like_bed` and `filename_[ChromosomeName].like_wig` respectively. And finally, a file `filename_Gathering.like_bed` will gather the detected nucleosomes on every chromosome. Note that a path `/path/filename/` or `/path/filename_[ChromosomeName]/` will be built to record the preliminary and intermediate data.
-c, --chrname| Specify the name (or abbreviation) of the chromosome, if you would like to do nucleosome detection **ONLY on ONE single chromosome**. For nucleosome detection on ***multiple chromosomes, please do NOT use this parameter***, software will detect nucleosome on each chromosome *ONE-BY-ONE* in the input data as default.
-l, --chrlength | The length of the chromosome. **ONLY used for nucleosome detection on ONE single chromosome**. If you do NOT use this parameter, software will find the maximum coordinate in the input data to represent the chromosome length as default. For nucleosome detection on ***multiple chromosomes, please do NOT use this parameter***. The length of each chromosome will be determined by the tag with maximum coordinate of the corresponding chromosome respectively.
--s_p | “s” or “p”, default = s. **Set to “p” if the input data is paired-end tags.** Otherwise, set to “s” or use the default setting if the input data is single-end tags.
--pe_max | The superior limit of the length of paired-end tags, default = 200. The tags longer than the cutoff will be ignored. **This parameter is ONLY available for paired-end sequencing data.** Please avoid using too large value.
--pe_min | The inferior limit of the length of paired-end tags, default = 100. The tags shorter than the cutoff will be ignored. **This parameter is ONLY available for paired-end sequencing data.** Please avoid using too small value.

4. Examples:
* Example 1:  
`$ python3 iNPS_V1.2.2.py -i /PathA/InputFile.bed -o /PathB/Output -c chr1 -l 247249719`  
&nbsp;&nbsp;&nbsp;&nbsp;Do nucleosome detection ONLY on chromosome 1, as the parameter “-c” has been set to “chr1”. And since the “-l” has been set to 247249719, the maximum coordinate of resulted nucleosome profiles will be 247249719. The output files are listed in the following table:

Name | --- | Description 
--- | --- | ---
/PathB/Output_chr1.like_bed | Results | Coordinates of detected nucleosomes in chr1 
/PathB/Output_chr1.like_wig | Results | Detected nucleosome profiles in chr1 
/PathB/Output_chr1/chr1.bed | Intermediate records | MNase-seq tags of chr1, extracted from the input file `/PathA/InputFile.bed` 
/PathB/Output_chr1/InputData_Summary.txt | Intermediate records | Recording the number of tags of chr1, the maximum coordinate among the tags of chr1, and the chromosome length of chr1. 

* Example 2:  
`$ python3 iNPS_V1.2.2.py -i /PathA/InputFile.bed -o /PathB/Output -c chr1`  
&nbsp;&nbsp;&nbsp;&nbsp;Do nucleosome detection ONLY on chromosome 1, as the parameter “-c” has been set to “chr1”. Without “-l” setting, software will use the maximum coordinate of MNase-seq tag of chromosome 1 as the length of chromosome 1. The output files are listed in the following table: 

Name | --- | Description 
--- | --- | ---
/PathB/Output_chr1.like_bed | Results | Coordinates of detected nucleosomes in chr1 
/PathB/Output_chr1.like_wig | Results | Detected nucleosome profiles in chr1 
/PathB/Output_chr1/chr1.bed | Intermediate records | MNase-seq tags of chr1, extracted from the input file `/PathA/InputFile.bed` 
/PathB/Output_chr1/InputData_Summary.txt | Intermediate records | Recording the number of tags of chr1, the maximum coordinate among the tags of chr1, and the chromosome length of chr1. 

* Example 3:  
`$ python3 iNPS_V1.2.2.py -i /PathA/InputFile.bed -o /PathB/Output`  
&nbsp;&nbsp;&nbsp;&nbsp;Do nucleosome detection on each chromosome in “InputFile.bed”. Software will use the tag with maximum coordinate of each chromosome as the length of the corresponding chromosome respectively. The output files are listed in the following table:

Name | --- | Description 
--- | --- | ---
/PathB/Output_chr1.like_bed & ... & /PathB/Output_chrY.like_bed | Results | Coordinates, shape properties, and statistical scores of the detected nucleosomes in each of the 24 chromosomes (1 ~ 22, X, and Y) respectively. 
/PathB/Output_Gathering.like_bed | Results | Gather the nucleosome information of the 24 “like_bed” files for each of the 24 chromosomes respectively.
/PathB/Output_chr1.like_wig & ... & /PathB/Output_chrY.like_wig | Results | Detected nucleosome profiles in each of the 24 chromosomes (1 ~ 22, X, and Y) respectively 
/PathB/Output/chr1.bed & ... & /PathB/Output/chrY.bed | Intermediate records | Splitting the input file “InputFile.bed” by chromosomes 
/PathB/Output/InputData_Summary.txt | Intermediate records | Recording the number of tags, the maximum coordinate among the tags, and the chromosome length of each of the 24 chromosomes (1 ~ 22, X, and Y) respectively. 

# Inputs
# Outputs
