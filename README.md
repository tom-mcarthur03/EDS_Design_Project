# EDS_DESIGN_PROJECT

## Project overview

This repository contains the code and notebooks for an MSc design project on modelling nitrous oxide ($N_2O$) emissions in a wastewater treatment plant using operational sensor data. The workflow follows the structure of the accompanying report and covers preprocessing, exploratory data analysis, feature selection, feature engineering, predictive modelling, classification of high-$N_2O$ events, and LSTM-based mid-horizon modelling.

The repository is organised to mirror the report workflow as closely as possible, so the notebooks should be run in section order.

## Repository structure

```text
EDS_DESIGN_PROJECT/
├── data/
├── docs/
│   ├── colormap_imperial.txt
│   └── data_description.md
├── notebooks/
│   ├── Section 2 - EDA, Preprocessing, Feature Selection/
│   ├── Section 3 - Feature Engineering/
│   ├── Section 4 - Predictive Modelling/
│   ├── Section 5 - Classification/
│   ├── .gitkeep
├── .gitignore
└── requirements.txt
```

## Notebook execution order

The main recommended execution path follows the report section order.

### Section 2 - EDA, Preprocessing, Feature Selection

Run the notebooks in the following order:

1. `Global EDA.ipynb`
2. `Data Preprocessing.ipynb`
3. `Local EDA.ipynb`
4. `Data Quality Analysis.ipynb`
5. `Feature Selection.ipynb`

This section establishes the exploratory and preprocessing workflow that supports the later modelling stages.

### Section 3 - Feature Engineering

After Section 2, continue with the notebooks in `notebooks/Section 3 - Feature Engineering/`.

This section contains feature engineering work for different segments and lag configurations:

- `NEWSeg4_10lag.ipynb`
- `Seg2_10lag.ipynb`
- `seg2_30lag.ipynb`
- `Seg4_10lag.ipynb`

### Section 4 - Predictive Modelling

Next, continue with the notebooks in `notebooks/Section 4 - Predictive Modelling/`.

This section contains the continuous predictive modelling workflow together with LSTM-related work:

- `Model development.ipynb`
- `lstm_feature_engineering.ipynb`

### Section 5 - Classification

Finally, run the notebooks in `notebooks/Section 5 - Classification/`.

This section contains classification workflows for high-N2O event detection:

- `classification_seg4_N2O.ipynb` for classification with N2O included as an input
- `classification_seg4_no_N2O.ipynb` for classification without N2O as an input

## Relation to the report

The notebook structure is intended to follow the order of the final report:

- **Section 2**: EDA, preprocessing, data quality analysis, and feature selection
- **Section 3**: feature engineering
- **Section 4**: predictive modelling, including continuous modelling and LSTM-related work
- **Section 5**: classification of high-N2O events

This repository is therefore designed to function as a structured computational companion to the written project report.

## Requirements and environment

Project dependencies are listed in `requirements.txt`.

Additional project-specific reference material is provided in `docs/`, including:

- `data_description.md`
- `colormap_imperial.txt`

## Data

*This section will be updated after the data reorganisation is finalised*