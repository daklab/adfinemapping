# Integration of Deep Learning Annotations with Functional Genomics Improves Identification of Causal Alzheimer's Disease Variants

Code and data for reproducing figures from:

> Lakhani et al. *Integration of Deep Learning Annotations with Functional Genomics Improves Identification of Causal Alzheimer's Disease Variants*
> [https://www.medrxiv.org/content/10.1101/2025.03.07.25323578v1](https://www.medrxiv.org/content/10.1101/2025.03.07.25323578v1)

## Data

All public data files are available on Zenodo:

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.19226023.svg)](https://doi.org/10.5281/zenodo.19226023)

### Download all files

Using [zenodo_get](https://github.com/dvolgyes/zenodo_get):

```bash
pip install zenodo_get
mkdir adfinemapping_public && cd adfinemapping_public
zenodo_get 10.5281/zenodo.19226023
```

Or using the Zenodo API directly:

```bash
pip install requests
python -c "
import requests, os
r = requests.get('https://zenodo.org/api/records/19226023')
for f in r.json()['files']:
    print(f'Downloading {f[\"key\"]}...')
    resp = requests.get(f['links']['self'], stream=True)
    with open(f['key'], 'wb') as out:
        for chunk in resp.iter_content(8192):
            out.write(chunk)
"
```

The dataset is ~4.5 GB across 32 files. All files are downloaded into a single flat directory. See `zenodo_description.txt` for a complete file-by-file description with column definitions.

## Reproducing Figures

The public notebook `process_focal_analysis_public.Rmd` reproduces all main and supplementary figures from the downloaded data.

### Requirements

R (>= 4.0) with the following packages:

```r
install.packages(c("tidyverse", "data.table", "arrow", "patchwork",
                    "ggsci", "ggrepel", "scales", "PRROC"))

# Bioconductor packages
if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")
BiocManager::install(c("GenomicRanges", "IRanges", "ComplexUpset"))
```

### Running

1. Download the Zenodo data to a local directory
2. Open `process_focal_analysis_public.Rmd`
3. Set `DATA_DIR` to the path where you downloaded the files
4. Knit or run all chunks

## Repository Structure

| File | Description |
|---|---|
| `process_focal_analysis_public.Rmd` | Public notebook — reproduces all figures from Zenodo data |
| `process_focal_analysis_prepublic.Rmd` | Preprocessing notebook — aggregates protected data into public-ready files |
| `process_focal_analysis_v11.Rmd` | Full analysis notebook (requires internal data access) |
| `copy_data_for_zenodo.sh` | Copies raw data files to the public release directory |
| `upload_to_zenodo.py` | Uploads files from the public directory to Zenodo |
| `zenodo_env.yml` | Conda environment for the upload script |
| `zenodo_description.txt` | Zenodo dataset description with file details |
| `zenodo_data_inventory.md` | Internal inventory of all public release files |

## Data Overview

### LDSC Annotation Results (4 files)

Stratified LD Score Regression heritability enrichment across seven annotation types (Baseline, Glass Epigenomics, Roadmap, DeepSea, Enformer Tensorflow, Enformer TF/Glass, ChromBPNet) and three GWAS (Kunkle 2019, Wightman 2021, Bellenguez 2022). Includes multivariate LDSC with jointly-estimated tau*.

### Fine-Mapping Results (4 files)

SuSiE/PolyFun results for Bellenguez 2022 under four annotation models: SuSiE (flat prior), Baseline, Baseline+Omics, Baseline+Omics+DL. Per-variant PIPs, credible sets, and effect sizes.

### PRS Summaries (4 files)

Polygenic risk score AUC, odds ratios, and prevalence by decile across five ancestries (EUR, AFR, AMR, EAS, SAS). Aggregate statistics only (no individual-level data).

### Cell-Type Annotations (9 files)

ATAC-seq peaks (4 cell types), ABC enhancer-gene predictions (4 cell types), and PLAC-seq chromatin interactions. Brain cell types: microglia, oligodendrocyte, astrocyte, neuron.

### caQTL Predictions (8 files)

ChromBPNet and Enformer predicted allelic effects on chromatin accessibility for fine-mapped microglia variants under different centering strategies. Includes motifBreakR TF motif disruption analysis.

### Other (3 files)

Bellenguez GWAS summary statistics (hg38, parquet) and finemapped variant annotation tables (FAVOR).

## Citation

If you use this data or code, please cite:

```
Lakhani et al. Integration of Deep Learning Annotations with Functional Genomics
Improves Identification of Causal Alzheimer's Disease Variants. medRxiv (2025).
https://doi.org/10.1101/2025.03.07.25323578
```
