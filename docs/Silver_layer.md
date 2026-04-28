# Day 2 Lecture: Silver Layer – Cleaning Amazon Reviews Data (Scala + PySpark)

## Objective
The goal of Day 2 is to transform raw (Bronze layer) Amazon reviews data into a clean, structured, and analysis-ready dataset (Silver layer). This involves handling inconsistent formats, missing values, and irrelevant columns while preparing the data for downstream analytics and machine learning.

---

## 1. Recap from Day 1 (Bronze Layer)

In Day 1, we:
- Loaded raw CSV data into Spark DataFrames
- Explored schema and sample records
- Identified inconsistencies such as:
  - Ratings stored as text (e.g., "1 out of 5")
  - Review counts stored as text (e.g., "5 reviews")
  - Unclean text fields
  - Presence of unnecessary columns

The Bronze layer represents raw, unprocessed data.

---

## 2. What is the Silver Layer?

The Silver layer is the stage where:
- Data is cleaned and standardized
- Data types are corrected
- Irrelevant columns are removed
- New useful features are created

This layer ensures that:
- Data is reliable
- Data is consistent
- Data is ready for analysis and modeling

---

## 3. Key Problems in the Dataset

Typical issues in the dataset include:

| Issue | Example |
|------|--------|
| Non-numeric ratings | "1 out of 5" |
| Non-numeric review counts | "5 reviews" |
| Text inconsistency | " Great Product " |
| Missing values | null |
| Redundant columns | profile links, names |

---

## 4. Step 1: Extract Numeric Values

### Problem
Columns like `rating` and `review_count` are stored as strings instead of numbers.

### Solution
Use regular expressions to extract numeric values.

### Why Regex?
Regex allows pattern matching inside strings. The pattern `\d+` matches one or more digits.

---

### Scala Implementation

```scala
.withColumn("rating_clean",
  regexp_extract($"rating", "\\d+", 0).cast("int")
)
.withColumn("review_count_clean",
  regexp_extract($"review_count", "\\d+", 0).cast("int")
)
