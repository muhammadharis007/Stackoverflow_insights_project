# 🧩 Stack Overflow Developer Survey 2025 — Data Overview

### 📘 Project Summary

This repository contains the **cleaned and feature-engineered version** of the 2025 Stack Overflow Developer Survey.  
The dataset has been standardized, encoded, and validated for downstream analysis and model building.

All preprocessing follows the **official schema** provided in `survey_results_schema.csv`.  
Each transformation is deterministic, logged, and reproducible.

---

## ⚙️ Preprocessing Summary

| Stage                         | Description                                                                          | Output             |
| ----------------------------- | ------------------------------------------------------------------------------------ | ------------------ |
| **1. Load & Validate Schema** | Imported schema CSV and matched all questions to data columns                        | ✅ Schema verified |
| **2. Data Cleaning**          | Trimmed spaces, standardized casing, normalized country codes, and fixed known typos | ✅ Clean           |
| **3. Missing Data Handling**  | Imputed missing numeric fields using median; dropped survey metadata columns         | ✅ Consistent      |
| **4. Multi-Select Expansion** | Split multi-answer questions into binary dummy columns                               | ✅ Done            |
| **5. Numeric Conversions**    | Converted salary, experience, and age fields to numeric ranges                       | ✅ Verified        |
| **6. Outlier Winsorization**  | Capped extreme salaries at 1st/99th percentiles                                      | ✅ Stable          |
| **7. Derived Features**       | Added AI usage flags, experience bands, and region grouping                          | ✅ Created         |
| **8. Export & Logging**       | Exported cleaned datasets + processing logs                                          | ✅ Complete        |

---

## 📂 Final Deliverables

| File                        | Description                                                |
| --------------------------- | ---------------------------------------------------------- |
| `cleaned_full.csv`          | Cleaned, validated dataset with all survey fields          |
| `feature_engineered.csv`    | Reduced feature set with engineered variables for modeling |
| `survey_results_schema.csv` | Schema reference (original Stack Overflow structure)       |
| `preprocessing_log.txt`     | Step-by-step record of preprocessing actions               |

---

## 🔍 Quick Usage

```python
import pandas as pd

df = pd.read_csv("feature_engineered.csv")

# Example: analyze AI sentiment among AI users
df_ai = df[df['AISent'].notna()]
df_ai.groupby('is_ai_user')['ai_sentiment_score'].mean()
```

---

## 🚦 Data Status

| Check                 | Result           |
| --------------------- | ---------------- |
| Schema alignment      | ✅ Passed        |
| Missing key fields    | ❌ None          |
| Duplicate responses   | ❌ None          |
| Encoding completeness | ✅ Verified      |
| Derived feature logic | ✅ Cross-checked |
| Export integrity      | ✅ Confirmed     |

---

## 👥 Notes for Collaborators

- Use `feature_engineered.csv` for ML or dashboards — it’s leaner and consistent.
- Use `cleaned_full.csv` for deep exploration or new feature creation.
- All scripts are located in `/src/preprocessing/`.
- Random seed fixed at `42` for reproducibility.
- Keep transformations modular; update both code and documentation together.

---

## 🧠 Next Steps (Team To-Do)

- [ ] Add exploratory notebooks in `/notebooks/EDA/`
- [ ] Document feature definitions in `/docs/features.md`
- [ ] Integrate cleaned data into analysis pipeline
- [ ] Review edge-case handling for regional salary normalization

---

**Maintainer:** _Data Foundation Team — Stack Overflow Survey 2025_  
**Version:** v1.0 (Oct 2025)  
**Status:** ✅ Ready for Analysis
