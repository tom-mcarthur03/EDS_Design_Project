# Data Description – N₂O Emissions Modelling Dataset

## Project Overview

This repository contains time series data from the Avedøre wastewater treatment plant (WWTP) in Copenhagen, Denmark for analysing operational dynamics and to model **N₂O emissions** produced during biological treatment processes. The data was collected using the plant's SCADA system and cloud-based control platform, and includes 12 sensor measurements and 12 control signal measurements for two bioreactor tanks (24 measurement signals in total). 

---

## Original Dataset

### Data Source

* **Institution:** BIOFOS
* **Location:** Avedøre, Copenhagen, Denmark
* **Repository:** Mendeley Data
* **Version:** 4
* **DOI:** 10.17632/xmbxhscgpr.1
* **Direct link:** https://data.mendeley.com/datasets/xmbxhscgpr/1

---

### Data Characteristics

| Attribute         | Description           |
| ----------------- | --------------------- |
| Data type         | Time series (CSV)     |
| Sampling interval | 2 minutes             |
| Dataset duration  | June 2022 – June 2024 |
| Number of signals | 24 variables          |

## Data Storage in This Repository

Processed datasets used for modelling are stored in the repository:

```text
data/
└── segment_data/
    ├── segment_2_clean.csv
    ├── segment_3_clean.csv
    ├── segment_4_clean.csv
    ├── seg2_86days.csv
    ├── seg3_53days.csv
    └── seg3_52days.csv
```

---

# Processed Dataset Summary

The table below summarises the datasets generated during preprocessing, including the applied data processing steps and time coverage.

| Dataset               | Source Segment | Preprocessing Applied                                                                   | Missing Data Status                                     | Time Coverage                 |
| --------------------- | -------------- | --------------------------------------------------------------------------------------- | ------------------------------------------------------- | ----------------------------- |
| `segment_2_clean.csv` | Segment 2      | Resampled to 10-min resolution; gaps >1 hour removed based on key operational variables | Smaller gaps (<1h) may remain; no interpolation applied | Sep 2022 – Mar 2023           |
| `segment_3_clean.csv` | Segment 3      | Resampled to 10-min resolution; gaps >1 hour removed based on key operational variables | Smaller gaps (<1h) may remain; no interpolation applied | Mar 2023 – Feb 2024           |
| `segment_4_clean.csv` | Segment 4      | Resampled to 10-min resolution; gaps >1 hour removed based on key operational variables | Smaller gaps (<1h) may remain; no interpolation applied | Feb 2024 – Jun 2024           |
| `seg2_86days.csv`     | Segment 2      | Extracted longest continuous valid block where all key variables are present            | No missing values; no interpolation applied                                     | *[Add exact start–end dates]* |
| `seg3_53days.csv`     | Segment 3      | Extracted continuous valid block where all key variables are present                    | No missing values; no interpolation applied                                       | *[Add exact start–end dates]* |
| `seg3_52days.csv`     | Segment 3      | Extracted continuous valid block where all key variables are present                    | No missing values; no interpolation applied                                 | *[Add exact start–end dates]* |

Notes:

* All datasets maintain a **10-minute temporal resolution**.
* Removal of missing data was based on the following key operational variables identified as the most relevant predictors of N2O emissions during feature selection:
```
t1_nh4
t1_no3
t1_airflow
t1_temp
t1_ss
t1_po4
inflow

```

* Rows were considered **invalid if any of these variables were missing**.
* **No interpolation was applied during preprocessing**, ensuring that the modelling datasets consist only of **observed measurements**. If needed, imputation will only be implemented for specific machine learning models. The objective of the data cleaning process was to preserve the original distribution, phases and thresholds from the raw dataset. 

---

## Preprocessing Workflow

The raw dataset underwent several preprocessing steps to ensure data quality and consistency for feature engineering and machine learning model development, while preserving the original distribution of the raw data to capture the process dynamics of a real, operational WWTP.

The workflow consisted of **data cleaning, resampling, and missing data filtering**.

---

### 1. Initial Quality Checks

Initial exploratory checks were conducted to verify the integrity and structure of the raw dataset.

The following steps were applied:

* Review dataset structure including **data types, dataframe dimensions, set datetime index, and descriptive statistics**
* Remove **duplicate rows with identical timestamps**
* Remove observations from **Tank 2**, as complete data was only available for **Tank 1**
* Standardise **variable names using consistent naming conventions**

---

### 2. Handling Negative Values

Negative values were identified in several sensor measurements.
These values were transformed using a linear shift or interpolation before resampling since they provide no meaningful physical information on N2O production or the process phases. 

---

# 3. Timestamp Alignment Analysis

The raw dataset was recorded at an **approximate 2-minute sampling interval**, however, observations were irregularly logged at even and odd timestamps across variables. 

The following analyses were conducted on the frequency of measurement logging:

* Analysis of **timestamp distribution** to evaluate sensor logging intervals

After multiple experiments with aligning the timestamps to a 2 minute frequency, we found that the irregular logging did not significantly alter the statistical distribution of the resampled data, especially since it is resampled to 10 minutes before feature engineering and model construction. 

Therefore, timestamp alignment was **excluded from the final preprocessing pipeline** to maintain a **simpler and computationally efficient workflow**, which is desirable for real-time or operational implementations of machine learning models.

---

# 4. Resampling to 10-Minute Resolution

The cleaned dataset was resampled from the original 2-minute frequency to a 10-minute sampling interval.

Additional preprocessing before resampling included removal of **extreme outliers in N₂O measurements**. We decided to preserve the rest of the peaks in order to ensure that our models could capture real peaks in key variables such as N2O emissions. 

Resampling was implemented using **custom aggregation functions** for different variable types:

* **Numerical variables:** aggregated using the mean.
* **Categorical / control signals:** aggregated using appropriate categorical aggregation rules (mode or maximum).

After resampling, we analysed the distribution of the raw dataset with the resampled one. This confirmed that the process phases and feature dynamics are preserved after resampling, with slight smoothing effects for t1_airflow and t1_do. We also found that the distribution of control  (phasecode, t1_phase) and watchdog (storm mode) signals was maintained after resampling. 

---

# 5. Gap Detection and Segment Creation

Following resampling, missing data was analysed using a set of features identified during feature selection, based on their ability to predict N2O emissions. 

A row was considered **invalid if any of these variables were missing**.

Consecutive missing rows were grouped into **gap blocks**, and the dataset was divided into operational segments.

Segment boundaries were defined when **gaps exceeded 24 hours**

This resulted in several continuous operational segments, of which **Segments 2, 3, and 4** were retained for analysis (segment 1 was missing inflow data). 

---

# 6. Gap Filtering

Within each segment, shorter outages were removed to improve data continuity.

Threshold applied: **gaps longer than 1 hour were removed** based on the set of features used for modelling. 

No interpolation was applied at this stage to ensure that modelling datasets contain **only observed measurements**. This is in alignment with our project objectives to design models that are interpretable and capture the process dynamics of real operational WWTPs. 

---

# 7. Extraction of Continuous Modelling Windows

From the cleaned segments, the **longest continuous periods with no missing values in the key variables** were extracted.

The selected modelling windows include:

* `seg2_86days.csv`
* `seg3_53days.csv`
* `seg3_52days.csv`

Each dataset represents a **fully continuous time-series window suitable for machine learning models that require temporal continuity**.
