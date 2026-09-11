# Ribeiro Regeneration Expression Dashboard

Interactive gene-expression explorer for the whole-body regeneration transcriptomics study from:

> Ribeiro, R.P., Ponz-Segrelles, G., Bleidorn, C., Aguado, M.T. (2019). Comparative transcriptomics
> in Syllidae (Annelida) indicates that posterior regeneration and regular growth are comparable,
> while anterior regeneration is a distinct process. *BMC Genomics* 20, 855.
> https://doi.org/10.1186/s12864-019-6223-y

**Authors:** Alina Krawczynski, Rannyele Passos Ribeiro (powered by Claude)
**Lab:** Özpolat Lab, Washington University in St. Louis, 1 Brookings Drive, St. Louis, MO 63130
**Year:** 2026

## What this is

A self-contained Jupyter dashboard for exploring transcript expression across three regeneration
conditions (Normal / Anterior regeneration / Posterior regeneration) in two syllid annelids:
**Syllis gracilis** and **Sphaerosyllis hystrix**, each with its own independent *de novo* Trinity
assembly. Pick a species, then pick genes by ID, by pasting a list, or by searching Trinotate
BLASTX annotation for a keyword, then generate bar charts, a hierarchically-clustered heatmap, and
annotation lookups — all exportable as publication-ready vector PDFs and CSVs to a folder of your
choosing.

Built around each species' own TMM-normalized gene expression matrix.

## What's in this folder

| File | What it is |
|---|---|
| `Regeneration_Expression_Dashboard.ipynb` | The dashboard notebook — open this in Jupyter. |
| `Sgracilis_condition_gene_matrix.TMM.EXPR.matrix` | *Syllis gracilis* TMM-normalized expression matrix (309,576 genes x 3 conditions). |
| `Shystrix_condition_gene_matrix.TMM.EXPR.matrix` | *Sphaerosyllis hystrix* TMM-normalized expression matrix (179,841 genes x 3 conditions). |
| `Sgracilis_annotation_trimmed.csv.gz` | *S. gracilis* Trinotate functional annotation, pre-processed to gene-level (see note below). |
| `Shystrix_annotation_trimmed.csv.gz` | *S. hystrix* Trinotate functional annotation, pre-processed to gene-level (see note below). |

**Note on the annotation files:** the original Trinotate reports list one row per *transcript*
(with 16 columns including RNAMMER, Pfam, SignalP, TmHMM, eggnog, Kegg, multiple GO-term columns,
and full nucleotide/peptide sequence columns) at 616MB and 438MB for the two species — far too
large to host on GitHub, and mostly bulk this dashboard never reads. We pre-collapsed each report
to one row per *gene* (matching the expression matrix's gene-level ID space, preferring an
annotated isoform when one exists) and pre-parsed the compound BLASTX best-hit field down to a
plain description, then gzip-compressed the result — shrinking both files to under 1.5MB combined
with identical annotation content to what the dashboard would have produced by parsing the raw
reports at runtime.

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
cd Desktop/annelid-expression-dashboards/Ribeiro2019_Regeneration_Dashboard
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

1. **Choose a Species and Regeneration Conditions** — pick *Syllis gracilis* or *Sphaerosyllis
   hystrix* (the gene picker refreshes to that species' own gene list), then select any custom
   combination of the 3 conditions.
2. **Choose Target Genes** — filter/click from the gene list, paste a list of `TR#|c#_g#` IDs, or
   search by Trinotate BLASTX annotation keyword (e.g. "kinase", "laminin") — scoped to whichever
   species is currently selected.
3. Click **🎯 Generate Visuals (View Only)** to preview bar charts, the multi-gene heatmap, and
   annotation results on-screen, or **💾 Export Active Results** to save vector PDFs and CSVs to a
   folder you choose (via the folder-browse button or by typing a path).

A colorblind-safe palette toggle (Purple-Orange / Brown-Teal / Cividis) is available next to the
default red-blue scheme for both the bar charts and the heatmap.

---

## Normalization

Reads for each condition were mapped back to that species' own Trinity assembly and gene-level
counts were TMM-normalized (trimmed mean of M-values). In plain terms, TMM rescales each
condition's library so a handful of very highly-expressed transcripts can't skew the comparison
between conditions — it corrects for differences in sequencing depth and RNA composition so one
condition doesn't just look like it has "more of everything" purely because it was sequenced more
deeply. *Sgracilis* and *Shystrix* gene IDs are two entirely separate ID spaces and are never
comparable to one another directly.

## BLAST sequence search (optional)

The notebook has an optional cell that lets you paste a nucleotide sequence and find the matching
transcript in whichever species' own Trinity assembly is currently selected. **Everything else in
the dashboard works without this** — if BLAST isn't set up, that one cell just prints a short
reminder and nothing else is affected.

To enable it:

1. **Install NCBI BLAST+** (the free `blastn` / `makeblastdb` / `blastdbcmd` toolkit):
   - **Windows / macOS:** download the installer from
     <https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/> and run it.
   - **conda (any OS):** `conda install -c bioconda blast`
   - The notebook finds BLAST+ automatically as long as it's on your system `PATH` (the installers
     and conda both do this for you). If it's somewhere unusual, set `BLAST_BIN_DIR` at the top of
     the BLAST code cell to the folder containing the executables.
2. **Build the two search databases once** (one per species). The species transcriptome FASTAs are
   not included here (they're large). The exact commands — rewrite the FASTA headers, then run
   `makeblastdb` for each species — are in the comment at the top of the BLAST code cell. Put the
   resulting database files in a folder called **`blast_db/`** next to the notebook, named
   `ribeiro_sgracilis` and `ribeiro_shystrix` — that's where the notebook looks by default.

## Citation

If you use this dashboard, please cite the original paper:

> Ribeiro, R.P., Ponz-Segrelles, G., Bleidorn, C., Aguado, M.T. (2019). Comparative transcriptomics
> in Syllidae (Annelida) indicates that posterior regeneration and regular growth are comparable,
> while anterior regeneration is a distinct process. BMC Genomics 20, 855.
