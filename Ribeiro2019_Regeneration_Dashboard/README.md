# Ribeiro Regeneration Expression Dashboard

Interactive gene-expression explorer for the whole-body regeneration transcriptomics study from:

> Ribeiro, R.P., Ponz-Segrelles, G., Bleidorn, C., Aguado, M.T. (2019). Comparative transcriptomics
> in Syllidae (Annelida) indicates that posterior regeneration and regular growth are comparable,
> while anterior regeneration is a distinct process. *BMC Genomics* 20, 855.
> https://doi.org/10.1186/s12864-019-6223-y

**Authors:** Alina Krawczynski, Rannyele Passos Ribeiro (powered by Claude AI)
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

## Requirements

- Python 3.9+
- `pandas`, `numpy`, `matplotlib`, `seaborn`, `scipy`, `ipywidgets`

```bash
pip install pandas numpy matplotlib seaborn scipy ipywidgets
```

Then open `Regeneration_Expression_Dashboard.ipynb` in Jupyter and run all cells in order.

## How to use it

1. **Choose a Species and Regeneration Conditions** — pick *Syllis gracilis* or *Sphaerosyllis
   hystrix* (the gene picker refreshes to that species' own gene list), then select any custom
   combination of the 3 conditions.
2. **Choose Target Genes** — filter/click from the gene list, paste a list of `TR#|c#_g#` IDs, or
   search by Trinotate BLASTX annotation keyword (e.g. "kinase", "laminin") — scoped to whichever
   species is currently selected.
3. **Generate Visuals** to preview bar charts, the multi-gene heatmap, and annotation results
   on-screen, or **Export Active Results** to save vector PDFs and CSVs to a folder you choose (via
   the folder-browse button or by typing a path).

A colorblind-safe palette toggle (Purple-Orange / Brown-Teal / Cividis) is available next to the
default red-blue scheme for both the bar charts and the heatmap.

## Normalization

Reads for each condition were mapped back to that species' own Trinity assembly and gene-level
counts were TMM-normalized (trimmed mean of M-values). In plain terms, TMM rescales each
condition's library so a handful of very highly-expressed transcripts can't skew the comparison
between conditions — it corrects for differences in sequencing depth and RNA composition so one
condition doesn't just look like it has "more of everything" purely because it was sequenced more
deeply. *Sgracilis* and *Shystrix* gene IDs are two entirely separate ID spaces and are never
comparable to one another directly.

## BLAST sequence search (optional)

The notebook includes a local BLAST search cell for looking up a pasted nucleotide sequence
against whichever species' own Trinity assembly is currently selected. This requires installing
NCBI BLAST+ and building both search databases yourself — the databases themselves aren't included
here (they're large, regenerable binary indexes, not primary data). Full setup instructions,
including the exact `makeblastdb` commands for both species, are in the markdown cell above the
BLAST cell in the notebook.

## Citation

If you use this dashboard, please cite the original paper:

> Ribeiro, R.P., Ponz-Segrelles, G., Bleidorn, C., Aguado, M.T. (2019). Comparative transcriptomics
> in Syllidae (Annelida) indicates that posterior regeneration and regular growth are comparable,
> while anterior regeneration is a distinct process. BMC Genomics 20, 855.
