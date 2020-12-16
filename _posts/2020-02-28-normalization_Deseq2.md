---
layout: post
title: Normalization DeSeq2
---
Generalized Linear Model - normalization
https://hbctraining.github.io/DGE_workshop/lessons/02_DGE_count_normalization.html
Since tools for differential expression analysis are comparing the counts between sample groups for the same gene, gene length does not need to be accounted for by the tool. However, sequencing depth and RNA composition do need to be taken into account.

To normalize for sequencing depth and RNA composition, DESeq2 uses the median of ratios method. On the user-end there is only one step, but on the back-end there are multiple steps involved, as described below.

|        | Sample 1          | Sample 2  |Sample 3|
| ------ |:-------------:| -----:|-----:|
| Gene 1| 0| 10 |4|
| Gene 2| 2 | 6 |12|
| Gene 3 | 33 |55|200|

Step 1:
Log of raw base counts
Log with base e

Step 2:
Average of the logs for each gene in each sample

Step 3:
Filters genes with 0 counst in more than one sample

Step 4:
Subtract log(raw counts) -log(average) for eacg gene
This is a ratio essentially of each gene across all samples

Step 5:
Calculate the median for each gene

This helps to remove extreme gene expression like genes with high expression influencing genes with low expression. Thus focusing on genes with median expression and houskeeping genes

Step 6: 
Convert median to normal values which is the scaling factor
e^median = Normal

Step 7:
Divide original read counts by scaling factor

Reference - [StatQuest: DESeq2, part 1, Library Normalization](https://youtu.be/UFB993xufUU) 



