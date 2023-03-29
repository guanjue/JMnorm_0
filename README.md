# JMnorm      <img src="https://raw.githubusercontent.com/guanjue/JMnorm/main/Figs/CAMP4.svg" align="right" width="120"/>

## A novel approach for Jointly Multi-feature normalization of epigenomic data across cell types and species

### Outline
**[(1) Summary](#Summary)**<br>
#####
**[(2) Citation](#Citation)**<br>
#####
**[(3) JMnorm Overview](#JMnorm-Overview)**<br>
#####
**[(4) Requirements](#Requirements)**<br>
#####
**[(5) Installation](#Installation)**<br>
#####
**[(6) Input data](#Input-data)**<br>
#####
**[(7) Running JMnorm](#Running-JMnorm)**<br>
#####
**[(8) Output data after JMnorm](#Output-data-after-JMnorm)**<br>
#####
**[(9) Support](#Support)**<br>
#####
**[(10) LICENSE](#LICENSE)**<br>
#####

## Summary
Combinatorial patterns of chromatin features reflect the functions of genomic regions in transcriptional regulation. However, technical noise from different epigenomic datasets can hinder the ability to extract true biological information. While many chromatin features, including chromatin accessibility and certain histone modifications, show correlated relationships, most existing approaches in data normalization are designed to process each feature independently. Such strategies can potentially introduce bias into the normalized data and lose the correlations among the chromatin features. Here, we present a novel approach named Joint Multi-feature normalization (JMnorm), for simultaneously normalizing multiple chromatin features across cell types and species by borrowing information from partially correlated features. 

## Citation
Guanjue Xiang, Yuchun Guo, David Bumcrot. JMnorm: a novel approach for Jointly Multi-feature normalization of epigenomic data across cell types and species. (2023)


## JMnorm Overview
![logo](https://raw.githubusercontent.com/guanjue/tmp_figs/master/figs/JMnorm.Figures1.png)
Overview of the JMnorm normalization method: The method consists of four key steps. (A) Transform the correlated components of various epigenomic signals into a mutually independent, high-dimensional PCA space. Each column on the left represents the average signal vector of a specific chromatin feature at cCRE regions in the reference or target sample. The columns on the right display the corresponding transformed PCA signal matrices derived from all chromatin features at cCRE regions. The yellow box signifies the rotation matrix learned by PCA from the reference signal matrix. (B) Generate reference clusters based on reference data in the PCA space. Assign target data to reference clusters according to the Euclidean distance in the PCA space. The heatmap represents the average PCA signal matrix of the reference clusters derived from reference data. (C) Apply QTnorm to normalize the target signal matrix against the reference matrix within each cluster (within-cluster QTnorm). Within each cluster, the number of cCREs may vary between the reference and target samples. (D) Reconstruct the JMnorm-normalized target signal matrix in the original signal space. The yellow box indicates the transposed rotation matrix learned by PCA in the first step (Panel A).

## Requirements
R version 4.0.0 or later
Packages: readr, pheatmap

## Installation 
```
# create conda environment for JMnorm with required dependencies
conda create -n jmnorm r r-readr r-dynamicTreeCut r-pheatmap

# activate the environment
conda activate jmnorm

# start R
# source the script in R
# source("JMnorm.script.R")
```


## Input data
- The input Target signal matrix and input Reference signal matrix should be formatted as N-by-(M+1) matrices, where N represents the number of cCREs, and M represents the number of chromatin features. The first column of each matrix contains the cCRE IDs. The signal values in orignal linear scale for each chromatin feature in the cCREs are saved in the 2~M columns.
- Example input Target / Reference signal matrices can be found in these links [Target signal matrix](https://github.com/guanjue/JMnorm/blob/main/docs/TCD8.JMnorm_sigmat.txt) & [Reference signal matrix](https://github.com/guanjue/JMnorm/blob/main/docs/ref.raw_sigmat.txt).
```
# Input reference signal matrix
>>> head ref.raw_sigmat.txt 
1	0.927	0	0	0	0	0	0
2	0.235	0.17	0.023	0.611	0.014	0.062	0.038
3	1.684	2.701	1.453	0.741	0.819	1.376	5.44
4	0.829	1.017	0.627	0.413	0.455	0.455	2.712
5	2.385	1.427	1.292	0.602	1.159	3.49	6.337
6	3.79	0.732	0.636	0.293	2.049	1.057	0.244
7	2.793	0.681	0.406	0.219	1.085	0.954	0.27
8	3.204	0	0	0	0	0.309	0.098
9	1.264	0	0	0	0	0	0.134
10	1.364	0	0	0	0	0.315	0.062

# Input target signal matrix
>>> head TCD8.raw_sigmat.txt 
1	0	0	0	0	0	0	0
2	0	0.104478	0	0	0	0.139552	0
3	0.385093	2.3354	0	0	0	2.47516	6.02857
4	1.41256	0.571749	0	0	0	0	2.91614
5	2.92286	1.86457	0	0	0	0	9.50857
6	1.82927	1.53252	0.855691	0	0	1.21057	0.347561
7	0	0.613636	0.510331	0	0	0.636364	0
8	0.394737	0	0	0	0	0	0
9	0.657895	0	0	0	0	0	0
10	0	0	0	0	0	0	0

# The chromatin features used in this example file: ATAC, H3K27ac, H3K27me3, H3K36me3, H3K4me1, H3K4me3, H3K9me3

```


## Running JMnorm
Detailed instructions on using JMnorm can be found in the [Getting started with JMnorm](https://guanjue.github.io/JMnorm/) R markdown file.
The testing signal matrices can be found in this link: https://github.com/guanjue/JMnorm/tree/main/docs



## Output data after JMnorm
- The output target signal matrix after JMnorm should be formatted as N-by-(M+1) matrices, where N represents the number of cCREs, and M represents the number of chromatin features. The first column of each matrix contains the cCRE IDs. The signal values in orignal linear scale for each chromatin feature in the cCREs are saved in the 2~M columns.
- Example output target signal matrix after JMnorm can be found in this [Target.JMnorm_sigmat.txt](https://github.com/guanjue/JMnorm/blob/main/docs/TCD8.JMnorm_sigmat.txt).
```
>>> head TCD8.JMnorm_sigmat.txt
1	0.068	0.05	0.099	0.025	0.002	-0.064	0.137
2	0.129	0.105	0.156	0.073	0.094	0.131	0.187
3	0.341	2.493	0.269	0.006	0.435	0.984	4.906
4	1.15	0.499	0.664	0.216	0.417	0.161	2.134
5	1.566	2.022	0.383	-0.018	0.408	-0.015	7.836
6	0.674	1.123	1.054	0.323	0.513	0.59	0.466
7	0.334	0.535	0.709	0.219	0.414	0.458	0.332
8	0.444	0.099	0.161	0.059	0.091	-0.048	0.208
9	0.604	0.104	0.162	0.05	0.096	-0.059	0.214
10	0.075	0.079	0.13	0.065	0.086	-0.101	0.179
```


## Support
For questions or issues, please either create an issue on the GitHub repository or feel free to reach out via the following email addresses: gxiang@camp4tx.com, guanjuexiang@gmail.com


## LICENSE
This project is licensed under the MIT License. See the [LICENSE](https://github.com/guanjue/JMnorm/blob/main/LICENSE) file for details.







