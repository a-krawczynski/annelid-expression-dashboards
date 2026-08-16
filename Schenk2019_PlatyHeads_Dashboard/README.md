# PlatyHeads Expression Dashboard

Interactive gene-expression explorer for the head-maturation transcriptome from:

> Schenk, S. et al. (2019). *eLife*. https://doi.org/10.7554/eLife.45530

**Authors:** Alina Krawczynski, Rannyele Passos Ribeiro (powered by Claude AI)
**Lab:** Özpolat Lab, Washington University in St. Louis, 1 Brookings Drive, St. Louis, MO 63130
**Year:** 2026

## What this is

A self-contained Jupyter dashboard for exploring transcript expression across the paper's
developmental head-maturation cohorts (Immature / Premature / Mature, split by sex). Pick genes
by ID, by pasting a list, or by searching InterPro/Signature annotation keywords, then generate
bar charts, a hierarchically-clustered heatmap, and functional annotation lookups — all
exportable as publication-ready vector PDFs and CSVs to a folder of your choosing.

Built around **Data 13 (Maturation Counts)**, the study's own comprehensive master expression
matrix, cross-referenced against the paper's InterPro functional annotation.

## What's in this folder

| File | What it is |
|---|---|
| `PlatyHeads_Expression_Dashboard.ipynb` | The dashboard notebook — open this in Jupyter. |
| `_count_elife-41556-fig3-data13-v2.xlsx` | Data 13: the master count matrix from the paper's own supplementary files. |
| `InterproAnnotation_trimmed.csv.gz` | InterPro functional annotation, trimmed to the 4 columns this dashboard actually uses (from the paper's ~180MB full annotation export — see note below). |

**Note on the trimmed annotation file:** the original InterPro annotation export from the paper
ships 15 columns (per-hit alignment coordinates, E-values, GO terms, Pathway IDs, ORF IDs, etc.)
across 963,558 rows — about 180MB, too large to host directly on GitHub. Since this dashboard
only ever reads 4 of those columns, we trimmed and gzip-compressed it to ~6MB with byte-identical
values for every column that's actually used. If you need the full original annotation export
(e.g. for GO-term or pathway analysis outside this dashboard), it's available from the paper's own
supplementary data (`Figure 2 source data 3`).

## Requirements

- Python 3.9+
- `pandas`, `numpy`, `matplotlib`, `seaborn`, `scipy`, `ipywidgets`, `openpyxl`

```bash
pip install pandas numpy matplotlib seaborn scipy ipywidgets openpyxl
```

Then open `PlatyHeads_Expression_Dashboard.ipynb` in Jupyter (`jupyter notebook` or JupyterLab)
and run all cells in order.

## How to use it

1. **Set Demographics & Pairwise Conditions** — filter by sex, pick a predefined developmental
   comparison, or hand-pick a custom combination of Immature/Premature/Mature stages.
2. **Choose Target Transcripts** — filter/click from the gene list, paste a list of IDs, or search
   by annotation keyword (e.g. "kinase", "homeobox").
3. **Generate Visuals** to preview on-screen, or **Export Active Results** to save vector PDFs and
   CSVs to a folder you choose (via the folder-browse button or by typing a path).

A colorblind-safe palette toggle (Purple-Orange / Brown-Teal / Cividis) is available next to the
default red-blue scheme for both the bar charts and the heatmap.

## Normalization

The expression values in Data 13 were normalized upstream by the original study, not by this
notebook — we read the published counts exactly as released rather than re-normalizing anything.
See the Methods section of Schenk et al. 2019 for the exact normalization procedure used.

## BLAST sequence search (optional)

The notebook includes a local BLAST search cell for looking up a pasted nucleotide sequence
against the study's own HAMN01 transcriptome assembly. This requires installing NCBI BLAST+ and
building the search database yourself — the database itself isn't included here (it's a large,
regenerable binary index, not primary data). Full setup instructions, including the exact
`makeblastdb` command, are in the markdown cell above the BLAST cell in the notebook.

## Citation

If you use this dashboard, please cite the original paper:

> Schenk, S. et al. (2019). eLife. https://doi.org/10.7554/eLife.45530
