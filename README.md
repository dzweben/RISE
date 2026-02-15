# RISE

## Overview
This repository contains the RISE analysis pipeline for social media behavior (MTES/SSM), reward measures, and downstream regression analyses. The workflow is organized into three main phases: SSM cleaning, data integration, and final analyses, plus a neural data check module.

## Repository Structure
- `SSM/`: Social media data cleaning and MTES/SSM scoring
- `SSM/RISE_datacleaning.Rmd`: Computes MTES composites and SSM outputs
- `data_integration/`: Joins reward variables with demographics/BDI and SSM outputs
- `data_integration/rewardprep.Rmd`: Integration and cleaning script
- `final_analyses/`: Regression analyses and model variants
- `final_analyses/rise_final_analyses.Rmd`: Primary analysis script
- `neural_data_check/`: Neural variables QA and correlations
- `neural_data_check/neural_data_check.Rmd`: QC and correlation checks
- `Syntax1.sps`: SPSS syntax used for ancillary steps

## Data Flow
1. **SSM cleaning** (`SSM/RISE_datacleaning.Rmd`)
   - Reads raw SSM/MTES CSV data.
   - Filters rows with `mtes` in {1, 2}.
   - Recodes survey scales to start at 0 (subtracts 1 from selected MTES items).
   - Builds MTES aggregate variables and computes z-scored composites.
   - Produces cleaned SSM outputs (e.g., `ssm_data.csv`).

2. **Data integration** (`data_integration/rewardprep.Rmd`)
   - Reads BDI/demographics and reward variables.
   - Recodes race/ethnicity/gender fields and standardizes variable names.
   - Merges demographic/BDI data with reward sample info and SSM outputs by `rise_id`.
   - Writes the integrated analysis dataset (e.g., `RISE_ssm.reward.totaldata.csv`).

3. **Final analyses** (`final_analyses/*.Rmd`)
   - Reads the integrated dataset and subsets variables for pre-registered models.
   - Runs regression models with covariates (Age, Sex) and reward/behavioral predictors.
   - Produces tables using `broom`/`flextable` and exports results.

4. **Neural data checks** (`neural_data_check/neural_data_check.Rmd`)
   - Loads neural-ready datasets and checks variable readiness.
   - Generates correlation plots/QA figures for MID variables.

## Running the Pipeline
1. Update absolute paths in each Rmd to match your local data locations.
2. Run `SSM/RISE_datacleaning.Rmd` first to create SSM outputs.
3. Run `data_integration/rewardprep.Rmd` to merge SSM with reward and BDI/demographics.
4. Run one of the `final_analyses/*.Rmd` scripts depending on the analysis variant.
5. Run `neural_data_check/neural_data_check.Rmd` if neural QC is needed.

## Notes on Paths and Reproducibility
The scripts currently use absolute paths (e.g., `/Users/dannyzweben/Desktop/CABLAB_Files/RISE/...`). For portability, define a project root and build paths relative to it.

## Data Handling
Raw datasets and binary documents are ignored by git (see `.gitignore`). Keep sensitive data locally or in a secure storage location and point the scripts to those files when running the analyses.
