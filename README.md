# 📊 Student Academic Performance — Data Preprocessing Pipeline

[![Python](https://img.shields.io/badge/Python-3.9%2B-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![pandas](https://img.shields.io/badge/pandas-150458?style=for-the-badge&logo=pandas)](https://pandas.pydata.org)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white)](https://scikit-learn.org)

> A 9-step data preprocessing pipeline applied to a student academic
> performance dataset — cleaning, encoding, scaling, and preparing
> data for machine learning.

---

## 🔍 Project Overview

Raw datasets are rarely machine-learning ready. This project documents
a complete preprocessing pipeline applied to a 525-record student
academic performance dataset, identifying and resolving five categories
of real-world data quality issues before producing two output files:
a human-readable cleaned dataset and a fully transformed ML-ready dataset.

**Dataset:** 525 students across three university courses
(Data Science, Economics, Statistics), with 9 attributes including
demographics, academic performance, and socioeconomic indicators.

---

## 📋 Data Quality Issues Found & Resolved

| Issue | Before | After |
|-------|--------|-------|
| Age data type | Object (mixed integers and text) | Integer — fully numeric |
| Text age values | 'Twenty', 'Nineteen' in 2 rows | Converted to 20, 19 |
| Gender categories | 5 inconsistent variants ('M', 'Male', 'male', 'F', 'Female') | 2 canonical: Male, Female |
| Course categories | 5 inconsistent variants (casing, trailing spaces) | 3 canonical: title case |
| Missing Study_Hours | 1 missing value | Imputed with mean (4.99) |
| Missing Attendance | 1 missing value | Imputed with mean (70.31) |
| Duplicate rows | 25 fully duplicated records | Removed — 0 remaining |
| Outliers | Tested via Z-score | None detected |
| Noise | Domain-range checks applied | No out-of-range values found |
| **Total records** | **525** | **500** |
| **Total columns** | **9** | **17 (ML-ready file)** |

---

## ⚙️ Pipeline Steps

| Step | Operation | Key Decision |
|------|-----------|-------------|
| 1 | Load data | pd.read_excel() + full EDA suite |
| 2 | Inspect | df.info(), df.describe(), unique() on categoricals |
| 3 | Handle missing values & inconsistencies | Mean imputation (justified by symmetry analysis); text-to-numeric Age conversion |
| 4 | Remove duplicates | 25 fully duplicated rows dropped |
| 5 | Detect outliers | Z-score method (threshold = 3); 0 outliers found |
| 5b | Export cleaned dataset | Pre-transformation checkpoint saved |
| 6 | Scale numeric features | Min-Max Scaling chosen because 0 outliers found |
| 7 | Encode categorical variables | Label Encoding (binary); One-Hot Encoding (multi-class, drop_first=True) |
| 8 | Handle noise | Domain-range checks on Attendance and Study_Hours |
| 9 | Reinspect | Shape, nulls, duplicates — all confirmed clean |

---

## 📁 Repository Structure

```
student-performance-preprocessing/
├── NUTDTS_815_Project_Precious_Faseyosan.ipynb   # Full preprocessing notebook
├── student_preprocessing_dataset_project.xlsx    # Raw dataset (525 records)
├── cleaned_student_dataset.xlsx                  # Output 1: cleaned, pre-transformation (500 records, 9 cols)
├── ML_ready__student_dataset.xlsx                # Output 2: scaled, encoded, ML-ready (500 records, 17 cols)
└── README.md
```

---

## 📤 Output Files

| File | Records | Columns | Description |
|------|---------|---------|-------------|
| `cleaned_student_dataset.xlsx` | 500 | 9 | Cleaned and standardised. Text categories corrected, duplicates removed, missing values imputed. No scaling or encoding applied — human-readable. |
| `ML_ready__student_dataset.xlsx` | 500 | 17 | Fully transformed. Numeric features Min-Max scaled. Gender and Final_Result label-encoded. Course one-hot encoded. Student_ID dropped. |

**Student_ID** is saved separately with its original index so model predictions
can be traced back to individual students after training.

---

## 🧰 Tech Stack

| Tool | Purpose |
|------|---------|
| pandas | Data loading, cleaning, transformation |
| scikit-learn (MinMaxScaler, LabelEncoder) | Scaling and encoding |
| scipy.stats (zscore) | Outlier detection |

---

## 🔑 Key Decisions Documented

**Why mean imputation, not median?**
Skewness scores for all five numeric columns fell between -0.5 and +0.5,
and mean/median differences were negligible. Approximately symmetric
distributions make mean imputation valid and appropriate.

**Why Min-Max Scaling, not Standard Scaling?**
Standard Scaling (Z-score normalisation) is preferred when significant
outliers are present. Step 5 found zero outliers across all columns —
making Min-Max the more appropriate choice for producing a bounded,
interpretable [0, 1] range.

**Why drop_first=True in One-Hot Encoding?**
With 3 course categories, only 2 binary columns are needed. The third
is always implied when both generated columns equal 0. Dropping it
avoids multicollinearity without losing any information.

**Why Label Encoding for Gender and Final_Result, not One-Hot?**
Both are binary columns (2 categories only). Label Encoding is safe
here because no false ordering is implied — Male/Female and Pass/Fail
have no numerical relationship a model could misinterpret.

---

## 👤 Author

**Precious Faseyosan**
Graduate Petroleum Engineer | MSc Data Science Candidate

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=flat&logo=linkedin)](https://www.linkedin.com/in/precious-faseyosan)
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat&logo=github)](https://github.com/PreciousFaseyosan)
