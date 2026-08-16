# Vianello Gut Expression Dashboard

Interactive gene-expression explorer for the comparative gut transcriptomics study from:

> Vianello, E. et al. (2026). `doi:10.5281/zenodo.17746910`

**Authors:** Alina Krawczynski, Rannyele Passos Ribeiro (powered by Claude AI)
**Lab:** Özpolat Lab, Washington University in St. Louis, 1 Brookings Drive, St. Louis, MO 63130
**Year:** 2026

## What this is

A self-contained Jupyter dashboard for exploring transcript expression along the anterior-to-posterior
axis of the *Platynereis dumerilii* gut (10 segments: mouth → stomodeum → transition1 → esophagus →
transition2 → stomach → transition3 → midgut → transition4 → hindgut). Pick genes by ID, by pasting
a list, or by searching a transcription-factor registry / functional dictionary for a keyword, then
generate bar charts, a hierarchically-clustered heatmap, and annotation lookups — all exportable as
publication-ready vector PDFs and CSVs to a folder of your choosing.

Built around `VST_Pd_corrected.rds`, the study's own variance-stabilized, batch-corrected master
expression matrix.

## What's in this folder

| File | What it is |
|---|---|
| `Gut_Expression_Dashboard.ipynb` | The dashboard notebook — open this in Jupyter. |
| `VST_Pd_corrected.rds` | Master expression matrix: VST-normalized, batch-corrected counts (37,392 transcripts x 30 samples). |
| `Pd_samplemetadata_standard.rds` | Maps each sample column to its gut segment + replicate number. |
| `Pd_dictionary_standard.rds` | Functional gene dictionary (symbol + description), keyed by `XLOC_...` gene ID. |
| `Pd_TF_ID.xlsx` | Curated transcription-factor registry, keyed by `XLOC_...` gene ID. |

All four files are small enough (under 10MB combined) to host directly on GitHub as-is — no
trimming was needed for this dataset.

## Requirements

- Python 3.9+
- `pandas`, `numpy`, `matplotlib`, `seaborn`, `scipy`, `ipywidgets`, `openpyxl`, `pyreadr`

```bash
pip install pandas numpy matplotlib seaborn scipy ipywidgets openpyxl pyreadr
```

`pyreadr` is required to read the `.rds` (R data) files directly in Python without needing R
installed. Then open `Gut_Expression_Dashboard.ipynb` in Jupyter and run all cells in order.

## How to use it

1. **Choose Gut Segments** — select any custom combination of the 10 anatomical regions (all
   selected by default).
2. **Choose Target Transcripts** — filter/click from the gene list, paste a list of `XLOC_...`
   IDs, or search by TF-registry/dictionary annotation keyword (e.g. "homeobox", "Wnt").
3. **Generate Visuals** to preview on-screen, or **Export Active Results** to save vector PDFs and
   CSVs to a folder you choose (via the folder-browse button or by typing a path).

A colorblind-safe palette toggle (Purple-Orange / Brown-Teal / Cividis) is available next to the
default red-blue scheme for both the bar charts and the heatmap.

## Normalization

Raw reads were mapped to the *Platynereis dumerilii* reference and counted per transcript, then
Variance-Stabilizing-Transformed (VST) and batch-corrected across all 30 samples. In plain terms,
VST re-scales each count so its variance no longer depends on its average expression level (raw
RNA-seq counts are naturally noisier for highly-expressed genes) — this keeps low- and
high-abundance transcripts equally comparable and is a standard preprocessing step before
clustering or visualization.

## BLAST sequence search (optional)

The notebook includes a local BLAST search cell for looking up a pasted nucleotide sequence
against the study's own pdumv021 reference transcriptome. This requires installing NCBI BLAST+ and
building the search database yourself — the database itself isn't included here (it's a large,
regenerable binary index, not primary data). Full setup instructions, including the exact
`makeblastdb` command, are in the markdown cell above the BLAST cell in the notebook.

## Citation

If you use this dashboard, please cite the original study:

> Vianello, E. et al. (2026). `doi:10.5281/zenodo.17746910`
