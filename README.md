# 📊 English Premier League Match Outcome Prediction (EPL Home Win Prediction)

This project uses Python to analyze English Premier League match data (2000–2018) and build classification models to predict whether the home team wins (`Home Win` / `Not Home Win`).

---

## 👥 Team & Responsibilities

The project was completed by a team of 4 members with the following responsibilities:

| Name | Role | Responsibilities |
| :---- | :---- | :---- |
| **Vu Ngoc Cam Tu** | Team Lead / Data Engineer | Selected the dataset and defined the prediction task. Responsible for `02_LamSachDuLieu.ipynb`: removing redundant columns, scaling by Match Week, one-hot encoding and producing `cleaned_data.csv`. |
| **Nguyen Anh Tu** | Data Analyst + Machine Learning Engineer  | Co-authored `01_KhamPhaDuLieu.ipynb` and led `03_HuanLuyenModel.ipynb`: temporal train/test split, trained Logistic Regression and summarized accuracy results.|
| **Nguyen Cao Nhat Minh** | Data Analyst + Machine Learning Engineer | Co-authored `01_KhamPhaDuLieu.ipynb` and led `03_HuanLuyenModel.ipynb`: temporal train/test split, trained Logistic Regression and summarized accuracy results. |
| **Truong Khanh Huyen** | Data Engineer / Documenter | Supported data cleaning in `02_LamSachDuLieu.ipynb` and wrote project documentation (`README.md`). |

---

## 📁 Repository Structure

```
EPL-HomeWin-Prediction/
│
├── data/                          <- Data files
│   ├── raw_data.csv               <- Raw input (do not modify)
│   └── cleaned_data.csv           <- Cleaned dataset (used for modeling)
│
├── notebooks/                     <- Jupyter notebooks
│   ├── 01_KhamPhaDuLieu.ipynb   <- Step 1: Exploration and visualizations
│   ├── 02_LamSachDuLieu.ipynb          <- Step 2: Data cleaning and preprocessing
│   └── 03_HuanLuyenModel.ipynb     <- Step 3: Model training and evaluation
│
└── README.md                      <- Project documentation
```

---

## 🔍 Data Source

- English Premier League match history covering seasons **2000/01 through 2017/18**.
- Dataset contains **6,840 matches** and **39 columns** of pre-match statistics, cumulative performance, goal differences, and recent form indicators.
- The raw dataset is transformed into a cleaned modeling dataset with no leaked final score information.

---

## 💡 Key Findings

- **Home advantage is strong:** home teams average **1.53 goals** per match versus **1.13 goals** for away teams.
- **Structured missing values:** the value `'M'` in `HM1–HM5` and `AM1–AM5` indicates unavailable pre-match form data at the beginning of a season.
- **Most predictive signals:** difference in cumulative points (`DiffPts`), difference in form points (`DiffFormPts`), and goal differentials (`HTGD`, `ATGD`).
- **Clean data readiness:** after date conversion, scaling, and encoding, the cleaned dataset is ready for model training without major NaN issues.

---

## 🧾 Detailed Report — 3 Parts

### 1. Data Understanding (EDA)
- Objective: verify the dataset before preprocessing.
- Actions:
  - loaded the raw dataset and inspected rows, data types, summary statistics, and missing values.
  - examined match distributions, home/away goal statistics, and the structure of the `HMx` / `AMx` recent-result columns.
  - confirmed that `Date` requires datetime conversion and recent-result values require categorical encoding.

### 2. Data Cleaning & Preprocessing
- Objective: produce a reliable feature set for classification.
- Actions:
  - converted `Date` to datetime and sorted by match date.
  - removed leakage and redundant columns such as `FTHG`, `FTAG`, `HTGS`, `ATGS`, `HTGC`, `ATGC`, `HomeTeam`, `AwayTeam`, `HTFormPtsStr`, and `ATFormPtsStr`.
  - dropped weak or redundant history features: `HM4`, `HM5`, `AM4`, `AM5`.
  - scaled cumulative match statistics by `MW` to create per-match averages and then dropped `MW`.
  - one-hot encoded recent result columns `HM1–HM3` and `AM1–AM3`, preserving `M` as a separate category.
- Output: `data/cleaned_data.csv` with clean, encoded features for modeling.

### 3. Modelling & Evaluation
- Objective: predict the home-team win outcome with a temporally valid split.
- Actions:
  - loaded `data/cleaned_data.csv`, ordered matches chronologically, and split the dataset into an earlier training set and a later test set.
  - selected features including:
    - `DiffPts`, `DiffFormPts`, `HTGD`, `ATGD`
    - recent form streak features for home and away teams
    - one-hot encoded recent-result indicators for `HM1–HM3` and `AM1–AM3`
  - standardized features using `StandardScaler` fit on training data.
  - compared three models:
    - `LogisticRegression`
    - `RandomForestClassifier`
    - `XGBClassifier`
  - evaluated models using accuracy, precision, recall, F1-score, and ROC-AUC.
  - visualized confusion matrices and ROC curves to validate performance.

---

## 📊 Results

- Models were benchmarked against a majority-class baseline to confirm predictive power.
- The strongest predictive features are:
  - `DiffPts`
  - `DiffFormPts`
  - `HTGD`
  - `ATGD`
- Model comparison summary:

| Model | Purpose | Evaluation Focus |
| --- | --- | --- |
| Logistic Regression | Interpretable baseline | Balanced precision/recall behavior |
| Random Forest | Nonlinear ensemble | Captures interactions between cumulative and form features |
| XGBoost | Gradient-boosted ensemble | Maximizes predictive ranking and robustness |

> Exact numeric test metrics are available in `03_HuanLuyenModel.ipynb`.

---

## 🚀 How to Run

```bash
pip install pandas numpy matplotlib seaborn scikit-learn xgboost jupyter
jupyter notebook
```

Run notebooks in order:
1. `01_KhamPhaDuLieu.ipynb`
2. `02_LamSachDuLieu.ipynb`
3. `03_HuanLuyenModel.ipynb`

---

## 👇 Notes

- Keep `data/raw_data.csv` unchanged and use `data/cleaned_data.csv` for model training.
- The workflow is designed to reproduce the full pipeline from data exploration through model evaluation.
