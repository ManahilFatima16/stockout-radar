# Stockout Radar 📦

### ML-Based Demand Forecasting and Inventory Replenishment Recommendation System

Stockout Radar is a machine learning-based demand forecasting and inventory replenishment decision-support system designed for small online retailers. It analyzes historical sales data to forecast future product demand and helps retailers make informed inventory replenishment decisions.

**Academic Project | Sukkur IBA University | Fall 2026**

---

## 🚀 Live Demo

Stockout Radar is available as a deployed full-stack application.

| Resource | Link |
|---|---|
| **Live Application** | [https://stockout-radar.vercel.app](https://stockout-radar.vercel.app) |
| **Backend API** | [https://stockout-radar-api.vercel.app](https://stockout-radar-api.vercel.app) |
| **Full-Stack Source Code** | [smaharx/stockout-radar](https://github.com/smaharx/stockout-radar) |
| **Original ML Repository** | [ManahilFatima16/stockout-radar](https://github.com/ManahilFatima16/stockout-radar) |

The production application includes demand forecasting, inventory tracking, and replenishment recommendations through a web-based dashboard.

**Note:** The production application uses authentication for authorized access. The full-stack implementation and deployment are maintained in a collaborator's repository. This repository contains the original machine learning implementation and research notebooks.

---

## 📌 Project Overview

Small retailers often depend on spreadsheets and historical sales records to manage their inventory. Without proper demand forecasting, they may experience:

- **Stockouts:** Products become unavailable when customers want to purchase them.
- **Overstocking:** Excess inventory increases storage costs and ties up business capital.
- **Poor Replenishment Decisions:** Retailers struggle to determine when and how much stock to reorder.

Stockout Radar addresses these challenges by forecasting product demand for the next four weeks and providing inventory replenishment recommendations based on the forecast and current stock levels.

The system uses historical transaction data to identify demand patterns, engineer meaningful features, and train machine learning models.

**Important:** The original dataset contains sales information but does not include historical inventory levels or stockout labels. Therefore, Stockout Radar forecasts product demand and applies a replenishment rule using user-provided stock information. It does not directly predict or validate actual stockout events.

---

## ✨ Key Features

- Historical sales data preprocessing and cleaning.
- Weekly product-level sales aggregation.
- Exploratory Data Analysis (EDA).
- Feature engineering using historical sales patterns.
- Four-week product demand forecasting.
- Machine learning model training and evaluation.
- Model comparison using MAE, RMSE, and WAPE.
- Inventory tracking and stock management in the deployed application.
- Inventory replenishment recommendations.
- Web-based dashboard for viewing forecasts and inventory information.
- Full-stack application deployment.

---

## 📊 Dataset

The project uses the **Online Retail II Dataset** from the UCI Machine Learning Repository.

| Dataset Information | Details |
|---|---|
| Dataset | Online Retail II |
| Source | UCI Machine Learning Repository |
| Total Transactions | 1,067,371 |
| Transaction Period | December 2009 – December 2011 |
| License | CC BY 4.0 |
| File Format | Excel (.xlsx) |

**Dataset Source:** [Online Retail II – UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/502/online+retail)

The raw dataset is not included in this repository. Users must download it from UCI and place `online_retail_II.xlsx` inside `data/raw/`.

---

## ⚙️ Machine Learning Workflow

The project follows a structured machine learning pipeline.

### 1. Data Cleaning

- Removed cancelled transactions.
- Removed records with non-positive quantities and prices.
- Removed non-product entries such as postage and service charges.
- Removed unnecessary columns, including Customer ID.
- Prepared the cleaned dataset for further analysis.

### 2. Data Aggregation

- Converted transaction-level records into weekly sales data.
- Aggregated sales by product and week.
- Added zero-sales weeks for products where no sales occurred.
- Prepared a product-by-week dataset for demand forecasting.

### 3. Exploratory Data Analysis (EDA)

- Analyzed product sales patterns.
- Studied weekly and seasonal demand trends.
- Examined product-level demand distributions.
- Identified sales patterns and unusual observations.
- Created visualizations to understand the dataset.

### 4. Feature Engineering

Created 16 features using historical sales information, including:

- Previous sales (lag features).
- Rolling mean and standard deviation.
- Order counts.
- Historical prices and price changes.
- Sales trend ratio.
- Week of year and month.
- Weeks remaining until Christmas.
- Product age.

All features use information available before the prediction week to prevent data leakage.

### 5. Target Variable

The model predicts the total number of units sold over the next four weeks for each eligible product.

### 6. Train, Validation and Test Split

A chronological split was used to preserve the time-based nature of the dataset.

| Dataset | Period |
|---|---|
| Training | February 2010 – May 2011 |
| Validation | June 2011 – August 2011 |
| Testing | September 2011 – November 2011 |

Three-week gaps were used between splits because the prediction target covers four weeks.

### 7. Model Development

The following machine learning models and baselines were evaluated:

- Naive Forecasting.
- Four-Week Moving Average.
- Linear Regression.
- Ridge Regression.
- Random Forest Regressor.
- Gradient Boosting Regressor.

Hyperparameter tuning was performed using `RandomizedSearchCV` and `TimeSeriesSplit` on training data.

### 8. Model Evaluation

The models were evaluated using:

| Metric | Purpose |
|---|---|
| MAE | Measures average absolute prediction error. |
| RMSE | Penalizes larger prediction errors more heavily. |
| WAPE | Measures overall forecasting error relative to actual demand. |

---

## 📈 Model Evaluation Results

The following results are from the original machine learning notebook evaluation on the historical test dataset.

| Model | MAE | RMSE | WAPE |
|---|---:|---:|---:|
| Naive (4 × lag1) | 84.93 | 332.80 | 0.672 |
| Moving Average (4 × mean4) | 66.79 | 240.40 | 0.529 |
| Linear Regression | 65.78 | 238.31 | 0.521 |
| Ridge Regression | 65.77 | 238.31 | 0.520 |
| Random Forest | 69.01 | 229.93 | 0.546 |
| Gradient Boosting (Squared Loss) | 67.69 | 237.10 | 0.536 |
| **Gradient Boosting (Absolute-Error Loss, Tuned)** | **60.51** | **246.93** | **0.479** |

The tuned Gradient Boosting model achieved the lowest MAE and WAPE among the evaluated models.

It reduced MAE by approximately 9% compared with the four-week moving average baseline.

Although it achieved better MAE and WAPE, its RMSE was higher than some alternative models because large bulk-order demand spikes remained difficult to predict.

*Note: These are historical notebook evaluation results, not a guarantee of current production forecasting accuracy. The separately maintained full-stack deployment may use a different model artifact and evaluation version.*

---

## 🏗️ System Workflow

The overall system connects machine learning-based forecasting with inventory replenishment decision support.

```text
       Historical Sales Data
                |
                v
          Data Cleaning
                |
                v
       Weekly Aggregation
                |
                v
       Exploratory Analysis
                |
                v
       Feature Engineering
                |
                v
        Model Training
                |
                v
       Demand Forecasting
                |
                v
       Inventory Management
                |
                v
      Replenishment Assessment
                |
                v
      Inventory Recommendations
                |
                v
         Web Dashboard
```

The machine learning pipeline generates demand forecasts, while the deployed full-stack application connects forecasts with operator-provided inventory information to produce replenishment recommendations.

---

## 🛠️ Technology Stack

### Machine Learning and Data Processing

| Technology | Purpose |
|---|---|
| Python | Main programming language |
| Pandas | Data manipulation |
| NumPy | Numerical computations |
| Scikit-learn | Machine learning models |
| Joblib | Model persistence |
| Jupyter Notebook | Data analysis and experimentation |
| Matplotlib / Seaborn | Data visualization |

### Full-Stack Production Application

The separately maintained production application uses:

| Technology | Purpose |
|---|---|
| Next.js | Frontend framework |
| React | User interface |
| TypeScript | Frontend development |
| FastAPI | Backend REST API |
| PostgreSQL (Neon) | Database |
| Python | Backend and ML integration |
| Vercel | Application deployment |

See the [full-stack repository](https://github.com/smaharx/stockout-radar) for its implementation details.

---

## 📁 Repository Structure

The original machine learning repository is organized as follows:

```text
stockout-radar/
│
├── data/
│   ├── raw/                     # Original dataset (not committed)
│   └── processed/               # Processed datasets (not committed)
│
├── notebooks/
│   ├── 01_cleaning.ipynb
│   ├── 02_aggregation.ipynb
│   ├── 03_eda.ipynb
│   ├── 04_features.ipynb
│   ├── 05_split_baselines.ipynb
│   └── 06_models.ipynb
│
├── src/                         # Shared helper functions
│
├── models/                      # Saved machine learning models
│
├── app/                         # Original Streamlit application
│
├── reports/
│   └── figures/                 # Evaluation plots and visualizations
│
├── requirements.txt             # Python dependencies
│
└── README.md
```

---

## 💻 How to Run the Machine Learning Project

Follow these steps to set up the original machine learning repository locally.

### Prerequisites

- Python 3.10 or a compatible Python version.
- Git.
- Jupyter Notebook or VS Code.
- Required Python libraries.
- Online Retail II dataset.

### 1. Clone the Repository

```bash
git clone https://github.com/ManahilFatima16/stockout-radar.git

cd stockout-radar
```

### 2. Create a Virtual Environment

**Windows (PowerShell):**

```powershell
python -m venv .venv

.venv\Scripts\Activate.ps1
```

**Mac / Linux:**

```bash
python -m venv .venv

source .venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Download the Dataset

Download the Online Retail II dataset from:

https://archive.ics.uci.edu/dataset/502/online+retail

Place the downloaded Excel file inside:

```text
data/raw/online_retail_II.xlsx
```

### 5. Run the Jupyter Notebooks

Open the project in Jupyter Notebook or VS Code.

Execute the notebooks in the following order:

```text
01_cleaning.ipynb
02_aggregation.ipynb
03_eda.ipynb
04_features.ipynb
05_split_baselines.ipynb
06_models.ipynb
```

Select the virtual environment kernel before executing the notebooks.

The first execution may take longer because the raw Excel dataset needs to be processed. Processed CSV files are saved for faster subsequent loading.

---

## 🔒 Data Leakage Prevention

Preventing data leakage was an important part of the machine learning workflow.

The following practices were followed:

- Features use historical information available before the prediction week.
- Rolling features use `shift(1)` before calculating rolling statistics.
- The target variable is used only as the training label.
- Chronological splitting preserves the time-based structure.
- Feature scaling is performed inside a Scikit-learn Pipeline.
- Hyperparameter tuning is performed using training data only.
- Random seeds are fixed for reproducibility.

---

## ⚠️ Project Limitations

- The original dataset does not contain historical stock levels or actual stockout records.
- The replenishment recommendations depend on user-provided inventory information.
- Only one Christmas season is available in the training data.
- Products with fewer than eight weeks of historical data are excluded from the model table.
- Sudden bulk-order spikes can result in larger forecasting errors.
- The model's predictions are based on historical patterns and may not fully capture future market changes.
- The current replenishment rule does not account for every real-world supply chain factor, such as open purchase orders, supplier minimum order quantities, and multiple warehouse locations.

---

## 👥 Team Members

**Machine Learning Semester Project – Sukkur IBA University**

| Team Member | Contribution |
|---|---|
| Muhammad Qasim | Project Lead and ML Pipeline |
| Shahzaib Mahar | Data Preprocessing |
| Manahil Fatima | Model Development and Original Application Work |

The full-stack application was further developed and deployed in collaboration with the project team. Its maintained source code is available in the collaborator's repository.

---

## 🤝 Collaboration and Related Repository

This repository contains the original machine learning project, including its research workflow, notebooks, and model development work.

The full-stack implementation is maintained separately in the following collaborator repository:

**[smaharx/stockout-radar](https://github.com/smaharx/stockout-radar)**

The collaborator's repository extends the original project with frontend, backend, database integration, and production deployment.

Both repositories represent different parts of the Stockout Radar project.

---

## 📚 Acknowledgements

- Chen, D. (2012). *Online Retail II*. UCI Machine Learning Repository.
- Sukkur IBA University for providing the academic learning environment.
- The project team members for their contributions to machine learning development, preprocessing, and application implementation.

**Dataset License:** Creative Commons Attribution 4.0 (CC BY 4.0).

---

## 🤖 AI Use Disclosure

AI tools were used for guidance, debugging, and conceptual explanations during project development. Details of AI assistance are provided in the project's academic report and AI Use Disclosure.

---

**Developed as a Machine Learning Semester Project | Fall 2026**

⭐ If you find this project interesting, feel free to explore the repository and its implementation.