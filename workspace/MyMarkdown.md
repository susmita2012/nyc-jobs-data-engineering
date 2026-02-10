
# NYC Jobs Data Engineering Coding Challenge

## Overview
This project implements an end-to-end **Data Engineering pipeline** using **PySpark** to analyze job postings published by the City of New York.  
The solution follows a structured approach aligned with real-world data engineering best practices and the expectations outlined in the challenge.

The pipeline covers:
- Data Exploration
- Data Processing & Feature Engineering
- KPI Analysis
- Production-oriented design considerations

---

## Dataset
- **Source**: NYC official jobs portal  
- **Format**: CSV  
- **File**: `nyc-jobs.csv`  
- **Content**: Internal and external job postings including salary ranges, agencies, job categories, and qualification details

---

## Project Structure

```
nyc_jobs_project/
│
├── data/
│   ├── nyc-jobs.csv
│   └── processed_nyc_jobs.parquet
│
├── notebooks/
│   ├── 01_exploration.ipynb
│   ├── 02_kpis.ipynb
│   └── 03_data_processing.ipynb
│
├── README.md
```

---

## 1. Data Exploration

### Objective
Understand the structure, quality, and analytical suitability of the raw dataset.

### Key Activities
- Schema inspection (numerical vs categorical vs text)
- Null value profiling
- Cardinality analysis of categorical columns
- Salary range sanity checks
- Identification of columns suitable for KPIs vs removal

### Key Findings
- Dataset is string-heavy (typical public CSV source)
- Salary fields require validation and normalization
- Job category is well-suited for aggregation
- Several free-text columns have high null density and limited analytical value

---

## 2. Data Processing & Feature Engineering

### Objectives
- Clean and standardize raw data
- Apply reusable transformations
- Engineer meaningful analytical features
- Remove unused or noisy columns
- Persist a curated dataset

### Processing Steps

#### Column Pre-processing
- Standardized column names to `snake_case`
- Removed spaces and improved readability

#### Data Cleaning & Wrangling
- Filtered records with missing salary ranges
- Validated salary consistency (`salary_range_from <= salary_range_to`)
- Normalized qualification text (lowercasing and trimming)

#### Feature Engineering (≥ 3)
1. **avg_salary**  
   - Average of salary range for consistent comparison

2. **posting_date & posting_year**  
   - Parsed date and extracted year for time-based analysis

3. **requires_degree**  
   - Boolean flag derived from qualification text using regex

#### Feature Removal
- Dropped free-text and high-null columns such as:
  - Job description
  - Additional information
  - Preferred skills (from curated layer)

#### Target File
- Stored processed dataset as:
  - **Format**: Parquet
  - **Path**: `data/processed_nyc_jobs.parquet`

---

## 3. KPI Analysis

KPIs are computed immediately after exploration with minimal, KPI-driven feature preparation.

### KPIs Implemented

1. **Top 10 Job Categories by Number of Postings**
2. **Salary Distribution per Job Category**
   - Min, Avg, Max salary
3. **Correlation Between Degree Requirement and Salary**
4. **Highest Paying Job per Agency**
5. **Average Salary per Agency (Last 2 Years)**
6. **Highest Paid Skills in the US Market**
   - Derived from qualification text

All KPIs use **Spark-native functions** and avoid UDFs for performance and scalability.

---

## 4. Code Quality & Best Practices

- Modular functions for processing logic
- Clear separation of concerns:
  - Exploration
  - Processing
  - Analytics
- Extensive inline code comments
- Spark SQL functions used instead of Python built-ins
- Defensive checks for derived columns

---

## 5. Testing Strategy

Suggested test cases:
- Schema validation after processing
- Salary calculation accuracy
- Null filtering logic
- Feature existence checks before KPI execution

These can be implemented using `pytest` with a local Spark session.

---

## 6. Deployment Proposal

### Suggested Architecture
- **Bronze**: Raw CSV ingestion
- **Silver**: Processed Parquet dataset
- **Gold**: KPI outputs / dashboards

### Deployment Options
- Databricks Jobs
- Apache Airflow DAG
- Azure Data Factory orchestration

---

## 7. Triggering Strategy

- Scheduled batch execution (daily / weekly)
- Event-based trigger on new CSV arrival
- Parameterized execution for reprocessing historical data

---

## Conclusion

This solution demonstrates a **production-aware data engineering approach**, balancing correctness, clarity, and scalability.  
All transformations and KPIs are intentionally designed to be explainable, testable, and aligned with enterprise data engineering standards.

---

## Author
Susmita Biswas  
(Data Engineer)
