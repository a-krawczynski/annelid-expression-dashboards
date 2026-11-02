# Andreatta Fold-Change Comparison Dashboard

Interactive fold-change explorer for the genotype-comparison transcriptomics study from:

> Andreatta, G. et al. (2026). *PNAS* 123(12):e2514719123. https://doi.org/10.1073/pnas.2514719123

**Authors:** Alina Krawczynski, Rannyele Passos Ribeiro (built with assistance from Claude)

**Lab:** Özpolat Lab, Washington University in St. Louis

**Year:** 2026

## What this is

A self-contained Jupyter dashboard for exploring log2 fold-change results across ~20 pairwise
differential-expression comparisons between specific *Platynereis dumerilii* genotypes
(l-cry⁻ᐟ⁻, nr0b1/2⁻ᐟ⁻, nr0b1/2⁺ᐟ⁻, and their respective wild types). Unlike the other dashboards
in this series, there's no single expression matrix here. Instead, pick genes by raw ID or by a
curated gene symbol, then generate significance-colored bar charts, a hierarchically-clustered
log2FC heatmap, and annotation lookups; all exportable as publication-ready vector PDFs and CSVs
to a folder of your choosing.

Built directly from the paper's own precomputed differential-expression tables (PNAS Datasets S1,
S3, S4), which the notebook reads from the original supplementary files each time it runs.

## What's in this folder

| File | What it is |
|---|---|
| `Fold_Change_Comparison_Dashboard.ipynb` | The dashboard notebook — open this in Jupyter. |
| `pnas.2514719123.sd01.xlsx` | PNAS Dataset S1: l-cry DE results, immature and premature female heads (4 sheets). |
| `pnas.2514719123.sd03.xlsx` | PNAS Dataset S3: l-cry and nr0b1/2 DE results, immature heads, two independent analyses (8 sheets). |
| `pnas.2514719123.sd04.xlsx` | PNAS Dataset S4: nr0b1/2 and l-cry DE results, immature worms (8 sheets). |
| `pnas.2514719123.sd05.xlsx` | PNAS Dataset S5: the authors' curated genes of interest with clean gene symbols, used for symbol-based search. |

**About the data files and their license.** The four `.xlsx` files are the paper's supplementary
Datasets S1, S3, S4 and S5, included exactly as published and unmodified. They are © the authors and
distributed under the [Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 license
(CC BY-NC-ND 4.0)](https://creativecommons.org/licenses/by-nc-nd/4.0/); the originals are available
from the paper's Supporting Information at <https://doi.org/10.1073/pnas.2514719123>. Because that
license does not allow sharing modified versions, this repository does not include any combined or
cleaned copy of the data. Instead, when the notebook runs it reads the 20 DE sheets and the 4 curated
sheets, combines them, removes the duplicate rows present in Dataset S3A as published, and parses the
annotation strings, all in memory. Nothing modified is written to disk. Loading takes about 20 to 30
seconds. The license on these four files is separate from the license on this repository's own code.

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
cd Desktop/annelid-expression-dashboards/Andreatta2026_FoldChange_Dashboard
```

(Type `cd ` with a space then drag the folder onto the terminal window to fill in the path
automatically, then press Enter.)

### 4. Start Jupyter

```bash
pip install notebook
jupyter notebook
```

This opens Jupyter in your web browser. Click **`Fold_Change_Comparison_Dashboard.ipynb`** to open
the notebook.

### 5. Run the notebook

The **first code cell is a dependency check** — it automatically installs any Python packages the
notebook needs that you don't already have. Run it first (select it and press **Shift+Enter**).
If it installs anything, it will tell you to restart the kernel: do **Kernel → Restart Kernel**,
then continue.

Then run everything: **Kernel → Restart Kernel and Run All Cells**. Scroll to the bottom, the
interactive dashboard appears under the last cell.

> If you prefer to install the packages yourself instead of letting the first cell do it:
> ```bash
> pip install pandas numpy matplotlib seaborn scipy ipywidgets openpyxl
> ```

### 6. Use the dashboard

1. **Choose a Pairwise Comparison, then its Details** — pick one genotype comparison, then any
   subset of its tissue/mapping/DE-analysis variants.
2. **Choose Target Genes** — filter/click from the gene list, paste raw IDs or gene symbols, or
   search by annotation keyword (e.g. "cytochrome", "kinase").
3. Click **🎯 Generate Visuals (View Only)** to preview fold-change bar charts, the multi-gene
   heatmap, and annotation tables on-screen, or **💾 Export Active Results** to save vector PDFs
   and CSVs to a folder you choose (via the folder-browse button or by typing a path).

A colorblind-safe palette toggle (Purple-Orange / Brown-Teal / Cividis) is available next to the
default red-blue scheme for both the bar charts and the heatmap.

---

## Normalization

This dashboard doesn't show absolute expression at all, so there's no normalization step
performed here. Every log2FC/padj value plotted is read directly from the paper's own precomputed
differential-expression tables, which the paper generated with its own upstream normalization and
DE-testing pipeline; see the paper's Methods for that procedure.

## BLAST sequence search (optional)

The notebook has an optional cell that lets you paste a nucleotide sequence and find the matching
transcript in the **HAMN01** reference transcriptome (the assembly this dashboard's
transcriptome-mapped gene IDs were generated against). **Everything else in the dashboard works
without this**: if BLAST isn't set up, that one cell just prints a short reminder and nothing
else is affected.

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
   resulting database files in a folder called **`blast_db/`** next to the notebook; that's where
   the notebook looks by default.

Note this only covers transcriptome-mapped (`comp..._c#_seq#`) genes — genome-mapped (`XLOC_...`)
genes aren't searchable this way, since no matching genome reference sequence is available.

## Citation

If you use this dashboard, please cite the original paper:

> Andreatta, G. et al. (2026). PNAS 123(12):e2514719123. https://doi.org/10.1073/pnas.2514719123
