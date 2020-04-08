---
layout: post
title: Normalization for Single Cell Methods
---

#### 1. 10X 
Read depth normlaization during cellranger aggr.

#### 2. Seurat
method “LogNormalize” that normalizes the feature expression measurements for each cell by the total expression, 
  multiplies this by a scale factor (10,000 by default), and log-transforms the result. 

#### 3. Monocle - 
Size Factor Normalization
  read count for each gene / all read counts for that gene across all samples
  The median of these ratios for a sample, called the size factor.

