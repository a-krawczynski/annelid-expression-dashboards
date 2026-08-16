# Andreatta Fold-Change Comparison Dashboard

Interactive fold-change explorer for the genotype-comparison transcriptomics study from:

> Andreatta, G. et al. (2026). *PNAS* 123(12):e2514719123. https://doi.org/10.1073/pnas.2514719123

**Authors:** Alina Krawczynski, Rannyele Passos Ribeiro (powered by Claude AI)
**Lab:** Özpolat Lab, Washington University in St. Louis, 1 Brookings Drive, St. Louis, MO 63130
**Year:** 2026

## What this is

A self-contained Jupyter dashboard for exploring log2 fold-change results across ~20 pairwise
differential-expression comparisons between specific *Platynereis dumerilii* genotypes
(l-cry⁻ᐟ⁻, nr0b1/2⁻ᐟ⁻, nr0b1/2⁺ᐟ⁻, and their respective wild types). Unlike the other dashboards
in this series, there's no single expression matrix here — instead, pick genes by raw ID or by a
curated gene symbol, then generate significance-colored bar charts, a hierarchically-clustered
log2FC heatmap, and annotation lookups — all exportable as publication-ready vector PDFs and CSVs
to a folder of your choosing.

Built directly from the paper's own precomputed differential-expression tables (PNAS Datasets S1,
S3, S4).

## What's in this folder

| File | What it is |
|---|---|
| `Fold_Change_Comparison_Dashboard.ipynb` | The dashboard notebook — open this in Jupyter. |
| `de_results_all_comparisons.csv` | Combined differential-expression results across all ~20 pairwise comparisons (130,914 rows). |
| `curated_gene_summary.csv` | Curated gene-of-interest list (Dataset S5) with clean, human-readable gene symbols, used for symbol-based search. |

`de_results_all_comparisons.csv` is 52MB — large but under GitHub's 100MB per-file limit, so no
trimming was needed here (unlike the annotation files in some of the other dashboards, this table
is mostly numeric fold-change/p-value data, not bulky free text).

## Requirements

- Python 3.9+
- `pandas`, `numpy`, `matplotlib`, `seaborn`, `scipy`, `ipywidgets`

```bash
pip install pandas numpy matplotlib seaborn scipy ipywidgets
```

Then open `Fold_Change_Comparison_Dashboard.ipynb` in Jupyter and run all cells in order.

## How to use it

1. **Choose a Pairwise Comparison, then its Details** — pick one genotype comparison, then any
   subset of its tissue/mapping/DE-analysis variants.
2. **Choose Target Genes** — filter/click from the gene list, paste raw IDs or gene symbols, or
   search by annotation keyword (e.g. "cytochrome", "kinase").
3. **Generate Visuals** to preview fold-change bar charts, the multi-gene heatmap, and annotation
   tables on-screen, or **Export Active Results** to save vector PDFs and CSVs to a folder you
   choose (via the folder-browse button or by typing a path).

A colorblind-safe palette toggle (Purple-Orange / Brown-Teal / Cividis) is available next to the
default red-blue scheme for both the bar charts and the heatmap.

## Normalization

This dashboard doesn't show absolute expression at all, so there's no normalization step
performed here. Every log2FC/padj value plotted is read directly from the paper's own precomputed
differential-expression tables, which the paper generated with its own upstream normalization and
DE-testing pipeline — see the paper's Methods for that procedure.

## BLAST sequence search (optional)

The notebook includes a local BLAST search cell for looking up a pasted nucleotide sequence
against the HAMN01 reference transcriptome (the assembly this dashboard's transcriptome-mapped
gene IDs were generated against). This requires installing NCBI BLAST+ and building the search
database yourself — the database itself isn't included here (it's a large, regenerable binary
index, not primary data). Full setup instructions, including the exact `makeblastdb` command, are
in the markdown cell above the BLAST cell in the notebook. Note this only covers
transcriptome-mapped (`comp..._c#_seq#`) genes — genome-mapped (`XLOC_...`) genes aren't
searchable this way, since no matching genome reference sequence is available.

## Citation

If you use this dashboard, please cite the original paper:

> Andreatta, G. et al. (2026). PNAS 123(12):e2514719123. https://doi.org/10.1073/pnas.2514719123
