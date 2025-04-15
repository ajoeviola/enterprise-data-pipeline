# Customer Churn Prediction Pipeline with Databricks and scikit-learn

An end-to-end machine learning pipeline for predicting customer churn using Databricks, Delta Lake, and scikit-learn. This project simulates an enterprise-grade data solution that ingests raw transaction data, performs data engineering, trains a predictive model, and scores new customer data.

---

## 🚀 Project Overview

**Goal:** Predict whether a customer will make a repeat purchase based on transactional behavior.

**Tech Stack:**
- **Databricks Notebooks** for ingestion, cleaning, and modeling
- **Delta Lake** for structured data storage
- **scikit-learn** for model training and evaluation
- **Pandas** for data manipulation
- **joblib** for saving/loading models

---

## 📂 Project Structure
```
enterprise-data-pipeline/
├── data/                      # Sample raw data or generated CSVs
├── notebooks/
│   ├── 01_data_ingestion.ipynb
│   ├── 02_model_training.ipynb
│   └── 03_Generate_Churn_Prediction.ipynb
├── models/
│   ├── churn_model.pkl
│   └── pipeline.pkl
├── scored_customers.csv      # Example output file
├── requirements.txt
└── README.md
```

---

## 🧪 Data Pipeline Stages

### 1. Data Ingestion
- Source: https://www.kaggle.com/datasets/logiccraftbyhimanshi/walmart-customer-purchase-behavior-dataset?resource=download
- Ingested into Databricks via notebook
- Written to `/delta/cleaned_customer_data` as Delta Lake format

### 2. Data Cleaning & Engineering
- Converted and standardized types
- Extracted temporal features from purchase date
- One-hot encoded categorical variables
- Aggregated data to customer level (total spend, avg rating, etc.)


### 3. Model Training
- Trained a Logistic Regression model using `scikit-learn`
- Tuned using recall as the optimization goal in order to identify positives churn candidates, while accepting false positives as a trade-off
- Achieved:
  - **Recall (class 1):** 0.75
  - **F1 Score (class 1):** 0.60
  - **Precision (class 1):** ~0.51
- Saved model and pipeline using `joblib`

### 5. Batch Inference
- New customer data loaded from `new_customers.csv`
- Preprocessed using saved pipeline
- Predictions generated using saved model
- Output saved to `scored_customers.csv`

---

## 📈 Example Output
| Customer_ID | Predicted_Return | Return_Probability |
|-------------|------------------|---------------------|
| 12345       | 1                | 0.81                |
| 67890       | 0                | 0.42                |

---

## 🧠 Model Usage
To generate churn predictions:

```bash
# Load model and pipeline
model = joblib.load("models/churn_model.pkl")
pipeline = joblib.load("models/pipeline.pkl")

# Load and transform new customer data
new_df = pd.read_csv("new_customers.csv")
new_df_aggregated = aggregate_customer_data(new_df) 
X_new = pipeline.transform(new_df_aggregated)
predictions = model.predict(X_new)
```

---

## 📊 Future Enhancements
- Add support for MLflow tracking
- Deploy Streamlit UI for non-technical users
- Trigger automated workflows via Databricks Jobs or GitHub Actions
- Store scored data in Delta tables for BI tools
