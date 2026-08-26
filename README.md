# JAG1-ICD RIME-DIA Proteomics

This repository contains the R code used for the processing, statistical analysis, and visualization of the proteomics data generated to characterize the interactome of the endogenous **Jagged-1 intracellular domain (JAG1-ICD)** in the human pancreatic adenocarcinoma cell line **HPAF-II**.

## Experimental overview

The JAG1-ICD interactome was investigated using **Rapid Immunoprecipitation Mass Spectrometry of Endogenous proteins (RIME)**. Two independent antibodies targeting JAG1-ICD were used to increase confidence in the identification of specific interactors, with IgG immunoprecipitation used as a negative control.

RIME samples were analyzed by **data-independent acquisition (DIA) mass spectrometry** on an Orbitrap Exploris 480. Raw data were processed using directDIA in Spectronaut, and downstream statistical analyses were performed in R.

## Repository structure

``` text
JAG1-RIME-proteomics/
├── analysis/
│   └── RIME_JAG1_analysis.qmd
├── data/
│   ├── HPA_subcellular_experimental_2025.tsv
│   └── HPA_subcellular_predicted_2025.tsv
├── results/
├── plots/
└── JAG1-RIME-proteomics.Rproj
```

The `analysis/` directory contains the complete Quarto analysis workflow.

The `data/` directory in this repository contains only the Human Protein Atlas annotations used for subcellular localization analysis. Proteomics input data are not stored on GitHub and should be obtained from PRIDE.

The `results/` and `plots/` directories are generated locally during the analysis and are not tracked by Git.

## Data availability

The mass spectrometry proteomics data and associated Spectronaut output files generated in this study have been deposited to the **ProteomeXchange Consortium** via the **PRIDE** partner repository with the dataset identifier:

**PXDXXXXXX**

To reproduce the analysis, download the required Spectronaut protein-group output file from PRIDE:

``` text
Rime_J1_HPAF_SN19_Report_Log2_Protein_Quant_GF_Pivot.tsv
```

and place it in the local `data/` directory.

Subcellular localization annotations were obtained from the **Human Protein Atlas Subcellular resource** in 2025. Experimentally supported subcellular localization data were complemented with predicted localization information for membrane and secreted proteins lacking experimental subcellular localization data. These annotation files are included in the repository.

## Analysis workflow

The analysis is implemented in:

``` text
analysis/RIME_JAG1_analysis.qmd
```

The workflow includes:

1.  Import and cleaning of Spectronaut protein-group data
2.  Quality-control assessment
3.  iBAQ-based protein abundance ranking
4.  Filtering based on valid values across JAG1-ICD immunoprecipitations
5.  Missing-value imputation
6.  Median-centering normalization
7.  Principal component analysis
8.  Differential enrichment analysis using `limma`
9.  Combination of evidence from the two independent JAG1-ICD antibodies using Fisher's method
10. Subcellular localization annotation
11. Visualization of enriched proteins and the JAG1-ICD interactome

Proteins are considered significantly enriched in the combined analysis when the Benjamini-Hochberg-adjusted combined p-value is ≤ 0.05 and the mean log2 fold change across the two antibody comparisons is \> 1.

## Running the analysis

Clone or download this repository and open:

``` text
JAG1-RIME-proteomics.Rproj
```

Download the required Spectronaut output from PRIDE and place it in `data/` as described above.

The complete analysis can then be run by rendering:

``` text
analysis/RIME_JAG1_analysis.qmd
```

The analysis writes processed tables to `results/` and generated figures to `plots/`.

Package and R version information used for the analysis is reported at the end of the Quarto document using `sessionInfo()`.

## Code availability

The analysis code is archived at:

**DOI: XXXXX**

The DOI will be updated upon archival of the final version of the repository.
