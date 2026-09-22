# **Bulk RNA-Seq Meta-Analysis Framework**

A reproducible pipeline for combining differential expression results across multiple independent bulk RNA-seq studies (GEO/SRA-derived) to identify genes that are consistently up- or down-regulated across datasets, rather than relying on the findings of any single study.

---

## Overview

Individual RNA-seq studies are limited by small cohorts, single-lab technical variation, and cohort-specific noise. This pipeline addresses that by:

1. Processing multiple independent datasets through a consistent DESeq2 workflow
2. Annotating and standardizing gene identifiers across studies
3. Identifying genes shared across study-level DE results
4. Running a formal meta-analysis (effect size combination) across studies
5. Summarizing, visualizing, and functionally interpreting the combined results

## Repository Structure

```
Bulk-RNA-Seq-Analysis/
├── inputs/                          # per-study accession lists, run tables, decoy sequences
│   ├── SRR_Acc_List.txt
│   ├── SraRunTable.csv
│   └── decoys.txt
├── scripts/
│   ├── 00_Setup.R                   # environment/package setup
│   ├── 01_DESeq2.R                  # per-study DESeq2 differential expression
│   ├── 02_annotation.R              # gene ID / symbol annotation
│   ├── 03_common_genes_venn.R       # overlap of DE genes across studies
│   ├── 04_meta_analysis_metavolcano.R  # cross-study meta-analysis (metavolcano)
│   ├── 05_meta_summary.R            # summary tables of meta-analysis results
│   ├── 06_volcano_plot.R            # volcano plot visualization
│   ├── 07_enrichment_analysis.R     # GO/KEGG enrichment on meta-analysis results
│   └── 08_network_analysis.R        # protein-protein interaction / network analysis
├── data/                            # (gitignored) raw and intermediate per-study data
├── results/                         # (gitignored) output tables and figures
└── README.md
```

> [!NOTE]
> Large raw data files (FASTQ, full count matrices, `.h5`/`.rds` intermediates) are excluded from version control via `.gitignore`. Only scripts, small reference tables, and final summarized results are tracked.

## Workflow

Run the scripts in `scripts/` in numeric order:

| Script | Purpose |
|---|---|
| `00_Setup.R` | Install/load required packages, set project paths |
| `01_DESeq2.R` | Run DESeq2 differential expression per included study |
| `02_annotation.R` | Map gene IDs to consistent gene symbols across studies |
| `03_common_genes_venn.R` | Identify genes overlapping across study-level DE gene lists |
| `04_meta_analysis_metavolcano.R` | Combine per-study effect sizes into a meta-analysis |
| `05_meta_summary.R` | Produce summary tables of meta-analysis results |
| `06_volcano_plot.R` | Visualize combined results as a volcano plot |
| `07_enrichment_analysis.R` | GO/KEGG functional enrichment on the meta-analysis gene list |
| `08_network_analysis.R` | Build a protein-protein interaction network from top genes |

## Requirements

- R (≥ 4.5)
- Key packages: `DESeq2`, `tximport`, `clusterProfiler`, `org.Hs.eg.db`, `MetaVolcanoR` (or equivalent meta-analysis package), `tidyverse`, `ggplot2`, `pheatmap`

Install via:
```r
install.packages("BiocManager")
BiocManager::install(c("DESeq2", "tximport", "clusterProfiler", "org.Hs.eg.db"))
```
## Data Sources

Datasets included in this meta-analysis were identified via public repositories (GEO, ArrayExpress, GREIN, recount3) following a defined eligibility screen (organism, sample size, control-vs-disease structure). Accession lists and run tables are provided under `inputs/`.

## License

MIT License — see `LICENSE` for details.
