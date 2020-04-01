---
layout: post
title: Monocle3 Clustering, Annotation and Pseudotime Analysis
---

#### Load the following librararies
library("monocle3")
library(devtools)
library(dplyr)


#### Load the data
```cds_monocle3 <- load_cellranger_data("../../outs")```
#### Pre-process the data
Normalize the data using Principal Components Analysis since this is RNA seq analysis
```cds_m_preproc <- preprocess_cds(cds_monocle3, num_dim = 30)```
#### Reduction of Dimensionality
```cds_red_dim <- reduce_dimension(cds_m_preproc, reduction_method = 'UMAP', preprocess_method = 'PCA')```
#### Cluster the Cells
# cluster_cells uses a technique called community detection to group cells. 
# This approach was introduced by Levine et al as part of the phenoGraph algorithm.
# cluster_cells() also divides the cells into larger, more well separated groups called partitions, 
# using a statistical test from Alex Wolf et al, introduced as part of their PAGA algorithm.
```cds_cluster <- cluster_cells(cds_red_dim)```
#### Learn the trajectory graph
```cds_graph <- learn_graph(cds_cluster)```
Plot the cells from the learn graph to see the clusters, with and without the trajectory graph to detect the root cluster/node
```plot_cells(cds_graph, show_trajectory_graph = FALSE)```
In this case,I have preliminary annotations according to cluster number and thus selected ```show_trajectory_graph = FALSE```

#### Annoate the cell clusters
# Add a new column to colData(cds_ordered) and initialize it with the values of clusters(cds)
```colData(cds_ordered)$assigned_cell_type <- as.character(partitions(cds_ordered))```
# Now, we can use the dplyrpackage's recode() function 
# to remap each cluster to a different cell type
```
colData(cds_ordered)$assigned_cell_type = dplyr::recode(colData(cds_ordered)$assigned_cell_type,
                                                "1"="OL1",
                                                "2"="Med Spiny Neurons",
                                                "3"="Med Spiny Neurons",
                                                "4"="OL2",
                                                "5"="Immune Cells",
                                                "6"="Astrocytes",
                                                "7"="NSPCs",
                                                "8"="post-NSPCs",
                                                "9"="post-NSPCs",
                                                "10"="OPCs",
                                                "11"="Unclassified neurons",
                                                "12"="OPCs",
                                                "13"="OL1",
                                                "14"="post-NSPCs",
                                                "15"="Immune Cells",
                                                "16"="Ependymal Cells",
                                                "17"="Unclassified neurons",
                                                "18"="Neurons (IN)",
                                                "19"="Unclassified neurons",
                                                "20"="post-NSPCs",
                                                "21"="Med Spiny Neurons",
                                                "22"="Vessels",
                                                "23"="Vessels",
                                                "24"="Astrocytes",
                                                "25"="Immune Cells",
                                                "26"="Neurons (IN)",
                                                "27"="Immune Cells",
                                                "28"="Immune Cells",
                                                "29"="Unclassified neurons",
                                                "30"="Unclassified neurons",
                                                "31"="Neurons (IN)",
                                                "32"="OL1",
                                                "33"="OL1")
```                                                
# Let's see how the new annotations look:
```
plot_cells(cds_ordered, group_cells_by="partition", 
           color_cells_by="assigned_cell_type",           
           show_trajectory_graph = FALSE)
```
