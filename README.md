# RNA-seq Analysis: Dexamethasone Treatment in Airway Smooth Muscle Cells

**Author:** Despoina Eleftheriadou\
**Dataset:** [Himes et al. 2014, PLoS ONE](https://doi.org/10.1371/journal.pone.0099625) — `airway` Bioconductor package

[**View the full rendered report →**](https://edespoina.github.io/airway-rnaseq-analysis/airway_rna-seq.html)

------------------------------------------------------------------------

## Overview

This project analyses an RNA-seq experiment in which human airway smooth muscle (ASM) cells from four donors were treated with **dexamethasone** (a glucocorticoid used to treat asthma) or left untreated. The goal is to identify genes that change expression in response to the drug.

The dataset contains read counts for \~64,000 genes across 8 samples (4 treated, 4 untreated) from 4 different cell lines.

------------------------------------------------------------------------

## Methods

Raw counts were loaded from the `airway` Bioconductor package and filtered to remove genes with zero counts across all samples.

Differential expression was tested with **DESeq2** using the design `~ cell + dex`, which accounts for donor (cell line) variability.

Genes with padj \< 0.05 and \|LFC\| \> 0.25 were considered significant.

Functional enrichment was performed with **clusterProfiler** (GO Biological Process), and validated with Fisher's exact test on the top GO term.

------------------------------------------------------------------------

## Key Results

- **951 genes** up-regulated and **763 genes** down-regulated (padj \< 0.05, \|LFC\| \> 0.25) in dexamethasone-treated cells
- Top GO terms for up-regulated genes: **response to peptide hormone**, **cell-substrate adhesion**, **response to oxidative stress**
- Top GO terms for down-regulated genes: **axonogenesis**, **monocyte differentiation**, **DNA damage response**
- Fisher's exact test confirmed enrichment of the top GO term among DE genes (OR = 2.30, p = 0.030)

### Comparison with Himes et al. 2014

To validate the analysis, results were compared with the original paper's top differentially expressed genes. **8 out of 11** top genes were fully replicated with the same direction and significance. The three discrepancies (CLCNKA, ADARB2, SERPINA3) all involve low-count or borderline genes where differences between pipelines (Tophat/edgeR vs STAR/DESeq2) are expected to have the largest effect.

------------------------------------------------------------------------

## How to Reproduce

1.  Clone this repository:

    ``` bash
    git clone https://github.com/edespoina/airway-rnaseq-analysis.git
    ```

2.  Open `airway_rna-seq.Rmd` in RStudio

3.  Install dependencies:

    ``` r
    if (!requireNamespace("BiocManager", quietly = TRUE))
      install.packages("BiocManager")
    BiocManager::install(c("airway", "DESeq2", "clusterProfiler", "org.Hs.eg.db"))
    install.packages(c("tidyverse", "pheatmap", "ggrepel"))
    ```

4.  Knit the document (`Ctrl/Cmd + Shift + K`)

------------------------------------------------------------------------

## Repository Structure

```         
airway-rnaseq-analysis/
├── README.md
├── airway_rna-seq.Rmd      # Full analysis source
└── docs/
    └── airway_rna-seq.html # Rendered report (GitHub Pages)
```

------------------------------------------------------------------------

## Reference

Himes BE et al. (2014). *RNA-Seq transcriptome profiling identifies CRISPLD2 as a glucocorticoid responsive gene that modulates cytokine function in airway smooth muscle cells.* PLoS ONE 9(6): e99625.
