# Paré Regeneration Expression Dashboard

Interactive gene-expression explorer for the posterior-regeneration transcriptomics study from:

> Paré, A. et al. (2023). *BMC Genomics* 24:583. https://doi.org/10.1186/s12864-023-09602-z

**Authors:** Alina Krawczynski, Rannyele Passos Ribeiro (powered by Claude)
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

---

## Getting started (no prior Python/Jupyter experience needed)

You only have to do steps 1–2 once per computer.

### 1. Install Python

Download and install **Miniconda** (a small, free Python distribution):
<https://www.anaconda.com/download/success> → "Miniconda Installers" → pick your operating system.
Accept the default options during installation.

- **Windows:** this also installs a program called **Anaconda Prompt** — that's the "terminal"
  referred to below.
- **macOS / Linux:** use the built-in **Terminal** app.

### 2. Download this project

On the GitHub page for this repository, click the green **`<> Code`** button → **Download ZIP**,
then unzip it somewhere easy to find (e.g. your Desktop). If you know how to use `git`, you can
instead run `git clone <repository-url>`.

### 3. Open a terminal in this folder

Open Anaconda Prompt (Windows) or Terminal (Mac/Linux) and move into this dashboard's folder.
For example, if you unzipped the project onto your Desktop:

```bash
cd Desktop/annelid-expression-dashboards/Pare2023_Regeneration_Dashboard
```

(Type `cd ` — with a space — then drag the folder onto the terminal window to fill in the path
automatically, then press Enter.)

### 4. Start Jupyter

```bash
pip install notebook
jupyter notebook
```

This opens Jupyter in your web browser. Click **`Regeneration_Expression_Dashboard.ipynb`** to
open the notebook.

### 5. Run the notebook

The **first code cell is a dependency check** — it automatically installs any Python packages the
notebook needs that you don't already have. Run it first (select it and press **Shift+Enter**).
If it installs anything, it will tell you to restart the kernel: do **Kernel → Restart Kernel**,
then continue.

Then run everything: **Kernel → Restart Kernel and Run All Cells**. Scroll to the bottom — the
interactive dashboard appears under the last cell.

> If you prefer to install the packages yourself instead of letting the first cell do it:
> ```bash
> pip install pandas numpy matplotlib seaborn scipy ipywidgets
> ```

### 6. Use the dashboard

1. **Choose Regeneration Stages** — select any custom combination of the 6 stages (all selected
   by default).
2. **Choose Target Transcripts** — filter/click from the gene list, paste a list of
   `PladumEVm...` IDs, or search by curated-marker/Trinotate annotation keyword (e.g. "stem
   cell", "Wnt").
3. Click **🎯 Generate Visuals (View Only)** to preview bar charts, the multi-gene heatmap,
   annotation tables, and sequential DEG status tables on-screen, or **💾 Export Active Results**
   to save vector PDFs and CSVs to a folder you choose (via the folder-browse button or by typing
   a path).

A colorblind-safe palette toggle (Purple-Orange / Brown-Teal / Cividis) is available next to the
default red-blue scheme for both the bar charts and the heatmap.

---

## Normalization

Reads from 15 high-quality samples (2-3 replicates per stage; 3 outlier replicates were already
excluded by the original authors) were mapped to the de novo reference transcriptome and
TMM-normalized (trimmed mean of M-values). In plain terms, TMM rescales each sample's library so a
handful of very highly-expressed transcripts can't skew the comparison between samples — it
corrects for differences in sequencing depth and RNA composition so a stage with more total reads
doesn't just look like it has "more of everything."

## BLAST sequence search (optional)

The notebook has an optional cell that lets you paste a nucleotide sequence and find the matching
transcript in the study's own assembled transcriptome. **Everything else in the dashboard works
without this** — if BLAST isn't set up, that one cell just prints a short reminder and nothing
else is affected.

To enable it:

1. **Install NCBI BLAST+** (the free `blastn` / `makeblastdb` / `blastdbcmd` toolkit):
   - **Windows / macOS:** download the installer from
     <https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/> and run it.
   - **conda (any OS):** `conda install -c bioconda blast`
   - The notebook finds BLAST+ automatically as long as it's on your system `PATH` (the installers
     and conda both do this for you). If it's somewhere unusual, set `BLAST_BIN_DIR` at the top of
     the BLAST code cell to the folder containing the executables.
2. **Build the search database once.** The assembled transcriptome FASTA is not included here
   (it's large; it corresponds to ENA TSA accession `HBZZ01`). The exact commands (rewrite the
   FASTA headers, run `makeblastdb`) are in the comment at the top of the BLAST code cell. Put the
   resulting database files in a folder called **`blast_db/`** next to the notebook — that's where
   the notebook looks by default.

## Citation

If you use this dashboard, please cite the original paper:

> Paré, A. et al. (2023). BMC Genomics 24:583. https://doi.org/10.1186/s12864-023-09602-z
