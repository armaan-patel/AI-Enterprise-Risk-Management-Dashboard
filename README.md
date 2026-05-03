# AI Enterprise Risk Management Dashboard

## Overview

This project is an end-to-end AI-driven operational risk management system built using the P-COLD operational loss dataset. It combines data engineering, feature engineering, machine learning, star schema design, and Power BI dashboarding to analyze historical operational risk patterns and forecast future high-loss risk events.

The goal of the project is to move beyond traditional historical reporting and create a system that supports proactive enterprise risk monitoring.

## Project Objectives

- Analyze historical operational risk events and loss patterns
- Identify risk concentrations by business line, event type, causal factor, and location
- Engineer predictive features from operational loss data
- Train machine learning models to forecast future high-loss risk events
- Compare model performance across 90-day, 180-day, and 365-day horizons
- Build an interactive Power BI dashboard for risk monitoring and decision support

## Dataset

This project uses the **P-COLD (Public Collection of Operational Loss Data)** dataset, a real-world operational risk dataset containing historical operational loss events.

The dataset includes fields such as:

- Event ID
- Occurrence year and month
- Loss amount
- Bank involved code
- Province and city
- Causal factor
- Event type
- Business line

> Note: Dataset materials are subject to their original source terms and are not owned by this repository.

## Project Pipeline

The project is organized into four main stages:

### 1. Synthetic Data Generation

Notebook: `data_generation.ipynb`

This notebook was used during early development to create synthetic Basel-style operational risk data. It helped prototype the pipeline, star schema, and dashboard structure before switching to the real P-COLD dataset.

This notebook is retained for completeness but is not the primary dataset used in the final project.

### 2. P-COLD Feature Engineering

Notebook: `pcold_feature_engineer.ipynb`

This notebook processes the raw P-COLD dataset and prepares it for modeling and reporting.

Key tasks include:

- Cleaning missing and inconsistent values
- Standardizing dates and categorical fields
- Converting loss amounts into consistent currency units
- Creating loss severity indicators
- Engineering lag features
- Engineering rolling window features
- Creating frequency-based encodings
- Generating future-risk targets for 90, 180, and 365-day horizons

Key outputs include:

- Cleaned event-level dataset
- Fact table
- Dimension tables
- Monthly modeling datasets
- Multi-horizon forecasting datasets

### 3. Star Schema Construction

Notebook: `star_schema.ipynb`

This notebook converts the processed operational risk data into a star schema for Power BI reporting.

The star schema includes:

- `fact_risk_events_star`
- `dim_date`
- `dim_risk_star`
- `dim_business_unit`
- `dim_owner`

This structure supports efficient slicing and filtering in the dashboard.

### 4. AI Risk Forecasting

Notebook: `risk_ai_model.ipynb`

This notebook trains and evaluates machine learning models to predict whether a high-loss operational risk event is likely to occur within a future time window.

Models used:

- Logistic Regression
- Random Forest
- Extra Trees

Key modeling techniques:

- Train/test split with stratification
- One-hot encoding for categorical variables
- Numeric feature imputation
- Class imbalance handling
- Probability-based prediction
- F1-based threshold tuning
- ROC-AUC, precision, recall, and F1 evaluation
- Multi-horizon comparison across 90, 180, and 365 days

## Machine Learning Approach

The primary forecasting target is:

`future_high_loss`

This target indicates whether a high-loss operational risk event occurs within a future horizon.

Forecasting horizons:

- 90 days: primary short-term forecasting window
- 180 days: medium-term comparison window
- 365 days: long-term comparison window

The model outputs predicted probabilities, allowing risk segments to be ranked by likelihood of future high-loss events.

## Power BI Dashboard

The Power BI dashboard contains five pages:

### 1. Executive Overview

Provides a high-level summary of:

- Total events
- Total net loss
- Critical events
- Model performance
- Top predicted risk records

### 2. Historical Operational Risk Trends

Analyzes historical risk patterns, including:

- Net loss over time
- Loss by business unit
- Event count by impact level
- Risk event type distribution

### 3. Risk Concentration and Severity

Highlights where losses are concentrated by:

- Business unit
- Event type
- Loss bucket
- Impact level
- Individual high-loss events

### 4. AI Forecasting

Displays model-generated risk predictions, including:

- High-risk count
- Average predicted risk probability
- Maximum predicted risk probability
- Risk by business line
- Risk by event type
- Top forecasted risk segments

### 5. Model Performance

Compares machine learning model performance using:

- ROC-AUC
- Precision
- Recall
- F1 score
- Threshold tuning
- 90-day, 180-day, and 365-day horizon comparisons

## Key Findings

- Internal fraud and external fraud are among the most frequent operational risk event types.
- A small number of high-severity events account for a disproportionate share of total losses.
- Risk is concentrated across specific business lines and event types.
- Longer forecasting horizons tend to improve model performance.
- Extra Trees generally performs strongest across several evaluation metrics.
- Threshold tuning is important because high-loss operational risk events are relatively rare.

## Tools and Technologies

- Python
- Jupyter Notebook
- pandas
- NumPy
- scikit-learn
- Power BI
- Star schema data modeling
- Machine learning classification
- Data visualization

## How to Run the Project

### 0. Only Dashboard

For the dashboard only, it is accessible through my portfolio and requires no download here:
https://armaanp6789.wixsite.com/armaan-patel-portfol/b-s-capstone

### 1. Clone the repository

```bash
git clone https://github.com/armaan-patel/AI-Enterprise-Risk-Management-Dashboard.git
cd AI-Enterprise-Risk-Management-Dashboard
```

### 2. Install required Python packages

```bash
pip install pandas numpy scikit-learn openpyxl joblib
```

### 3. Add the raw dataset

Place the P-COLD files in:

```text
data/raw/pcold/
```

Expected files:

- `P-COLD-English ver.xlsx`
- `Data dictionary.xlsx`

### 4. Run notebooks in order

Run the notebooks in this order:

1. `pcold_feature_engineer.ipynb`
2. `star_schema.ipynb`
3. `risk_ai_model.ipynb`

Optional:

- `data_generation.ipynb`

The synthetic data notebook is not required for the final P-COLD pipeline.

### 5. Open Power BI dashboard

Open the dashboard file:

```text
dashboard/AI_ERM_Dashboard.pbix
```

Refresh the data sources if needed and confirm that file paths match your local project folder.

## Main Outputs

Processed data outputs include:

- `fact_risk_events.csv`
- `fact_risk_events_star.csv`
- `dim_risk.csv`
- `dim_risk_star.csv`
- `dim_business_unit.csv`
- `dim_date.csv`
- `future_model_dataset_90d.csv`
- `future_model_dataset_180d.csv`
- `future_model_dataset_365d.csv`
- `future_risk_predictions.csv`
- `model_comparison_metrics.csv`
- `horizon_model_comparison_metrics.csv`

Power BI uses these outputs for historical analysis, forecasting visuals, and model performance reporting.

## Business Value

This project demonstrates how AI and analytics can support enterprise risk management by:

- Identifying high-risk operational areas
- Prioritizing risk monitoring
- Supporting proactive mitigation planning
- Improving visibility into loss concentration
- Comparing short-term and long-term risk forecast performance

## Limitations

- P-COLD provides structured event data but does not include detailed free-text narratives for every event.
- Some fields required preprocessing due to missing or inconsistent formats.
- Forecasting operational risk is challenging because high-loss events are rare and often irregular.
- The model is intended for analytical support, not as a standalone risk decision system.

## Future Improvements

Potential future enhancements include:

- Adding explainability methods such as SHAP values
- Incorporating external economic or regulatory indicators
- Building interactive model retraining workflows
- Adding more granular institution-level features
- Deploying the model through a web application or Streamlit interface
- Expanding the dashboard with scenario analysis

## Author

**Armaan Patel**  
B.S. Computer Science, DeSales University  
Incoming NYU Stern MBA

## License

This project’s code is licensed under the MIT License.

Dataset and template materials are subject to their original source licenses or usage terms.
