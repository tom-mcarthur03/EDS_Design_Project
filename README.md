# EDS_DESIGN_PROJECT

## Project overview

This repository contains the code, notebooks, and processed data for an MSc design project on modelling nitrous oxide ($N_2O$) emissions in a wastewater treatment plant using operational sensor data. The workflow is organised to follow the structure of the accompanying report as closely as possible, moving from preprocessing and exploratory data analysis through feature selection, feature engineering, predictive modelling, classification of high-$N_2O$ events, and LSTM-based forecasting.

The repository is intended to function as a structured computational companion to the written report. For that reason, the notebooks are best run in section order.

## Repository structure

```text
EDS_DESIGN_PROJECT/
├── data/
│   ├── EDA/
│   │   ├── desktop.ini
│   │   └── df_10T.csv
│
│   ├── lstm_data/
│   │   ├── seg4_baseline_clean.csv
│   │   ├── seg4_extended_clean.csv
│   │
│   │   ├── absolute_n2o/
│   │   │   ├── final_results_absolute.csv
│   │   │   ├── r2_pivot_absolute.csv
│   │   │   └── rmse_pivot_absolute.csv
│   │
│   │   ├── delta_n2o/
│   │   │   ├── final_results_delta.csv
│   │   │   ├── r2_pivot_delta.csv
│   │   │   └── rmse_pivot_delta.csv
│   │
│   │   ├── figures/
│   │   │   └── peak_metrics_absolute.csv
│   │
│   │   └── results_all/
│   │       ├── hyperparameter_summary.csv
│   │       ├── main_results_table.csv
│   │       ├── r2_pivot_all.csv
│   │       └── rmse_pivot_all.csv
│
│   └── segment_data/
│       ├── seg4_df_7_features_baseline.xls
│       ├── seg4_df_8_features_baseline.xls
│       ├── seg4_df_features_10lags_ready.xls
│       ├── seg4_df_features_10lags_readynoN20.xls
│       ├── segment_2_clean.csv
│       ├── segment_3_clean.csv
│       └── segment_4_clean.csv
│
├── docs/
│   └── colormap_imperial.txt
│
├── notebooks/
│   ├── Section 2 - EDA, Preprocessing, Feature Selection/
│   │   ├── Data Preprocessing.ipynb
│   │   ├── Data Quality Analysis.ipynb
│   │   ├── Feature Selection.ipynb
│   │   ├── Global EDA.ipynb
│   │   └── Segment Based EDA.ipynb
│
│   ├── Section 3 - Feature Engineering/
│   │   ├── Seg2_10lag.ipynb
│   │   ├── seg2_30lag.ipynb
│   │   └── Seg4_10lag.ipynb
│
│   ├── Section 4 - Predictive Modelling/
│   │   ├── LSTM N2O Forecasting Model.ipynb
│   │   ├── LSTM Feature Engineering.ipynb
│   │   └── Model development.ipynb
│
│   ├── Section 5 - Classification/
│   │   ├── Classification with N2O.ipynb
│   │   └── Classification without N2O.ipynb
│
│   └── .gitkeep
│
├── .gitignore
├── README.md
└── requirements.txt
```

## Data flow

The data and notebook workflow follows a staged pipeline.

`data/EDA/aved_raw.csv` is the raw dataset used in the early preprocessing and exploratory analysis notebooks, especially the preprocessing workflow and the global EDA work. This is the starting point for the repository.

`data/EDA/df_10T.csv` is the preprocessed 10-minute dataset produced after the main preprocessing stage. It is then used in the feature selection work and as the basis for creating the cleaned segment datasets.

The cleaned segment files in `data/segment_data/` — `segment_2_clean.csv`, `segment_3_clean.csv`, and `segment_4_clean.csv` — are then used in the feature engineering notebooks. These segment datasets also feed into the general predictive modelling workflow and the LSTM feature engineering notebook.

The Section 3 feature engineering work exports the derived Segment 4 files used later in classification, including `seg4_df_7_features_baseline.xls`, `seg4_df_8_features_baseline.xls`, `seg4_df_features_10lags_ready.xls`, and `seg4_df_features_10lags_readynoN20.xls`. Although these files have `.xls` extensions, they are imported in the classification workflow as CSV-compatible files.

For the LSTM workflow, the cleaned segment datasets are first processed in `LSTM Feature Engineering.ipynb`, which generates the model-ready inputs used in `LSTM N2O Forecasting Model.ipynb`. The `data/lstm_data/` directory contains the prepared datasets (`seg4_baseline_clean.csv` and `seg4_extended_clean.csv`) alongside structured subfolders for model outputs. These include `absolute_n2o/` and `delta_n2o/` for task-specific results, `results_all/` for aggregated performance summaries, and `figures/` for derived evaluation metrics. Together, these folders store the key outputs of the LSTM experiments, including performance tables (R², RMSE), final results, and hyperparameter summaries.

## Notebook execution order

The recommended execution path follows the report structure and the data dependencies between notebooks.

### Section 2 - EDA, Preprocessing, Feature Selection

Run the notebooks in the following order:

1. `Global EDA.ipynb`
2. `Data Preprocessing.ipynb`
3. `Segment Based EDA.ipynb`
4. `Data Quality Analysis.ipynb`
5. `Feature Selection.ipynb`

In practical terms, `aved_raw.csv` is used in the early EDA and preprocessing stages, while `df_10T.csv` is the preprocessed dataset used for feature selection and for creating the cleaned segment datasets (`segment_X_clean.csv`).

### Section 3 - Feature Engineering

After Section 2, continue with:

1. `Seg2_10lag.ipynb`
2. `seg2_30lag.ipynb`
3. `Seg4_10lag.ipynb`

These notebooks use the cleaned segment datasets and generate the engineered feature files used later in classification.

### Section 4 - Predictive Modelling

Next, run:

1. `Model development.ipynb`
2. `LSTM Feature Engineering.ipynb`
3. `LSTM N2O Forecasting Model.ipynb`

`Model development.ipynb` uses the cleaned segment datasets directly. The LSTM workflow is sequential: `LSTM Feature Engineering.ipynb` prepares the LSTM input datasets, and `LSTM N2O Forecasting Model.ipynb` uses those exported datasets for model training and evaluation.

### Section 5 - Classification

Finally, run:

1. `Classification with N2O.ipynb`
2. `Classification without N2O.ipynb`

These notebooks use the exported Segment 4 feature files from the feature engineering stage, in particular the 10-lag datasets with and without N₂O-derived inputs.

## Relation to the report

The notebook structure is intended to mirror the workflow of the final report.

- **Section 2** covers exploratory data analysis, preprocessing, data quality analysis, and feature selection.
- **Section 3** covers feature engineering, including lag construction and segment-specific feature preparation.
- **Section 4** covers predictive modelling, including both the general modelling workflow and the LSTM-specific workflow.
- **Section 5** covers classification of high-$N_2O$ events with and without $N_2O$-derived predictors.

This layout is designed so that the repository can be read alongside the report, with the main computational steps appearing in the same broad order as the written methodology and results.

## Requirements and environment

Project dependencies are listed in `requirements.txt`. `colormap_imperial.txt` is also used throughout to align figure style, though it is generally defined in each notebook individually.

## Data

The repository contains the raw dataset used for preprocessing and EDA, the 10-minute preprocessed dataset used for feature selection, cleaned segment datasets used for downstream modelling, and exported intermediate datasets used for classification and LSTM forecasting.