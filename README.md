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

Each folder is fully standalone — its own notebook, data files, and a detailed README with
step-by-step setup and citation details. **Open the README inside the dashboard folder you want**
for the complete walkthrough.

## Quick start

If you've never used Python or Jupyter before, follow the full "Getting started" section in the
dashboard folder's own README — it walks through everything from installing Python. In short:

1. **Install Python** — the [Miniconda installer](https://www.anaconda.com/download/success)
   (pick your OS, accept the defaults).
2. **Download this project** — green **`<> Code`** button → **Download ZIP**, then unzip it.
3. **Open a terminal** (Anaconda Prompt on Windows; Terminal on macOS/Linux) and `cd` into the
   dashboard folder you want.
4. **Start Jupyter:**
   ```bash
   pip install notebook
   jupyter notebook
   ```
   then click the `.ipynb` file.
5. **Run the notebook.** The first code cell is a **dependency check** that automatically installs
   any missing Python packages — run it first, then do **Kernel → Restart Kernel and Run All
   Cells**. The interactive dashboard appears at the bottom.

### Installing the packages manually (optional)

The dependency-check cell does this for you, but if you'd rather install by hand:

```bash
pip install pandas numpy matplotlib seaborn scipy ipywidgets openpyxl pyreadr
```

(`openpyxl` is only needed by the Schenk and Vianello dashboards; `pyreadr` only by Vianello.)

## BLAST sequence search (optional, per dashboard)

Every dashboard includes an optional cell for looking up a pasted nucleotide sequence against that
study's reference transcriptome. **This is entirely optional** — the rest of each dashboard works
without it. Enabling it requires installing the free [NCBI BLAST+](https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/)
toolkit (or `conda install -c bioconda blast`) and building a small search database once; the
notebook auto-detects BLAST+ on your `PATH` and looks for the database in a `blast_db/` folder next
to the notebook. Full per-study instructions are in each dashboard's README and in the comment at
the top of its BLAST cell.
