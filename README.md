# Stockout Radar

ML-based demand forecasting and reorder recommendation system for small online retailers.

Machine Learning semester project, Sukkur IBA University, Fall 2026.

## Team

| Name | Student ID | Role |
|---|---|---|
| Muhammad Qasim (Lead) | 023-24-0279 | ML pipeline |
| Shahzaib Mahar | 023-24-0282 | Data preprocessing |
| Manahil Fatima | 023-24-0249 | Model development and GUI |

## Problem

Small online shops often restock from spreadsheets and past sales. This causes stockouts (lost sales) or overstock (cash tied up). Stockout Radar forecasts how many units of each product will sell in the **next 4 weeks**, then compares the forecast with the stock the user has to give a reorder recommendation.

The dataset has sales only, with no stock levels. So the system is a **demand forecaster plus a reorder rule**, where the user enters the current stock. It does not predict stockouts directly.

## Dataset

- **Online Retail II**, UCI Machine Learning Repository
- Licence: CC BY 4.0
- Source: https://archive.ics.uci.edu/dataset/502/online+retail
- 1,067,371 transactions from a UK online retailer, 1 Dec 2009 to 9 Dec 2011

The raw data is **not stored in this repository**. Download it and place `online_retail_II.xlsx` in `data/raw/` (see "How to run").

## Approach

| Step | What we did |
|---|---|
| Cleaning | Removed cancellations, quantity or price <= 0, and non-product codes (postage, fees, manual entries). 30,494 rows removed (about 2.9%). Dropped Customer ID. |
| Aggregation | One row per product per week (Monday to Sunday). Partial first and last weeks dropped. Zero-sales weeks added from each product's first sale. 4,675 products, 104 weeks, 407,702 product-weeks. |
| Target | Total units sold in weeks t to t+3 (next 4 weeks). |
| Features (16) | Sales lags (1, 2, 4 weeks), rolling mean and std (4, 8 weeks), order counts, price and price change, trend ratio, calendar (week of year, month, weeks to Christmas), product age. All use data up to week t-1 only. |
| Split | Chronological, with 3-week gaps because each target covers 4 weeks. Train 2010-02-01 to 2011-05-30, validation 2011-06-27 to 2011-08-08, test 2011-09-05 to 2011-11-07. |
| Baselines | Naive (4 x last week) and 4-week moving average. |
| Models | Linear Regression, Ridge, Random Forest, Gradient Boosting (squared loss, absolute-error loss, log target). Scaling is inside a scikit-learn `Pipeline`. |
| Tuning | `RandomizedSearchCV` with `TimeSeriesSplit`, on the training set only. |
| Metrics | MAE, RMSE, WAPE. |

## Results (test set, Sep to Nov 2011)

| Model | MAE | RMSE | WAPE |
|---|---|---|---|
| Naive (4 x lag1) | 84.93 | 332.80 | 0.672 |
| Moving average (4 x mean4) | 66.79 | 240.40 | 0.529 |
| Linear Regression | 65.78 | 238.31 | 0.521 |
| Ridge | 65.77 | 238.31 | 0.520 |
| Random Forest | 69.01 | 229.93 | 0.546 |
| Gradient Boosting, squared loss | 67.69 | 237.10 | 0.536 |
| **Gradient Boosting, absolute-error loss (tuned)** | **60.51** | 246.93 | **0.479** |

The tuned gradient boosting model has the best MAE and WAPE, about 9% lower MAE than the moving average. It has the highest RMSE of the main models, because absolute-error loss targets the typical week and under-predicts rare bulk-order spikes. The choice of loss function mattered much more than hyperparameter tuning.

## Repository structure

```
stockout-radar/
├── data/
│   ├── raw/                  (original dataset, not committed)
│   └── processed/            (generated files, not committed)
├── notebooks/
│   ├── 01_cleaning.ipynb
│   ├── 02_aggregation.ipynb
│   ├── 03_eda.ipynb
│   ├── 04_features.ipynb
│   ├── 05_split_baselines.ipynb
│   └── 06_models.ipynb
├── src/                      (shared helper functions)
├── models/                   (saved model, gb_model.joblib)
├── app/                      (Streamlit app)
├── reports/figures/          (plots used in the report)
├── requirements.txt
└── README.md
```

## How to run

1. Clone the repository and open a terminal in the project folder.

```bash
git clone https://github.com/ManahilFatima16/stockout-radar-.git
cd stockout-radar-
```

2. Create a virtual environment and install the libraries.

```powershell
# Windows (PowerShell)
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

```bash
# Mac / Linux
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

3. Download the dataset from the UCI link above and place `online_retail_II.xlsx` in `data/raw/`.

4. Run the notebooks in order (`01` to `06`) with the `.venv` kernel selected. Reading the Excel file takes 1 to 3 minutes the first time; the notebook saves a CSV copy for faster loading. All random seeds are fixed (`random_state=42`).

5. Run the app (after it is built):

```bash
streamlit run app/streamlit_app.py
```

## Leakage control

Every feature uses data up to the week before the prediction week. Rolling features use `shift(1)` before the window. Same-week units, orders and price are not used. The target (weeks t to t+3) is used only as the label. Scaling is fitted inside a `Pipeline` on training data only.

## Limitations

- The dataset has no stock levels, so the reorder rule cannot be validated against real stockouts.
- Only one Christmas season (2010) is in the training data, so seasonal peaks are hard to learn.
- Products with fewer than 8 weeks of history are not in the model table (cold start).
- The model under-predicts sudden bulk-order spikes. The largest errors come from a few products.
- Validation (Jun to Aug) and test (Sep to Nov) cover different seasons, so their error levels differ.
- Cross-validation folds have a small target overlap at the boundaries (up to 3 weeks).

## Project status

- [x] Cleaning, weekly aggregation, EDA
- [x] Feature engineering and leakage audit
- [x] Chronological split, baselines
- [x] Models, tuning, test evaluation
- [x] Error analysis (draft)
- [ ] Feature importance and residual plots
- [ ] Safety stock and reorder rule
- [ ] Streamlit app (link to be added here)
- [ ] Final report

## Acknowledgements and AI use

Dataset: Chen, D. (2012). Online Retail II. UCI Machine Learning Repository, CC BY 4.0.

An AI assistant (Claude) was used for guidance, debugging and explanation during development. The AI Use Disclosure in the report gives the details.