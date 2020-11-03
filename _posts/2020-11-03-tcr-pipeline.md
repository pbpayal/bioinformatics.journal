---
layout: post
title: TCR pipeline
---
# Mixcr pipeline
## OCT 2020
MiXCR v3.0.13 (server 10.54.13.36)
RNA Seq pipeline explanation 
https://mixcr.readthedocs.io/en/master/rnaseq.html
Align sequencing reads against reference V, D, J and C genes > assemble overlapping fragmented sequencing reads into long-enough CDR3 containing contigs (Perform two rounds of contig assembly)  > Perform extension of incomplete TCR CDR3s with uniquely determined V and J genes using germline sequences > Assemble clonotypes > Export clones

align > contig assembly > assemble > extract

Align - The align command aligns raw sequencing reads to reference V, D, J and C genes of T- and B- cell receptors (Mixcr uses GenBank gene accessions)

mixcr align -p rna-seq -s hsa -OallowPartialAlignments=true -r align1_report.txt read1_index_3_3.fq.gz read2_index_3_3.fq.gz alignmnets_rna.vdjca

https://www.takarabio.com/learning-centers/next-generation-sequencing/technical-notes/immune-profiling/tcr-repertoire-profiling-from-human-samples-(bulk)
  - What are T-cell receptors?
In humans and closely related species, cellular immunity is mediated by T cells (or T lymphocytes), which participate directly in the detection and neutralization of pathogenic threats. Essential to T-cell function are highly specialized extracellular receptors (T-cell receptors or TCRs) that selectively bind specific antigens displayed by major histocompatibility complex (MHC) molecules on the surface of antigen-presenting cells (APCs) (Figure 1, Panel A). Antigen recognition by TCRs activates T cells, causing them to proliferate rapidly and mount immune responses through the release of cytokines.

Given the relative specificity of TCR-antigen interactions, a tremendous diversity of TCRs are required to recognize the wide assortment of pathogenic agents one might encounter. To this end, the adaptive immune system has evolved a system for somatic diversification of TCRs that is unrivaled in all of biology. The vast majority of TCRs are heterodimers composed of two distinct subunit chains (α- and β-), which both contain variable domains and, in humans, are encoded by single-copy genes. The term "clonotype" is typically used to refer either to a particular TCR variant (TCR-α or TCR-β subunit) or to a particular pairing of TCR subunit variants (TCR-α + TCR-β) shared among a clonal population of T cells. TCR diversity is generated during the early stages of T-cell development. T-cell progenitors are derived from hematopoietic stem cells (HSCs) in the thymus, and as these cells divide, extensive recombination occurs between the V- and J-segments, and the V-, D-, and J-segments, in the TCR-α and TCR-β genes, respectively, via a mechanism that also incorporates and deletes additional nucleotides (Figure 1, Panel B). Ultimately, this process—commonly referred to as "V(D)J recombination"—yields a population of T cells with sufficient TCR diversity to collectively recognize any peptide imaginable. The region of TCR-β that spans the V-D and D-J junctions, known as "complementarity determining region 3" (CDR3), is unique to each TCR-β variant and is frequently used to quantify TCR diversity in high-throughput profiling experiments. Following somatic diversification, T cells that lack sufficient affinity for MHC molecules and those that recognize self-antigens are eliminated (positive and negative selection, respectively), yielding a functional T-cell repertoire.
TCR/BCR refenrece sequences library

https://mixcr.readthedocs.io/en/master/appendix.html#tcr-bcr-refenrece-sequences-library

 Perform two round of contig assembly and extend alignments

mixcr assemblePartial alignmnets_rna.vdjca alignmentsRescued_1.vdjca

mixcr assemblePartial alignmentsRescued_1.vdjca alignmentsRescued_2.vdjca

Perform extension of incomplete TCR CDR3s with uniquely determined V and J genes using germline sequences.

mixcr extendAlignments alignmentsRescued_2.vdjca alignmentsRescued_2_extended.vdjca

Assemble (see here for details) clonotypes

assemble command builds clonotypes from alignments obtained with align.  Clonotypes assembly is performed for a chosen assembling feature (e.g. CDR3 by default).  

mixcr assemble alignmentsRescued_2_extended.vdjca clones.clns

Extract

mixcr exportClones clones.clns clones.txt

https://mixcr.readthedocs.io/en/master/export.html

The resulting tab-delimited text file will contain columns with different types of information.

Appendix for syntax

https://mixcr.readthedocs.io/en/master/appendix.html#ref-encoding

 
# VDJ tools
## NOV 2020
Server: Linux - CentOS 7
```wget https://github.com/mikessh/vdjtools/releases/download/1.2.1/vdjtools-1.2.1.zip```

```unzip vdjtools-1.2.1.zip```

```export PATH=$PATH:/home/pbanerjee/tools/VDJtools/vdjtools-1.2.1```

``` java -jar vdjtools-1.2.1.jar joinsample -h```

```JoinSamples -p -m metadata.small.txt out/12```



