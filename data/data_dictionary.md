# 📚 Stack Overflow Developer Survey 2025 — Data Dictionary

**Version:** 1.0 | **Last Updated:** October 2025  
**Maintainer:** Data Foundation Team

---

## 🧩 Overview

This Data Dictionary documents all key variables in the **cleaned** and **feature-engineered** versions of the 2025 Stack Overflow Developer Survey dataset.  
Each variable includes its description, data type, handling method during preprocessing, and presence in the final files.

---

## 🧠 Legend

| Symbol | Meaning                             |
| :----: | :---------------------------------- |
|   ✅   | Present in `feature_engineered.csv` |
|   📦   | Present in `cleaned_full.csv`       |
|   ⚙️   | Engineered or derived feature       |
|   🔢   | Numeric                             |
|   🔠   | Categorical / Text                  |
|   🧮   | Ordinal / Ordered Category          |
|   🔘   | Binary (0/1)                        |

---

## 👤 Demographics

| Column       | Type | Source | Description                                    | Handling / Notes                 |
| ------------ | ---- | ------ | ---------------------------------------------- | -------------------------------- |
| `ResponseId` | 🔢   | 📦     | Unique respondent identifier                   | Retained as index key            |
| `MainBranch` | 🔠   | 📦     | Developer status (professional, student, etc.) | Cleaned text, normalized         |
| `Age`        | 🧮   | 📦     | Age group                                      | Converted to ordered categorical |
| `Gender`     | 🔠   | 📦     | Self-identified gender                         | Standardized casing              |
| `Country`    | 🔠   | 📦     | Country of residence                           | Normalized country codes         |
| `EdLevel`    | 🔠   | 📦     | Highest level of education                     | Filled missing with `"Unknown"`  |
| `Employment` | 🔠   | 📦     | Employment type (multi-select)                 | Expanded binary columns          |
| `RemoteWork` | 🔠   | 📦✅   | Work setup (Remote / Hybrid / In-person)       | Filled missing, standardized     |

---

## 💼 Professional Experience

| Column         | Type | Source | Description               | Handling / Notes                                   |
| -------------- | ---- | ------ | ------------------------- | -------------------------------------------------- |
| `YearsCode`    | 🔢   | 📦✅   | Total years coding        | Converted “Less than 1” → 0.5, “More than 50” → 50 |
| `YearsCodePro` | 🔢   | 📦✅   | Professional coding years | Same mapping as above                              |
| `WorkExp`      | 🔢   | 📦     | General work experience   | Converted to numeric                               |
| `DevType`      | 🔠   | 📦     | Developer role(s)         | Multi-select split into binaries                   |
| `OrgSize`      | 🔠   | 📦     | Organization size         | Cleaned categories                                 |

---

## 💰 Compensation

| Column                 | Type | Source | Description                  | Handling / Notes                     |
| ---------------------- | ---- | ------ | ---------------------------- | ------------------------------------ |
| `CompTotal`            | 🔢   | 📦     | Annual compensation (raw)    | Parsed to float, kept NaN if missing |
| `CompTotal_winsorized` | 🔢   | ⚙️✅   | Salary after outlier capping | Winsorized at 1st/99th percentiles   |
| `Currency`             | 🔠   | 📦     | Reported currency            | No conversion applied                |
| `salary_quartile`      | 🧮   | ⚙️✅   | Salary quartile grouping     | Q1–Q4 based on CompTotal             |
| `high_earner`          | 🔘   | ⚙️✅   | Top 25% salary flag          | 1 if >= 75th percentile              |

---

## 🧰 Technology Stack (Multi-Select Expanded)

| Column Pattern             | Type | Source | Description                | Handling / Notes             |
| -------------------------- | ---- | ------ | -------------------------- | ---------------------------- |
| `LanguageHaveWorkedWith_*` | 🔘   | 📦     | Programming languages used | Expanded from semicolon list |
| `LanguageWantToWorkWith_*` | 🔘   | 📦     | Languages desired          | Same expansion               |
| `DatabaseHaveWorkedWith_*` | 🔘   | 📦     | Databases used             | Expanded                     |
| `PlatformHaveWorkedWith_*` | 🔘   | 📦     | Cloud platforms used       | Expanded                     |
| `WebframeHaveWorkedWith_*` | 🔘   | 📦     | Web frameworks used        | Expanded                     |
| `AIBen_*`                  | 🔘   | 📦✅   | AI benefits perceived      | Expanded into binary flags   |
| `AIEthics_*`               | 🔘   | 📦✅   | AI ethical concerns        | Expanded into binary flags   |

---

## 🤖 AI Usage & Sentiment

| Column               | Type | Source | Description                      | Handling / Notes            |
| -------------------- | ---- | ------ | -------------------------------- | --------------------------- |
| `AISelect`           | 🔠   | 📦✅   | Currently using AI tools         | Standardized “Yes/No”       |
| `AISent`             | 🧮   | 📦✅   | Attitude toward AI               | Mapped to ordinal sentiment |
| `AIAcc`              | 🧮   | 📦✅   | Trust in AI accuracy             | 1–5 scale                   |
| `AIThreat`           | 🔠   | 📦✅   | Perceived AI job threat          | Cleaned values              |
| `AIChallenges_*`     | 🔘   | 📦✅   | AI adoption barriers             | Multi-select expansion      |
| `is_ai_user`         | 🔘   | ⚙️✅   | Whether respondent uses AI tools | 1 if AISelect == "Yes"      |
| `ai_sentiment_score` | 🔢   | ⚙️✅   | Numeric sentiment index          | +2 to -2 mapping            |

---

## 🧑‍💻 Job & Work Satisfaction

| Column                   | Type | Source | Description                           | Handling / Notes              |
| ------------------------ | ---- | ------ | ------------------------------------- | ----------------------------- |
| `JobSat`                 | 🔢   | 📦✅   | Overall satisfaction score            | Converted to numeric          |
| `JobSatPoints_*`         | 🔢   | 📦     | Point distribution across job aspects | Normalized                    |
| `job_satisfaction_score` | 🔢   | ⚙️✅   | Simplified satisfaction metric        | Direct conversion from JobSat |

---

## 💬 Stack Overflow Engagement

| Column                | Type | Source | Description                  | Handling / Notes    |
| --------------------- | ---- | ------ | ---------------------------- | ------------------- |
| `SOVisitFreq`         | 🧮   | 📦     | Visit frequency              | Ordinal scale       |
| `SOPartFreq`          | 🧮   | 📦✅   | Participation frequency      | Ordinal 0–5 scale   |
| `SOComm`              | 🔘   | 📦✅   | Community membership self-ID | Binary yes/no       |
| `so_engagement_score` | 🔢   | ⚙️✅   | Combined activity metric     | Based on SOPartFreq |

---

## 📘 Learning & Skill Development

| Column                | Type | Source | Description                   | Handling / Notes                      |
| --------------------- | ---- | ------ | ----------------------------- | ------------------------------------- |
| `LearnCode`           | 🔠   | 📦     | How learned to code           | Multi-select expanded                 |
| `LearnCodeOnline`     | 🔠   | 📦     | Online learning sources       | Multi-select expanded                 |
| `education_level`     | 🔠   | ⚙️✅   | Simplified education grouping | Basic, Undergraduate, Graduate, Other |
| `experience_category` | 🔠   | ⚙️✅   | Experience tier               | Junior / Mid / Senior / Expert        |

---

## 🌍 Regional Grouping

| Column      | Type | Source | Description                | Handling / Notes                           |
| ----------- | ---- | ------ | -------------------------- | ------------------------------------------ |
| `region`    | 🔠   | ⚙️✅   | Geographic region grouping | Derived from `Country` (continent mapping) |
| `is_remote` | 🔘   | ⚙️✅   | Remote worker flag         | 1 if RemoteWork includes “remote”          |

---

## ⚙️ Meta & Quality Checks

| Column                 | Type | Source | Description                  | Handling / Notes          |
| ---------------------- | ---- | ------ | ---------------------------- | ------------------------- |
| `attention_pass`       | 🔘   | ⚙️     | Passed attention check       | True if “Apples” selected |
| `data_version`         | 🔠   | ⚙️     | Processing version tag       | Added during export       |
| `processing_timestamp` | 🔠   | ⚙️     | Date/time of last processing | ISO timestamp             |

---

## 🧾 Notes

- Multi-select columns generate hundreds of binary flags; consider subsetting before modeling.
- Outlier handling only affects salary (`CompTotal_winsorized`).
- Missing values in experience or compensation are **not imputed** — analysts decide strategy.
- Currency normalization not performed (values remain self-reported).

---

**End of File**  
📦 `data/processed/cleaned_full.csv`  ✅ `data/processed/feature_engineered.csv`  
Maintained under `/docs/data_dictionary.md`
