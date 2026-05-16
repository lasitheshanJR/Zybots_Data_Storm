Latent Outlet Potential Estimation Framework
Overview

This project was developed for the OCTAVE – John Keells Group data science challenge.
The objective is to estimate the Maximum Monthly Purchase Potential (latent demand) for traditional trade beverage outlets in Sri Lanka.

The framework combines:

Data Engineering (Bronze → Silver → Gold pipeline)
Data Cleaning & Forensics
Feature Engineering
Latent Demand Estimation
Machine Learning-based Potential Prediction
Problem Statement

Historical sales do not represent true outlet demand because observed sales may be constrained by:

stockouts
credit limitations
delivery capacity
operational inefficiencies

Observed sales are treated as:

Y
observed
	​

=min(Y
potential
	​

,C)

Where:

Y
potential
	​

 = true latent demand
C = operational/system constraints

The goal is to estimate uncapped monthly purchase potential for each outlet.

Project Structure
project_root/
│
├── data/
│   ├── bronze/
│   │   ├── transactions_history_final.csv
│   │   ├── outlet_master.csv
│   │   ├── outlet_location.csv
│   │   ├── distributor_seasonality_details.csv
│   │   └── holiday_list.csv
│   │
│   ├── silver/
│   │   ├── cleaned_transactions.csv
│   │   ├── cleaned_holidays.csv
│   │   └── rejected_records/
│   │
│   └── gold/
│       ├── outlet_month_features.csv
│       └── final_predictions.csv
│
├── notebooks/
│   ├── 01_data_cleaning.ipynb
│   ├── 02_feature_engineering.ipynb
│   ├── 03_modeling.ipynb
│   └── 04_evaluation.ipynb
│
├── reports/
│   └── final_report.pdf
│
├── requirements.txt
└── README.md
Pipeline Architecture
Bronze Layer

Raw datasets are ingested without modification.

Input Datasets
transactions_history_final.csv
outlet_master.csv
outlet_location.csv
distributor_seasonality_details.csv
holiday_list.csv
Silver Layer

Data cleaning and validation layer.

Implemented Data Quality Checks
Duplicate detection
Null validation
Numeric range validation
Latitude/longitude validation
Data type standardization
Category normalization
Examples of Detected Issues
Duplicate transaction rows
Negative sales quantities
Invalid geo-coordinates
Inconsistent outlet categories
Missing values

Invalid records were quarantined into:

silver/rejected_records/
Gold Layer

Feature-engineered analytical dataset for modeling.

Engineered Features
Monthly outlet sales
SKU diversity
Cooler count
Holiday counts
Seasonality index
Average distributor sales
Outlet operational indicators
Feature Engineering
Sales Features
Total monthly liters
Average sales
Maximum historical sales
Sales variability
Product Features
Unique SKU count
Product diversity
Operational Features
Cooler count
Outlet type
Outlet size
Seasonal Features
Seasonality index
Holiday count
Poya day count
Modeling Approach
Latent Demand Logic

Observed sales are treated as censored demand.

Potential demand was estimated using:

Historical peak behavior
Peer outlet patterns
Engineered operational signals
Machine Learning Model
Algorithms Tested
Random Forest Regressor
LightGBM Regressor
Final Prediction Target
Maximum_Monthly_Liters
Evaluation Strategy

Since no true ground-truth target exists, evaluation was performed using:

Proxy consistency metrics
Relative regression performance
Business-rule validation
Prediction stability checks
Metrics Used
R² Score
Mean Absolute Error (MAE)
Business Validation Checks
Predicted potential ≥ observed sales
Higher cooler counts correspond to higher potential
Similar outlets show stable predictions
POI Enrichment Strategy

External geospatial signals were considered using:

OpenStreetMap
Overpass API

Targeted POIs:

Schools
Hospitals
Bus stands
Markets
Tourist locations

These were treated as catchment-demand proxies.

How to Run
1. Install Dependencies
pip install -r requirements.txt
2. Run Data Cleaning
python notebooks/01_data_cleaning.ipynb
3. Run Feature Engineering
python notebooks/02_feature_engineering.ipynb
4. Run Modeling Pipeline
python notebooks/03_modeling.ipynb
5. Generate Final Predictions
python notebooks/04_evaluation.ipynb
Final Output

Generated submission file:

final_predictions.csv

Format:

Outlet_ID	Maximum_Monthly_Liters
Generative AI Usage

LLMs including ChatGPT were used for:

brainstorming modeling strategies
debugging pipeline issues
generating boilerplate code
improving feature engineering logic
documentation refinement

All generated outputs were manually validated and refined before use.

Technologies Used
Python
Pandas
NumPy
Scikit-learn
LightGBM
Matplotlib
OpenStreetMap APIs
