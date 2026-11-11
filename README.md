# SPFuse: multirepresentation antibacterial activity modeling

SPFuse combines LightGBM predictions from molecular fingerprints (P) and physicochemical/topological descriptors (D). The archive in this repository contains experiment code, configurations and aggregate results.

## Models and code

Trained SPFuse classification and MIC models, retrained baseline checkpoints, split identifiers and selected out-of-fold predictions are available in the [Zenodo archive, version 1.0.1](https://doi.org/10.5281/zenodo.24134685). Download the parts and follow README_REASSEMBLY.txt there to reassemble and verify the archives. Version 1.0.0 contains redundant parts; use version 1.0.1.

The code archive in this repository is SPFuse_code_data_links_20261111.zip. It contains source_snapshot/ (frozen scripts and protocols), spfuse.py (training, prediction and verification entry point), data/feature_schema.json, training_config.json, requirements_spfuse.txt, results/ and analysis/. Third-party code retains its original licenses.

## Data sources

- S. aureus benchmark: [Onawole et al. data and original models](https://zenodo.org/records/18463329), [original project code](https://github.com/MujeebOnawole/XAI_Evaluation_Framework_for_Drug_Discovery), and [paper](https://doi.org/10.1186/s13321-026-01200-x).
- MIC regression: [SPARK downloads](https://www.collaborativedrug.com/spark-data-downloads), Achaogen LpxC inhibitor contribution, paired E. coli ATCC 25922 and P. aeruginosa ATCC 27853 records. SPARK data are subject to the [SPARK Data Terms of Use](https://www.collaborativedrug.com/hubfs/SPARK-data-terms-of-use.pdf).

## Reproduction status

The code snapshot alone is not a complete one-command reproduction package. The Zenodo model archive includes weights, feature schema, selected out-of-fold predictions and split identifiers, but excludes third-party raw and processed assay data, feature matrices and full reference prediction tables. Training and verification require reconstructing these omitted inputs; some historical scripts require additional project-relative artifacts. See the model archive README for details.

The recorded primary environment was Python 3.10.20, RDKit 2022.03.3, NumPy 1.26.3, pandas 2.2.3, SciPy 1.15.3, scikit-learn 1.6.1, LightGBM 4.7.0 and joblib 1.6.0. Baseline and attribution workflows have additional requirements. Upstream data and software remain subject to their original terms.

## Model structure

Classification features comprise 85 fragment counts (F), 6,311 fingerprint bits (P) and 123 non-fragment descriptors (D); SPFuse uses P and D. Classification uses a logistic model fitted to out-of-fold branch log-odds. MIC models are trained separately for each species and combine branch predictions using ridge regression. Molecular-pair and series analyses evaluate changes in the difference between species-specific log2 MIC values.
