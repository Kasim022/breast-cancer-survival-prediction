
# Breast Cancer Survival Prediction

Predicting survival in breast cancer patients using the SEER dataset: exploratory analysis, statistical testing, six machine learning models with hyperparameter tuning, and explainable AI (SHAP & LIME).

<img width="1149" height="653" alt="output" src="https://github.com/user-attachments/assets/975eea01-41e6-4081-8b79-6466c201ac4e" />
*SHAP summary: each dot is a patient; red = high feature value, dots to the right = higher predicted risk of death. Node Ratio, the engineered feature, is the strongest predictor.*
---

## Project Overview

The aim of this project is to predict whether a breast cancer patient is **alive or dead at the end of follow-up**, using clinical information available at diagnosis, and to explain **which factors drive that prediction**.

The full workflow:

1. **Data preprocessing** – missing value and duplicate checks (1 duplicate removed)
2. **Descriptive statistics** – summary statistics, class balance, chi-square and Mann–Whitney U tests
3. **Visualisation** – distributions, box plots, survival rate by category, correlation heatmap
4. **3D visualisation** – PCA and feature-based 3D scatter plots (Plotly)
5. **Feature engineering** – created **Node Ratio** (positive lymph nodes ÷ nodes examined); one-hot encoded categorical variables
6. **Machine learning** – baseline + 6 models, with class weighting to handle class imbalance
7. **Hyperparameter tuning** – GridSearchCV with 5-fold stratified cross-validation
8. **Explainable AI** – logistic regression coefficients, permutation importance, SHAP and LIME

---

## Dataset

**SEER Breast Cancer Data** – IEEE DataPort
🔗 https://doi.org/10.21227/a9qy-ph35

- Source: NCI Surveillance, Epidemiology, and End Results (SEER) Program, November 2017 update
- 4,024 female patients with infiltrating duct and lobular carcinoma, diagnosed 2006–2010
- 15 variables: age, race, marital status, T/N/6th stage, grade, A stage, tumour size, estrogen & progesterone status, regional nodes examined/positive, survival months, status
- Author: Jing Teng (North China Electric Power University)
- License: Creative Commons Attribution (CC BY)

**Target:** `Status` (Alive = 0, Dead = 1). The classes are imbalanced: **84.7% alive / 15.3% dead**.

> `Survival Months` was **excluded** from the models to prevent data leakage, since it is closely tied to the outcome.

---

## Results

Models were evaluated on a held-out, stratified 20% test set (805 patients). Because of the class imbalance, **ROC-AUC and recall on the "Dead" class** were used instead of accuracy.

| Model | Accuracy | Recall (Dead) | F1 (Dead) | ROC-AUC |
|---|---|---|---|---|
| Logistic Regression | 0.737 | 0.618 | 0.418 | **0.749** |
| Logistic Regression (tuned) | 0.729 | 0.585 | 0.398 | 0.746 |
| XGBoost (tuned) | 0.719 | 0.593 | 0.392 | 0.736 |
| SVM (tuned) | 0.729 | 0.553 | 0.384 | 0.729 |
| Random Forest (tuned) | 0.739 | 0.537 | 0.386 | 0.727 |
| KNN | 0.843 | 0.073 | 0.125 | 0.705 |
| MLP | 0.846 | 0.089 | 0.151 | 0.691 |
| Dummy (always "alive") | 0.847 | 0.000 | 0.000 | 0.500 |

### Key findings

- **Accuracy is misleading here:** the dummy model scores the highest accuracy (84.7%) while identifying no deaths.
- **Logistic regression performed as well as or slightly better than** more complex models such as XGBoost and Random Forest. The top models fall within ~0.02 AUC of each other, suggesting the relationships in this data are largely linear and the dataset itself limits performance.
- **Hyperparameter tuning mattered for XGBoost**, improving its AUC from 0.694 to 0.736.

### Explainable AI

SHAP, LIME, permutation importance and logistic regression coefficients **agreed** on the main drivers:

- **Increase risk of death:** Node Ratio (the engineered feature, ranked #1 across methods), number of positive lymph nodes, age, high tumour grade (III/IV), higher N/T stage
- **Decrease risk of death:** progesterone receptor positive, estrogen receptor positive, low tumour grade (I)

These findings match established clinical knowledge, supporting that the models learned meaningful patterns.

---

## Limitations

- The dataset only includes **stage II–III** patients, which reduces the apparent importance of stage compared with real-world data.
- `Status` reflects **death from any cause**, not only breast cancer.
- Follow-up is limited (maximum ~107 months), so "Alive" means alive at the end of follow-up.
- US data from 2006–2010 only; results may not generalise to other populations or time periods.
- Variables such as race and marital status show **associations, not causes**. They likely reflect unmeasured factors such as access to healthcare.
- The model predicts **survival among patients who already have breast cancer**. It does not identify causes of breast cancer.

---

## How to Run

1. Clone the repository:
   ```bash
   git clone https://github.com/Kasim022/breast-cancer-survival-prediction.git
   cd breast-cancer-survival-prediction
   ```
2. Install the requirements:
   ```bash
   pip install -r requirements.txt
   ```
3. Open `Breast Cancer.ipynb` in Jupyter or VS Code and run all cells.

---

## Tools & Libraries

Python · pandas · NumPy · Matplotlib · Seaborn · Plotly · SciPy · scikit-learn · XGBoost · SHAP · LIME

---

## Acknowledgements

- Dataset by Jing Teng via [IEEE DataPort](https://doi.org/10.21227/a9qy-ph35), derived from the [NCI SEER Program](https://seer.cancer.gov/).
- This project was completed with AI assistance from **Claude (Anthropic)**, used for guidance on methodology, code explanations and debugging.

---

## Connect with Me

- **LinkedIn:** [linkedin.com/in/mkasimabbas](https://www.linkedin.com/in/mkasimabbas/)
- **Kaggle:** [kaggle.com/kasima022](https://www.kaggle.com/kasima022)
- **GitHub:** [github.com/Kasim022](https://github.com/Kasim022)
