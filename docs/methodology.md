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

# - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

# SECTION: LEARNING & SKILL DEVELOPMENT

# Maintainer: John SilkSong

# - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

## 🧠 Insight: Learning & Skill Development

**Research Question:** How do learning methods correlate with experience level and technology adoption?

To answer this, a multi-stage analysis was conducted to first map the evolution of learning habits across a developer's career and then to quantify the link between specific learning resources and the adoption of different technology classes.

---

### 🔧 1. Initial Data Preparation

The primary challenge in this analysis was handling the multi-select nature of the survey questions `LearnCode` (QID276) and `LearnCodeOnline` (QID281). The raw data contains semicolon-separated strings in these columns.

- **Action:** Each of these columns was **one-hot encoded**. This process transforms a single column (e.g., `LearnCode`) into multiple binary (0/1) columns (e.g., `Learn_School`, `Learn_Books`, `Learn_On the job training`, etc.).
- **Reason:** This transformation is essential for quantitatively analyzing each learning method as an independent feature. It allows for aggregation (e.g., calculating percentages) and use in statistical models.

---

### 📊 2. Analysis of Learning vs. Experience Level

The goal here was to understand the trends and associations between a developer's career stage and their preferred learning methods.

#### a) Descriptive Analysis & Heatmap

- **What:** We grouped the preprocessed data by the `experience_category` feature ('Junior', 'Mid', 'Senior', 'Expert'). For each group, we calculated the mean of every binary learning column. This mean represents the percentage of developers in that group who use that specific learning method.
- **Why:** A heatmap was chosen to visualize these percentages because it makes it easy to spot **longitudinal trends**. We could immediately identify which methods see increased, decreased, or stable usage as experience grows.

#### b) Correspondence Analysis (CA)

- **What:** Correspondence Analysis is a statistical technique used to visualize the relationships and associations between two categorical variables. It projects the categories onto a 2D "perceptual map."
- **Why:** While the heatmap shows raw percentages, CA reveals the _relative strength of association_. It answers the question: "Which learning methods are most _distinctive_ or _characteristic_ of a Junior developer, compared to all other groups?" Methods that are universally popular (like "online resources") appear near the center, while more unique methods are pushed to the outer edges, closer to the experience levels they are most strongly associated with.
- **How:**
  1.  A **contingency table** of raw counts (not percentages) was created between `experience_category` and the one-hot encoded `LearnCode` columns.
  2.  The `prince` library in Python was used to fit the CA model to this table.
  3.  The resulting coordinates for each category were plotted to create the perceptual map.

---

### 🚀 3. Analysis of Learning Pathways for Technology Adoption

This analysis aimed to uncover if the adoption of "emerging" versus "established" technologies is fueled by different learning ecosystems.

#### a) Lift Score Calculation

- **What:** Lift is a metric from association rule mining that measures how much more likely two items are to occur together than if they were statistically independent.
  - **Formula:** `Lift(A, B) = P(A and B) / (P(A) * P(B))`
  - **In our context:** `Lift(Learning Method, Technology) = (Percentage of Tech users who use the method) / (Overall percentage of developers who use the method)`
- **Why:** Lift is more powerful than simple correlation. A Lift score greater than 1.0 indicates a positive association. For example, a Lift of 1.3 means developers using that specific tech are **30% more likely** to use that learning method than the average developer, making it a strong indicator of a preferred learning pathway.
- **How:**
  1.  Technology groups were defined: **Emerging** (e.g., Rust, Go) and **Established** (e.g., Java, JavaScript).
  2.  The Lift score was calculated for each `LearnCodeOnline` resource against each technology group.

#### b) Dumbbell Plot Visualization

- **Why:** A dumbbell plot was chosen over a standard bar or dot plot because its primary purpose is to **emphasize the gap** between two points. By connecting the "Established" and "Emerging" lift scores for each resource, the chart immediately draws the viewer's attention to the learning methods with the most significant divergence in preference, telling a clearer, more comparative story.
