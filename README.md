# NFL Playoff Prediction - Decision-Support Analytics

## Project Overview

This project develops a data-driven decision-support framework that predicts NFL playoff qualification using publicly available team-season statistics from Pro-Football-Reference (2018-2025). The analysis evaluates team quality through contextual performance indicators (efficiency metrics, turnover behavior, schedule context, and pass rush quality) rather than raw win-loss records.

## Repository Structure

### Data Files

- `Team_Stats_Merge_2018_2025.csv` - Raw merged dataset (256 rows, 121 columns) combining 8 Pro-Football-Reference source tables
- `Team_Stats_Merge_2018_2025_Data_Dictionary.docx` - Column definitions for the raw dataset
- `modeling_ready_dataset.csv` - Cleaned, engineered dataset ready for modeling (256 rows, 67 columns)

### Code / Notebooks

**Assignment 1 (Business and Data Understanding)**

- `build_team_season_master.ipynb` - Data collection and integration pipeline joining 8 source tables
- `eda_analysis.ipynb` - Exploratory data analysis including correlation heatmaps and distribution analysis

**Assignment 2 (Data Preparation and Modeling)**

- `feature_selection_analysis.ipynb` - Systematic evaluation of all 121 columns across 11 exclusion categories, supporting Section 1.1 variable selection decisions
- `data_preparation.ipynb` - Execution pipeline: variable selection (121 to 60 columns), missing data imputation, outlier documentation, feature engineering (7 differential features), and export
- `modeling_mm.ipynb` - Full modeling pipeline: expanding window setup, baseline models, Logistic Regression, Random Forest, Gradient Boosting, cross-model comparison, and diagnostic plots

### Reports

- `Assignment_1_-_Business_Phase_-_Completed_Written_Report.docx` - Assignment 1 written report
- `Assignment_2_Final.docx` - Assignment 2 written report (Data Preparation and Modeling)

### Plot Outputs

- `plot_model_comparison.png` - Grouped bar chart comparing average model performance
- `plot_confusion_matrices.png` - Confusion matrices for all 3 models on 2025 holdout
- `plot_f1_across_folds.png` - F1 score stability across 5 expanding window folds
- `plot_feature_importance.png` - Top 10 features by model comparison
- `plot_roc_curves.png` - ROC curves for all 3 models on 2025 holdout

## How to Run

### Prerequisites

Install required packages:

```
pip install -r requirements.txt
```

### Execution Order

1. **feature_selection_analysis.ipynb** - Run first to see the analytical evidence behind variable selection decisions
2. **data_preparation.ipynb** - Run second to produce `modeling_ready_dataset.csv`
3. **modeling_mm.ipynb** - Run third to train models and generate results and plots

Note: `build_team_season_master.ipynb` and `eda_analysis.ipynb` are Assignment 1 notebooks included for completeness. The Assignment 2 pipeline starts from `Team_Stats_Merge_2018_2025.csv` which was produced by the Assignment 1 build script.

## Key Findings

- Logistic Regression with L1 regularization was the best performing model (0.884 avg F1, 0.970 AUC-ROC)
- It outperformed both Random Forest (0.779 F1) and Gradient Boosting (0.825 F1)
- The simplest model outperforming complex alternatives indicates the relationships are fundamentally linear
- Scoring efficiency differential (r = +0.704) emerged as the single strongest predictor
- Defensive quality metrics (air yards allowed, yards after catch) were the strongest linear predictors
- On the 2025 final holdout, Logistic Regression correctly classified 30 of 32 teams (93.8% accuracy)

## Target Variable

- `made_playoffs`: Binary (1 = team qualified for playoffs, 0 = did not qualify)
- Distribution: 108 playoff (42.2%) vs 148 non-playoff (57.8%) across 256 team-seasons

## Validation Approach

Expanding-window temporal validation with 5 folds:

- Fold 1: Train 2018-2020, Test 2021
- Fold 2: Train 2018-2021, Test 2022
- Fold 3: Train 2018-2022, Test 2023
- Fold 4: Train 2018-2023, Test 2024
- Fold 5: Train 2018-2024, Test 2025
