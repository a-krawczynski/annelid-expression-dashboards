# Annelid Transcriptome Expression Dashboards

Five self-contained Jupyter dashboards for interactively exploring published annelid transcriptome
datasets — pick genes by ID, paste a list, or search by annotation keyword, then generate bar charts,
hierarchically-clustered heatmaps, and annotation lookups, all exportable as publication-ready vector
PDFs and CSVs.

**Authors:** Alina Krawczynski, Rannyele Passos Ribeiro (powered by Claude)
**Lab:** Özpolat Lab, Washington University in St. Louis, 1 Brookings Drive, St. Louis, MO 63130
**Year:** 2026

## Dashboards

| Folder | Study | What it explores |
|---|---|---|
| [`Schenk2019_PlatyHeads_Dashboard/`](Schenk2019_PlatyHeads_Dashboard/) | Schenk et al. 2019, *eLife* | Head-maturation transcriptome (Immature/Premature/Mature, by sex) |
| [`Vianello2026_GutExpression_Dashboard/`](Vianello2026_GutExpression_Dashboard/) | Vianello et al. 2026 | Gut expression along 10 anterior-to-posterior segments |
| [`Pare2023_Regeneration_Dashboard/`](Pare2023_Regeneration_Dashboard/) | Paré et al. 2023, *BMC Genomics* | Posterior regeneration across 6 stages (0–5dpa + control) |
| [`Andreatta2026_FoldChange_Dashboard/`](Andreatta2026_FoldChange_Dashboard/) | Andreatta et al. 2026, *PNAS* | log2 fold-change across ~20 genotype-comparison DE analyses |
| [`Ribeiro2019_Regeneration_Dashboard/`](Ribeiro2019_Regeneration_Dashboard/) | Ribeiro et al. 2019, *BMC Genomics* | Whole-body regeneration in two syllid species |

Each folder is fully standalone — its own notebook, data files, and README with setup instructions and
citation details. Open any dashboard's `README.md` for the specifics of that dataset.

## Requirements

- Python 3.9+
- `pandas`, `numpy`, `matplotlib`, `seaborn`, `scipy`, `ipywidgets` (some dashboards additionally need
  `openpyxl` and/or `pyreadr` — see each dashboard's own README)

```bash
pip install pandas numpy matplotlib seaborn scipy ipywidgets openpyxl pyreadr
```

Then open the dashboard notebook you want in Jupyter and run all cells in order.
