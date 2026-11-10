# SPFuse: multirepresentation antibacterial activity modeling

SPFuse combines the predictions of LightGBM models trained on structural fingerprints (P) and physicochemical/topological descriptors (D). The downloadable archive `SPFuse_code_data_links_20261111.zip` contains experiment code, fixed training configuration and aggregate results for the classification and paired minimum inhibitory concentration (MIC) analyses.

## Data sources

- **S. aureus classification benchmark:** [Onawole et al. data and original models (Zenodo)](https://zenodo.org/records/18463329); [original project code](https://github.com/MujeebOnawole/XAI_Evaluation_Framework_for_Drug_Discovery); [published paper](https://doi.org/10.1186/s13321-026-01200-x). Use the released molecule-level training/validation/test labels for the released partition.
- **MIC regression:** [SPARK public data downloads](https://www.collaborativedrug.com/spark-data-downloads). The analysis uses the Achaogen LpxC inhibitor contribution, paired *E. coli* ATCC 25922 and *P. aeruginosa* ATCC 27853 records, and the filtering and aggregation recorded in `source_snapshot/mic_regression/runs/achaogen_ec_pa_v1_20261108/`.
- SPARK data are subject to [SPARK Data Terms of Use](https://www.collaborativedrug.com/hubfs/SPARK-data-terms-of-use.pdf). The underlying datasets are linked rather than redistributed in this repository.

## Contents

Download and extract the archive to access the paths below.

- `source_snapshot/`: frozen analysis, training and evaluation code and recorded protocols, arranged by experiment. Third-party baseline sources retain their original license files.
- `spfuse.py`: portable training, inference and verification entry point from the model handoff.
- `data/feature_schema.json`: feature names and ordering.
- `training_config.json`, `requirements_spfuse.txt`: recorded SPFuse training settings and main Python dependencies.
- `results/`: aggregate classification and MIC metrics and paired-bootstrap outputs. Classification comparisons are five-seed mean metrics; bootstraps in the JSON files instead evaluate averages of seed predictions.
- `analysis/`: series-analysis scripts.

## Reproduction status

This code snapshot alone is **not a complete one-command reproduction package**. Raw and processed input data, frozen split membership and intermediate feature matrices, out-of-fold predictions, and fitted weights are not included here. `spfuse.py` expects the model-handoff directory structure (`models/`, processed `data/`, `oof/`, and `reference_predictions/`), while some frozen scripts expect intermediate artifacts at the historical project-relative locations; obtain or regenerate those artifacts before running. Classification and MIC models use separate training and different endpoints. No public URL for this project's weights or derived data is asserted here.

The recorded primary environment was Python 3.10.20, RDKit 2022.03.3, NumPy 1.26.3, pandas 2.2.3, SciPy 1.15.3, scikit-learn 1.6.1, LightGBM 4.7.0 and joblib 1.6.0. Baseline methods and attribution calculations have additional environment requirements; see the frozen protocol files. Different hardware and libraries can change results.

## Source structure and interpretation

The classification features consist of 85 fragment counts (F), 6,311 fingerprint bits (P), and 123 non-fragment descriptors (D); SPFuse uses P and D. For classification, a logistic regression model is trained on out-of-fold branch log-odds. MIC models are trained separately for each species, and ridge regression combines branch predictions. Molecular-pair and series analysis evaluates changes in the difference between species-specific log2 MIC values. Published baseline values and locally retrained baselines use the experimental summaries described in the manuscript.

This snapshot does not assert that every included historical script can be run without adaptation. Upstream data and software remain subject to their original terms and licenses.
