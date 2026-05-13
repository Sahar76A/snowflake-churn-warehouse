# snowflake-churn-warehouse
# Snowflake Churn Warehouse

Customer Churn Analytics & ML Scoring Platform  
Snowflake · Python · Power BI · AWS

---

## Overview

An end-to-end customer churn analytics platform that mirrors real-world data science and analytics engineering workflows.

The system ingests raw customer data, performs warehouse-native transformations in Snowflake, trains and scores a machine learning model, and delivers executive and operational dashboards in Power BI.

---

## Business Problem

Customer churn directly impacts revenue. This project enables stakeholders to:

- Understand who is churning and why  
- Quantify churn drivers (contract type, tenure, pricing, etc.)  
- Identify high-risk customers using ML probability scores  
- Take action using BI dashboards powered by warehouse logic  

---

## Architecture Overview

End-to-end pipeline:

Data Ingestion → Snowflake (RAW → STG → ANALYTICS) → Feature Engineering → ML Training & Scoring → BI Views → Power BI Dashboards

---

## AWS Context (Production-Realistic)

- Snowflake is deployed on AWS infrastructure  
- All compute and storage run on AWS-backed Snowflake services  
- Optional S3 landing zone for raw data ingestion  
- Snowflake external stages used for cloud-native loading  
- Warehouse-first design for transformations and analytics  

---

## Tech Stack

### Data & Processing
- Python 3.13  
- Pandas, NumPy  
- DuckDB (local validation & profiling)

### Data Warehouse
- Snowflake (RAW → STG → ANALYTICS)
- Warehouse-native data quality checks
- Semantic BI-ready views

### Machine Learning
- scikit-learn pipeline
- Logistic Regression model
- Metrics:
  - ROC AUC
  - PR AUC
  - Optimized decision threshold

### Business Intelligence
- Power BI (direct Snowflake connection)
- Executive + operational dashboards

### Cloud
- AWS (Snowflake-hosted environment)
- Optional S3 ingestion pattern

---

## Data Model (Snowflake)

### Core Tables
- ANALYTICS.CUSTOMER_CHURN_FEATURES  
- ANALYTICS.CUSTOMER_CHURN_SCORES  
- ANALYTICS.MODEL_RUNS  

### BI Views
- V_KPI_SNAPSHOT  
- V_CHURN_BY_CONTRACT  
- V_CHURN_BY_TENURE_BUCKET  
- V_CHURN_BY_MONTHLY_CHARGE_BAND  
- V_LATEST_CHURN_SCORES  
- V_TOP_RISK_CUSTOMERS  

These views provide a clean semantic layer for Power BI without duplicating logic.

---

## Machine Learning Workflow

- Feature extraction from Snowflake analytics schema  
- Train/test split  
- Data preprocessing pipeline:
  - Missing value imputation  
  - One-hot encoding  
- Logistic Regression model  
- Threshold optimization (PR/F1 tradeoff)  

### Model Output Write-back
- Model metadata stored in Snowflake  
- Per-customer churn probability scores saved  

### Performance
- ROC AUC ≈ 0.84  
- PR AUC ≈ 0.64  
- Optimized threshold ≈ 0.57  

---

## Design Decisions

- **Warehouse-first architecture:** All feature engineering lives in Snowflake for consistency between ML and BI  
- **Interpretable model:** Logistic Regression chosen for transparency and stable probability outputs  
- **Latest-run isolation:** BI views always reference the most recent model run to avoid stale data  

---

## Power BI Dashboards

### Executive Overview
- Total customers  
- Churn rate  
- Average monthly charges  
- Model performance (ROC AUC, PR AUC)  
- Churn breakdown by:
  - Contract type  
  - Tenure bucket  
  - Monthly charge band  

### Risk Targeting
- High-risk customer table  
- Churn probability filtering  
- Binary churn prediction  
- Latest model run only  

---

## Repository Structure

```text
snowflake-churn-warehouse/
│
├── src/
│   ├── config.py
│   ├── snowflake_client.py
│   ├── load_to_snowflake.py
│   ├── run_quality_checks.py
│   └── model_train_score.py
│
├── sql/
│   ├── 08_scoring_tables.sql
│   └── 09_bi_views.sql
│
├── data/
│   └── telco_churn.csv
│
├── powerbi/
│   └── churn_dashboard.pbix
│
├── run_local_duckdb.py
├── requirements.txt
├── .env.example
└── README.md
