# Contents
- [Overview](#overview)
- [Environment](environment)
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
