# Customer Segmentation & LTV Forecast

## Project Summary
Developed customer segmentation and lifetime value prediction model using RFM metrics and machine learning to prioritize high-value customer retention strategies.

## Key Objectives

- Segment customers based on purchase frequency and value.
- Predict future revenue contribution (LTV) by segment.
- Visualize customer clusters and LTV forecasts in Tableau.

## Tools & Technologies

- Python (pandas, scikit-learn, matplotlib, seaborn)
- SQL (BigQuery)
- Tableau
- Jupyter / notebooks

## Approach

- Extracted historical transactions from BigQuery and cleaned using Python.
- Calculated RFM (Recency, Frequency, Monetary) scores and created segments.
- Built regression model to predict LTV.
- Designed Tableau dashboard for business insights.

## Impact

- Identified 20% high-value customers contributing to 65% of revenue.
- Supported marketing team in optimizing campaigns by segment.

## Links

- GitHub Repo: https://github.com/Saru2123/saru-reddy-Customer-Segmentation-LTV
- Tableau Public: (Add link when published)

---

## Usage / How to run

Quick instructions to run the project locally and reproduce results.

Prerequisites
- Python 3.9+ (recommend using a virtual environment or conda).
- Access to the historical transactions dataset (BigQuery) or a local CSV export.
- (Optional) Google Cloud service account JSON for BigQuery access.

1) Clone the repository

    git clone https://github.com/Saru2123/saru-reddy-Customer-Segmentation-LTV.git
    cd saru-reddy-Customer-Segmentation-LTV

2) Create environment and install dependencies

Using pip + venv:

    python -m venv .venv
    source .venv/bin/activate    # macOS / Linux
    .\.venv\Scripts\activate     # Windows
    pip install -r requirements.txt

Or using conda (environment.yml provided):

    conda env create -f environment.yml
    conda activate customer-segmentation-ltv

3) Configure credentials (if using BigQuery)

    export GOOGLE_APPLICATION_CREDENTIALS="/path/to/service-account.json"  # macOS / Linux
    set GOOGLE_APPLICATION_CREDENTIALS="C:\path\to\service-account.json"  # Windows

4) Run the pipeline (examples)

- Extract data from BigQuery (example script):

    python scripts/01_extract_bigquery.py --project my-gcp-project --dataset sales --table transactions --out data/transactions.csv

- Calculate RFM and create segments:

    python scripts/02_compute_rfm.py --input data/transactions.csv --output data/rfm_scores.csv

- Train LTV model:

    python scripts/03_train_ltv.py --input data/rfm_scores.csv --output models/ltv_model.pkl

5) Example code snippets
A short example to compute RFM with pandas:

import pandas as pd
from datetime import datetime

df = pd.read_csv('data/transactions.csv', parse_dates=['order_date'])
reference_date = df['order_date'].max() + pd.Timedelta(days=1)

rfm = df.groupby('customer_id').agg({
    'order_date': lambda x: (reference_date - x.max()).days,
    'order_id': 'nunique',
    'amount': 'sum'
}).rename(columns={'order_date': 'recency', 'order_id': 'frequency', 'amount': 'monetary'})

# Simple scoring (1-5) using quantiles
for col in ['recency','frequency','monetary']:
    if col == 'recency':
        rfm[col + '_score'] = pd.qcut(rfm[col], 5, labels=[5,4,3,2,1])
    else:
        rfm[col + '_score'] = pd.qcut(rfm[col], 5, labels=[1,2,3,4,5])

rfm['rfm_score'] = rfm['recency_score'].astype(int)*100 + rfm['frequency_score'].astype(int)*10 + rfm['monetary_score'].astype(int)
print(rfm.head())

A short example to train a regression model (scikit-learn):

from sklearn.ensemble import RandomForestRegressor
from sklearn.model_selection import train_test_split
from sklearn.metrics import mean_absolute_error
import joblib

X = rfm[['recency','frequency','monetary']]
y = rfm['ltv']  # assume ltv column available or computed

X_train,X_test,y_train,y_test = train_test_split(X,y,test_size=0.2,random_state=42)
model = RandomForestRegressor(n_estimators=100, random_state=42)
model.fit(X_train,y_train)

preds = model.predict(X_test)
print('MAE:', mean_absolute_error(y_test, preds))
joblib.dump(model, 'models/ltv_model.pkl')

6) Output and artifacts
- data/: processed datasets and intermediate CSVs
- models/: trained model files (pickle/joblib)
- notebooks/: exploratory notebooks (optional)
- tableau/: packaged Tableau workbook or export

---

Created by: Saru2123
Date: 2025-10-30
