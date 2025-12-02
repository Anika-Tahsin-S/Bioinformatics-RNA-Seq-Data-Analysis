# Bioinformatics-RNA-Seq-Data-Analysis

This repo contains the code and report for an RNA-Seq differential expression analysis assignment using the breast cancer dataset **GSE183947** from NCBI GEO. The goal is to compare gene expression between primary breast cancer tissue and matched adjacent normal tissue, and to perform downstream functional/pathway analysis.

## Dataset

- **GEO accession:** [GSE183947](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE183947)  
- **Input file used here:** `GSE183947_fpkm.csv.gz`  
  - 20,246 genes  
  - 60 samples (30 normal `CA.*`, 30 cancer `CAP.*`)  
  - Values are FPKM (Fragments Per Kilobase per Million)

> Note: The raw dataset is not tracked in the repo due to size; download it from GEO and place it in the project root if you want to re-run the analysis.

## Methods (summary)

1. **Data loading and preprocessing**
   - Loaded FPKM matrix with `pandas`.
   - Split samples into normal vs cancer using column name prefixes.
   - Transformed expression values to `log2(FPKM + 1)`.

2. **Differential expression analysis**
   - Per-gene Welch two-sample t-test (`scipy.stats.ttest_ind`).
   - Benjamini–Hochberg FDR correction.
   - Significant DEGs defined as: FDR < 0.05 and |log2FC| > 1.
   - Results saved to `GSE183947_DE_results.csv`.

3. **Visualisation**
   - **Volcano plot:** log2 fold change vs -log10(FDR).
   - **MA plot:** mean log2 expression vs log2 fold change.
   - Plots saved as:
     - `volcano_plot_cancer_vs_normal.png`
     - `ma_plot_cancer_vs_normal.png`

4. **Downstream functional analysis**
   - Up-regulated genes: analysed with **g:Profiler (g:GOSt)** for GO:BP and KEGG pathways.  
     Output: `GSE183947_up_enrichment_gprofiler.csv`.
   - Down-regulated genes: analysed with **DAVID** using `DEG_down_genes.txt` as the gene list.  
     Screenshots of the DAVID Annotation Summary are included in:
     - `david_disease.png`
     - `david_functional_annotations.png`
     - `david_pathways.png`

## Files

- `RNA-Seq Data Analysis.ipynb` – main Jupyter notebook with all steps (loading, DE, plots, enrichment).
- `GSE183947_fpkm.csv.gz` – FPKM expression matrix (download from GEO).
- `GSE183947_DE_results.csv` – differential expression results (per gene).
- `DEG_down_genes.txt` – down-regulated DEGs used as input to DAVID.
- `GSE183947_up_enrichment_gprofiler.csv` – g:Profiler enrichment results.
- `volcano_plot_cancer_vs_normal.png` – volcano plot figure.
- `ma_plot_cancer_vs_normal.png` – MA plot figure.
- `david_*.png` – DAVID summary screenshots.
- `report.tex` / `report.pdf` – LaTeX report for the assignment.
