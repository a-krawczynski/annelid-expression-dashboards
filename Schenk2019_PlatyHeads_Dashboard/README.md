# PlatyHeads Expression Dashboard

Interactive gene-expression explorer for the head-maturation transcriptome from:

> Schenk, S. et al. (2019). *eLife*. https://doi.org/10.7554/eLife.45530

**Authors:** Alina Krawczynski, Rannyele Passos Ribeiro (built with assistance from Claude)
**Lab:** Özpolat Lab, Washington University in St. Louis
**Year:** 2026

## What this is

A self-contained Jupyter dashboard for exploring transcript expression across the paper's
developmental head-maturation cohorts (Immature / Premature / Mature, split by sex). Pick genes
by ID, by pasting a list, or by searching InterPro/Signature annotation keywords, then generate
bar charts, a hierarchically-clustered heatmap, and functional annotation lookups. All
exportable as publication-ready vector PDFs and CSVs to a folder of your choosing.

Built around **Data 13 (Maturation Counts)**, the study's own comprehensive master expression
matrix, cross-referenced against the paper's InterPro functional annotation.

## What's in this folder

| File | What it is |
|---|---|
| `PlatyHeads_Expression_Dashboard.ipynb` | The dashboard notebook. Open this in Jupyter. |
| `_count_elife-41556-fig3-data13-v2.xlsx` | Data 13: the master count matrix from the paper's own supplementary files. |
| `InterproAnnotation_trimmed.csv.gz` | InterPro functional annotation, trimmed to the 4 columns this dashboard actually uses (from the paper's ~180MB full annotation export. See note below). |

**Note on the trimmed annotation file:** the original InterPro annotation export from the paper
ships 15 columns (per-hit alignment coordinates, E-values, GO terms, Pathway IDs, ORF IDs, etc.)
across 963,558 rows (about 180MB), too large to host directly on GitHub. Since this dashboard
only ever reads 4 of those columns, we trimmed and gzip-compressed it to ~6MB with byte-identical
values for every column that's actually used. If you need the full original annotation export
(e.g. for GO-term or pathway analysis outside this dashboard), it's available from the paper's own
supplementary data (`Figure 2 source data 3`).

---

## Getting started (no prior Python/Jupyter experience needed)

You only have to do steps 1–2 once per computer.

### 1. Install Python

Download and install **Miniconda** (a small, free Python distribution):
<https://www.anaconda.com/download/success> → "Miniconda Installers" → pick your operating system.
Accept the default options during installation.

- **Windows:** this also installs a program called **Anaconda Prompt** (the "terminal"
  referred to below).
- **macOS / Linux:** use the built-in **Terminal** app.

### 2. Download this project

On the GitHub page for this repository, click the green **`<> Code`** button → **Download ZIP**,
then unzip it somewhere easy to find (e.g. your Desktop). If you know how to use `git`, you can
instead run `git clone <repository-url>`.

### 3. Open a terminal in this folder

Open Anaconda Prompt (Windows) or Terminal (Mac/Linux) and move into this dashboard's folder.
For example, if you unzipped the project onto your Desktop:

```bash
cd Desktop/annelid-expression-dashboards/Schenk2019_PlatyHeads_Dashboard
```

(Type `cd ` (with a space) then drag the folder onto the terminal window to fill in the path
automatically, then press Enter.)

### 4. Start Jupyter

```bash
pip install notebook
jupyter notebook
```

This opens Jupyter in your web browser. Click **`PlatyHeads_Expression_Dashboard.ipynb`** to open
the notebook.

### 5. Run the notebook

The **first code cell is a dependency check**. It automatically installs any Python packages the
notebook needs that you don't already have. Run it first (select it and press **Shift+Enter**).
If it installs anything, it will tell you to restart the kernel: do **Kernel → Restart Kernel**,
then continue.

Then run everything: **Kernel → Restart Kernel and Run All Cells**. Scroll to the bottom. The
interactive dashboard appears under the last cell.

> If you prefer to install the packages yourself instead of letting the first cell do it:
> ```bash
> pip install pandas numpy matplotlib seaborn scipy ipywidgets openpyxl
> ```

### 6. Use the dashboard

1. **Set Demographics & Pairwise Conditions** — filter by sex, pick a predefined developmental
   comparison, or hand-pick a custom combination of Immature/Premature/Mature stages.
2. **Choose Target Transcripts** — filter/click from the gene list, paste a list of IDs, or search
   by annotation keyword (e.g. "kinase", "homeobox").
3. Click **🎯 Generate Visuals (View Only)** to preview on-screen, or **💾 Export Active Results**
   to save vector PDFs and CSVs to a folder you choose (via the folder-browse button or by typing
   a path).

A colorblind-safe palette toggle (Purple-Orange / Brown-Teal / Cividis) is available next to the
default red-blue scheme for both the bar charts and the heatmap.

---

## Normalization

The expression values in Data 13 were normalized upstream by the original study, not by this
notebook. We read the published counts exactly as released rather than re-normalizing anything.
See the Methods section of Schenk et al. 2019 for the exact normalization procedure used.

## BLAST sequence search (optional)

The notebook has an optional cell that lets you paste a nucleotide sequence and find the matching
transcript in the study's own **HAMN01** transcriptome assembly. **Everything else in the
dashboard works without this**. If BLAST isn't set up, that one cell just prints a short
reminder and nothing else is affected.

To enable it:

1. **Install NCBI BLAST+** (the free `blastn` / `makeblastdb` / `blastdbcmd` toolkit):
   - **Windows / macOS:** download the installer from
     <https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/> and run it.
   - **conda (any OS):** `conda install -c bioconda blast`
   - The notebook finds BLAST+ automatically as long as it's on your system `PATH` (the installers
     and conda both do this for you). If it's somewhere unusual, set `BLAST_BIN_DIR` at the top of
     the BLAST code cell to the folder containing the executables.
2. **Build the search database once.** The exact commands (download the HAMN01 FASTA, rewrite its
   headers, run `makeblastdb`) are in the comment at the top of the BLAST code cell. Put the
   resulting database files in a folder called **`blast_db/`** next to the notebook. That's where
   the notebook looks by default.

## Citation

If you use this dashboard, please cite the original paper:

> Schenk, S. et al. (2019). eLife. https://doi.org/10.7554/eLife.45530
