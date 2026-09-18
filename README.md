# JAG1-ICD RIME-DIA Proteomics

This repository contains the R code used for the processing, statistical analysis, and visualization of proteomics data generated to characterize the interactome of the endogenous **Jagged-1 intracellular domain (JAG1-ICD)** in the human pancreatic ductal adenocarcinoma cell line **HPAF-II**.

## Experimental overview

The JAG1-ICD interactome was investigated using **Rapid Immunoprecipitation Mass Spectrometry of Endogenous proteins (RIME)**. Two independent antibodies targeting JAG1-ICD were used to increase confidence in the identification of specific interactors, with IgG immunoprecipitation used as a negative control.

RIME samples were analyzed by **data-independent acquisition (DIA) mass spectrometry** on an Orbitrap Exploris 480. Raw mass spectrometry data were processed using directDIA in Spectronaut, and downstream statistical analyses were performed in R.

In addition to the protein-level interactome analysis, peptide-level data were analyzed to assess the enrichment of JAG1 peptides mapping specifically to the intracellular domain.

## Repository structure

```text
JAG1-RIME-proteomics/
├── README.md
├── analysis/
│   ├── RIME_JAG1_analysis.qmd
│   └── JAG1_peptide_mapping.qmd
├── data/
│   ├── HPA_subcellular_experimental_2025.tsv
│   └── HPA_subcellular_predicted_2025.tsv
├── results/
├── plots/
├── .gitignore
└── JAG1-RIME-proteomics.Rproj
```

The `analysis/` directory contains the complete Quarto analysis workflows.

The `data/` directory in this repository contains only the Human Protein Atlas annotations used for subcellular localization analysis. Proteomics input data are not stored on GitHub and should be obtained from PRIDE.

The `results/` and `plots/` directories are generated locally during the analyses and are not tracked by Git.

## Data availability

The mass spectrometry proteomics data and associated Spectronaut output files generated in this study have been deposited to the **ProteomeXchange Consortium** via the **PRIDE** partner repository (Deutsch et al., 2023; Perez-Riverol et al., 2022) with the dataset identifier:

**PXDXXXXXX**

To reproduce the analyses, download the required Spectronaut output files from PRIDE and place them in the local `data/` directory.

The protein-level interactome analysis requires:

```text
Rime_J1_HPAF_SN19_Report_Log2_Protein_Quant_GF_Pivot.tsv
```

The JAG1 peptide-level analysis requires:

```text
20251113_134101_20220819_Report.tsv
```

Subcellular localization annotations were obtained from the **Human Protein Atlas Subcellular resource** in 2025. Experimentally supported subcellular localization data were complemented with predicted localization information for membrane and secreted proteins lacking experimental subcellular localization data. These annotation files are provided directly in the repository.

## Analysis workflows

### JAG1-ICD interactome analysis

The main protein-level analysis is implemented in:

```text
analysis/RIME_JAG1_analysis.qmd
```

The workflow includes:

1. Import and cleaning of Spectronaut protein-group data
2. Quality-control assessment
3. iBAQ-based JAG1 abundance ranking
4. Filtering based on valid values across JAG1-ICD immunoprecipitations
5. Missing-value imputation
6. Median-centering normalization
7. Principal component analysis
8. Differential enrichment analysis using `limma`
9. Combination of evidence from the two independent JAG1-ICD antibodies using Fisher's method
10. Subcellular localization annotation
11. Visualization of enriched proteins and the JAG1-ICD interactome

Proteins are considered significantly enriched in the combined analysis when the Benjamini-Hochberg-adjusted combined p-value is ≤ 0.05 and the mean log2 fold change across the two antibody comparisons is > 1.

### JAG1 peptide mapping analysis

The peptide-level analysis is implemented in:

```text
analysis/JAG1_peptide_mapping.qmd
```

The workflow includes:

1. Extraction of JAG1 peptide observations from the Spectronaut peptide-level report
2. Classification of peptides according to whether they map to the JAG1 intracellular domain
3. Quantification of ICD and non-ICD peptide observations
4. Assessment of ICD and non-ICD peptide detection across individual MS runs
5. Calculation of the contribution of ICD-derived peptides to the total JAG1 MS2 signal
6. Comparison of peptide-level MS2 intensities between ICD and non-ICD peptides

## Running the analyses

Clone or download this repository and open the R project:

```text
JAG1-RIME-proteomics.Rproj
```

Download the required Spectronaut output files from PRIDE and place them in the local `data/` directory as described above.

The analyses can then be reproduced by rendering the corresponding Quarto documents in the `analysis/` directory.

Processed tables are written locally to `results/`, while generated figures are saved to `plots/`. These generated files are excluded from the GitHub repository.

R and package version information used for each analysis is reported at the end of the corresponding Quarto document using `sessionInfo()`.

## Code availability

The analysis code will be archived with a persistent DOI upon publication:

**DOI: XXXXX**