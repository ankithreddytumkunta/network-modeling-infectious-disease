# Network Modeling for Infectious Disease Spread

## Project Overview

This project focuses on modeling the spread of infectious diseases using county-level public health, demographic, economic, temporal, and spatial data. The goal is to understand infection activity patterns across locations and over time, and to evaluate how predictive modeling can support early drug demand planning and manufacturing preparedness.

The project follows a complete graduate-level data science workflow, including data collection, data cleaning, preprocessing, exploratory data analysis, feature engineering, model development, model evaluation, and interpretation.

## Problem Statement

Infectious disease outbreaks can create sudden increases in medicine demand. The COVID-19 pandemic showed that drug manufacturing and supply planning systems need stronger early-warning signals to respond to infectious disease activity. By modeling county-level disease trends, this project aims to support earlier identification of high-risk regions and improve planning for medicine production and distribution.

## Data Sources

The project uses three public data sources:

| Source      | Description                           |
| ----------- | ------------------------------------- |
| CDC-NNDSS   | Weekly infectious disease records     |
| U.S. Census | County-level population estimates     |
| BEA         | County-level income and economic data |

## Dataset Summary

| Stage                                |      Rows |
| ------------------------------------ | --------: |
| CDC raw dataset                      | 1,840,440 |
| Merged CDC, Census, and BEA dataset  | 1,079,478 |
| Final cleaned dataset                |   725,787 |
| Non-zero infection dataset           |   333,028 |
| High-burden disease modeling dataset |   174,042 |

The final modeling dataset focuses on high-burden infectious disease records to improve model learning and reduce noise from low-frequency disease categories.

## Project Workflow

| Notebook | Purpose                                     |
| -------- | ------------------------------------------- |
| Week 2   | Data collection, cleaning, and merging      |
| Week 3   | Data preprocessing and disease grouping     |
| Week 4   | Gaussian Bayesian / Bayesian Ridge modeling |
| Week 5   | Temporal Convolutional Network modeling     |
| Week 6   | Hybrid TCN-Spatial-GBN modeling             |

## Feature Engineering

The following feature engineering steps were applied:

* Encoded disease, state, and county variables
* Created lag features from previous infection activity
* Added cyclical week features using `week_sin` and `week_cos`
* Scaled numerical features such as population and income
* Created spatial lag features for nearby county influence
* Applied log transformation to reduce target skewness

## Target Variable

The target variable used for modeling was the log-transformed previous 52-week maximum infection activity:

```text
log(previous_52_week_max)
```

The log transformation was used because infectious disease activity was highly skewed, with many low values and a few large outbreak spikes.

## Models Used

### 1. Gaussian Bayesian Model / Bayesian Ridge Regression

This model was used to identify important disease, location, demographic, and temporal factors. It provides interpretability through coefficient-based feature importance.

### 2. Temporal Convolutional Network (TCN)

The TCN model was used to learn temporal disease patterns from weekly disease sequences. It captured how past infection activity influences future disease activity.

### 3. Hybrid TCN-Spatial-GBN Model

The hybrid model combined temporal sequence learning, spatial lag features, and Bayesian-style uncertainty modeling. It was designed to capture time-based, location-based, and factor-based relationships together.

## Model Performance

| Model                              |    MAE |   RMSE |     R² |
| ---------------------------------- | -----: | -----: | -----: |
| Gaussian Bayesian / Bayesian Ridge |  0.587 |  0.760 |  0.708 |
| Temporal Convolutional Network     | 0.5479 | 0.7136 | 0.7399 |
| Hybrid TCN-Spatial-GBN             | 0.6122 | 0.8301 | 0.5910 |

## Key Findings

* Disease type was one of the strongest predictors of infection activity.
* Location-based features such as state and county influenced disease patterns.
* Lag features showed that past infection activity improved future prediction.
* The TCN model achieved the best overall performance.
* Temporal patterns were highly important for infectious disease forecasting.
* Hybrid modeling showed value, but it requires additional tuning to outperform the temporal model.

## Manufacturing Relevance

The results of this project can support drug manufacturing and supply planning by:

* Estimating future medicine demand earlier
* Identifying high-risk disease groups
* Supporting county-level and regional supply planning
* Improving production readiness using weekly disease trends
* Reducing the risk of unexpected medicine supply shortages

## Repository Structure

```text
Network_Modeling_Infectious_Disease/
│
├── README.md
├── requirements.txt
│
├── notebooks/
│   ├── Week_2_Data_Collection_and_Merging.ipynb
│   ├── Week_3_Data_Preprocessing.ipynb
│   ├── Week_4_Causal_Network_Modeling.ipynb
│   ├── Week_5_Temporal_Modeling_TCN.ipynb
│   └── Week_6_Hybrid_Model.ipynb
│
├── data/
│   ├── Datasets.zip
│   └── df_high_burden.csv
│
├── report/
│   └── NETWORK_MODELING_FOR_INFECTIOUS_DISEASE_SPREAD.pdf
│
└── presentation/
    └── NETWORK_MODELING_FOR_INFECTIOUS_DISEASE.pptx
```

## Technologies Used

* Python
* Pandas
* NumPy
* Matplotlib
* Seaborn
* Scikit-learn
* TensorFlow / Keras
* keras-tcn
* Requests
* Geopy
* Shapely
* TQDM

## How to Run the Project

1. Clone the repository:

```bash
git clone https://github.com/ankithreddytumkunta/network-modeling-infectious-disease.git
```

2. Navigate into the project folder:

```bash
cd network-modeling-infectious-disease
```

3. Install required libraries:

```bash
pip install -r requirements.txt
```

4. Run the notebooks in order:

```text
Week 2 → Week 3 → Week 4 → Week 5 → Week 6
```

## Important Note

The BEA API key is not included in this repository for security reasons. To run the BEA data collection section, users should create their own BEA API key and store it securely.

Example:

```python
import os
BEA_API_KEY = os.getenv("BEA_API_KEY")
```

## Conclusion

This project demonstrates how network-based and temporal machine learning models can be used to study infectious disease spread. Among the models tested, the Temporal Convolutional Network achieved the best performance because it effectively learned weekly disease patterns. The findings suggest that temporal disease modeling can support early drug demand planning and improve manufacturing preparedness.

## Author

**Ankith Reddy Tumkunta**
Master’s Student, Data Science
Regis University
