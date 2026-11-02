# Vianello Gut Expression Dashboard

Interactive gene-expression explorer for the comparative gut transcriptomics study from:

> Vianello, E. et al. (2026). `doi:10.5281/zenodo.17746910`

**Authors:** Alina Krawczynski, Rannyele Passos Ribeiro (built with assistance from Claude)
**Lab:** Özpolat Lab, Washington University in St. Louis
**Year:** 2026

## What this is

A self-contained Jupyter dashboard for exploring transcript expression along the anterior-to-posterior
axis of the *Platynereis dumerilii* gut (10 segments: mouth → stomodeum → transition1 → esophagus →
transition2 → stomach → transition3 → midgut → transition4 → hindgut). Pick genes by ID, by pasting
a list, or by searching a transcription-factor registry / functional dictionary for a keyword, then
generate bar charts, a hierarchically-clustered heatmap, and annotation lookups. All exportable as
publication-ready vector PDFs and CSVs to a folder of your choosing.

Built around `VST_Pd_corrected.rds`, the study's own variance-stabilized, batch-corrected master
expression matrix.

## What's in this folder

| File | What it is |
|---|---|
| `Gut_Expression_Dashboard.ipynb` | The dashboard notebook. Open this in Jupyter. |
| `VST_Pd_corrected.rds` | Master expression matrix: VST-normalized, batch-corrected counts (37,392 transcripts x 30 samples). |
| `Pd_samplemetadata_standard.rds` | Maps each sample column to its gut segment + replicate number. |
| `Pd_dictionary_standard.rds` | Functional gene dictionary (symbol + description), keyed by `XLOC_...` gene ID. |
| `Pd_TF_ID.xlsx` | Curated transcription-factor registry, keyed by `XLOC_...` gene ID. |

All four data files are small enough (under 10MB combined) to host directly on GitHub as-is. No
trimming was needed for this dataset.

---

## Getting started (no prior Python/Jupyter experience needed)

You only have to do steps 1–2 once per computer.

### 1. Install Python

Download and install **Miniconda** (a small, free Python distribution):
<https://www.anaconda.com/download/success> → "Miniconda Installers" → pick your operating system.
Accept the default options during installation.

- **Windows:** this also installs a program called **Anaconda Prompt** (that's the "terminal"
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
cd Desktop/annelid-expression-dashboards/Vianello2026_GutExpression_Dashboard
```

(Type `cd ` (with a space) then drag the folder onto the terminal window to fill in the path
automatically, then press Enter.)

### 4. Start Jupyter

```bash
pip install notebook
jupyter notebook
```

This opens Jupyter in your web browser. Click **`Gut_Expression_Dashboard.ipynb`** to open the
notebook.

### 5. Run the notebook

The **first code cell is a dependency check**. It automatically installs any Python packages the
notebook needs that you don't already have (including `pyreadr`, which lets Python read the `.rds`
data files directly, no R installation required). Run it first (select it and press
**Shift+Enter**). If it installs anything, it will tell you to restart the kernel: do
**Kernel → Restart Kernel**, then continue.

Then run everything: **Kernel → Restart Kernel and Run All Cells**. Scroll to the bottom. The
interactive dashboard appears under the last cell.

> If you prefer to install the packages yourself instead of letting the first cell do it:
> ```bash
> pip install pandas numpy matplotlib seaborn scipy ipywidgets openpyxl pyreadr
> ```

### 6. Use the dashboard

1. **Choose Gut Segments** — select any custom combination of the 10 anatomical regions (all
   selected by default).
2. **Choose Target Transcripts** — filter/click from the gene list, paste a list of `XLOC_...`
   IDs, or search by TF-registry/dictionary annotation keyword (e.g. "homeobox", "Wnt").
3. Click **🎯 Generate Visuals (View Only)** to preview on-screen, or **💾 Export Active Results**
   to save vector PDFs and CSVs to a folder you choose (via the folder-browse button or by typing
   a path).

A colorblind-safe palette toggle (Purple-Orange / Brown-Teal / Cividis) is available next to the
default red-blue scheme for both the bar charts and the heatmap.

---

## Normalization

Raw reads were mapped to the *Platynereis dumerilii* reference and counted per transcript, then
Variance-Stabilizing-Transformed (VST) and batch-corrected across all 30 samples. In plain terms,
VST re-scales each count so its variance no longer depends on its average expression level (raw
RNA-seq counts are naturally noisier for highly-expressed genes). This keeps low- and
high-abundance transcripts equally comparable and is a standard preprocessing step before
clustering or visualization.

## BLAST sequence search (optional)

The notebook has an optional cell that lets you paste a nucleotide sequence and find the matching
transcript in the study's own **pdumv021** reference transcriptome. **Everything else in the
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
2. **Build the search database once.** The `pdumv021.transcripts.fasta` reference is not included
   here (it isn't publicly distributed. Request it from the study authors). Once you have it, the
   exact `makeblastdb` command is in the comment at the top of the BLAST code cell. Put the
   resulting database files in a folder called **`blast_db/`** next to the notebook. That's where
   the notebook looks by default.

## Citation

If you use this dashboard, please cite the original study:

> Vianello, E. et al. (2026). `doi:10.5281/zenodo.17746910`
