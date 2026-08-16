# Paré Regeneration Expression Dashboard

Interactive gene-expression explorer for the posterior-regeneration transcriptomics study from:

> Paré, A. et al. (2023). *BMC Genomics* 24:583. https://doi.org/10.1186/s12864-023-09602-z

**Authors:** Alina Krawczynski, Rannyele Passos Ribeiro (powered by Claude AI)
**Lab:** Özpolat Lab, Washington University in St. Louis, 1 Brookings Drive, St. Louis, MO 63130
**Year:** 2026

## What this is

A self-contained Jupyter dashboard for exploring transcript expression across six *Platynereis
dumerilii* posterior-regeneration stages (0dpa → 1dpa → 2dpa → 3dpa → 5dpa, plus a non-amputated
control). Pick genes by ID, by pasting a list, or by searching curated marker/Trinotate BLASTX
annotation for a keyword, then generate bar charts, a hierarchically-clustered heatmap, functional
annotation lookups, and per-gene differential-expression status across five sequential stage
comparisons — all exportable as publication-ready vector PDFs and CSVs to a folder of your
choosing.

Built around the study's own TMM-normalized master expression matrix.

## What's in this folder

| File | What it is |
|---|---|
| `Regeneration_Expression_Dashboard.ipynb` | The dashboard notebook — open this in Jupyter. |
| `tmm_expression_matrix.csv` | Master TMM-normalized expression matrix (119,737 genes x 15 samples). |
| `sample_metadata.csv` | Maps each sample column to its regeneration stage + replicate number. |
| `curated_marker_genes.csv` | Curated neurogenesis/stem-cell marker gene list with the paper's own precomputed DEG calls. |
| `deg_all_comparisons.csv` | Full differential-expression results table across all pairwise stage comparisons. |
| `gene_annotation_trinotate_trimmed.csv.gz` | Trinotate functional annotation, trimmed to the 2 columns this dashboard actually uses (see note below). |

**Note on the trimmed annotation file:** the original Trinotate annotation export carries 16
columns (transcript IDs, RNAMMER, Pfam, SignalP, TmHMM, eggnog, Kegg, multiple GO-term columns,
etc.) at 120MB — too large to host directly on GitHub. Since this dashboard only ever reads the
gene ID and the parsed BLASTX best-hit description, we trimmed it to just those two columns and
gzip-compressed it to under 1MB, with byte-identical values for the columns actually used.

## Requirements

- Python 3.9+
- `pandas`, `numpy`, `matplotlib`, `seaborn`, `scipy`, `ipywidgets`

```bash
pip install pandas numpy matplotlib seaborn scipy ipywidgets
```

Then open `Regeneration_Expression_Dashboard.ipynb` in Jupyter and run all cells in order.

## How to use it

1. **Choose Regeneration Stages** — select any custom combination of the 6 stages (all selected
   by default).
2. **Choose Target Transcripts** — filter/click from the gene list, paste a list of
   `PladumEVm...` IDs, or search by curated-marker/Trinotate annotation keyword (e.g. "stem
   cell", "Wnt").
3. **Generate Visuals** to preview bar charts, the multi-gene heatmap, annotation tables, and
   sequential DEG status tables on-screen, or **Export Active Results** to save vector PDFs and
   CSVs to a folder you choose (via the folder-browse button or by typing a path).

A colorblind-safe palette toggle (Purple-Orange / Brown-Teal / Cividis) is available next to the
default red-blue scheme for both the bar charts and the heatmap.

## Normalization

Reads from 15 high-quality samples (2-3 replicates per stage; 3 outlier replicates were already
excluded by the original authors) were mapped to the de novo reference transcriptome and
TMM-normalized (trimmed mean of M-values). In plain terms, TMM rescales each sample's library so a
handful of very highly-expressed transcripts can't skew the comparison between samples — it
corrects for differences in sequencing depth and RNA composition so a stage with more total reads
doesn't just look like it has "more of everything."

## BLAST sequence search (optional)

The notebook includes a local BLAST search cell for looking up a pasted nucleotide sequence
against the study's own assembled transcriptome. This requires installing NCBI BLAST+ and
building the search database yourself — the database itself isn't included here (it's a large,
regenerable binary index, not primary data). Full setup instructions, including the exact
`makeblastdb` command, are in the markdown cell above the BLAST cell in the notebook.

## Citation

If you use this dashboard, please cite the original paper:

> Paré, A. et al. (2023). BMC Genomics 24:583. https://doi.org/10.1186/s12864-023-09602-z
