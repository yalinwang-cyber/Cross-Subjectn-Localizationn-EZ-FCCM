# Cross-Subject Localization of Epileptogenic Zone for Drug-Resistant Epilepsy Based on the Causal Brain Network

This repository contains the codebase for our article, which uses stereoelectroencephalography (SEEG) data and causal brain network features calculated via full convergent cross mapping (FCCM) to localize the epileptogenic zone (EZ) in drug-resistant epilepsy patients. The code repository mainly consists of two parts: a statistical analysis script based on PERMANOVA, and a machine learning classification script based on Leave-One-Patient-Out cross-validation, used to distinguish between the EZ and NEZ. The dataset used is composed of statistical features of the aforementioned causal network metrics (including the mean values, maximum values, minimum values, and standard deviations of inward and outward information flows for EZ and NEZ channels; for the EZ, inward flow represents information flowing from NEZ to EZ, and outward flow represents information flowing from EZ to NEZ, while the definition for NEZ is the opposite).

## Project Structure

```text
├── statistics/              # Raw SEEG feature datasets
│   ├── ictal/               # Ictal period excel files (.xlsx)
│   └── interictal/          # Interictal period excel files (.xlsx)
├── statistic.py             # Statistical analysis script (PERMANOVA calculation)
├── machine_learning.py      # Binary classification modeling script (localize EZ)
├── statistics_result/      # Output directory for statistical results (auto-generated)
└── machine_result/          # Output directory for model evaluation summaries (auto-generated)
```

## Core content

### 1. Statistical Analysis (`statistic.py`)
- Evaluate the statistical differences in feature distributions between the EZ and NEZ for each patient:
- **Distance Matrix & Normalization:** Automatically construct sample-to-sample distance matrices using Z-score standardization and Euclidean distance.
- **PERMANOVA Test:** Perform Permutational Multivariate Analysis of Variance (10000 permutations) to test group differences.
- **Effect Size Calculation:** Compute Cohen's d effect size based on pseudo-F statistics and unequal variance degrees of freedom.

### 2. Machine Learning Classification (`machine_learning.py`)
Support binary classification tasks under 3 data modes (EZ = 1 vs NEZ = 0):
- **Analysis Modes:** Support `ictal` (ictal period only), `interictal` (interictal period only), and `combined` (combined ictal and interictal features).
- **Class Imbalance Handling:** Integrate 3 resampling strategies (SMOTE, RandomUnderSampler, SMOTEENN), applied strictly on training folds to prevent data leakage.
- **Classifier Benchmarking:** Cover 11 mainstream machine learning algorithms (SVM, XGBoost, Random Forest, Logistic Regression, KNN, Decision Tree, Naive Bayes, AdaBoost, LDA, MLP, LightGBM).
- **Validation Strategy:** Implement Leave-One-Patient-Out Cross-Validation to ensure out-of-sample generalization capability of the model.
- **Evaluation Metrics:** Comprehensively evaluate Precision, Recall, Specificity, Accuracy, F1-score, AUC, and AUPRC. Summary results are automatically exported to `machine_result/`.

## Installation and Environment Requirements
Please ensure Python 3.8+ and the following dependency libraries are installed:

```bash
pip install numpy pandas scipy scikit-learn xgboost lightgbm imbalanced-learn openpyxl
```

## Usage Guide
1. Place the data into the corresponding subdirectories under the `statistics/` folder (`statistics/ictal/` and `statistics/interictal/`).
2. Run the statistical analysis script:

```bash
python statistic.py
```

3. Run the machine learning modeling script (you can modify the `mode` variable in `machine_learning.py` to `'ictal'`, `'interictal'`, or `'combined'`):

```bash
python machine_learning.py
```
