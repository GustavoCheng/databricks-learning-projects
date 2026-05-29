# BCB Economic Indicators Pipeline with Databricks

This project is part of my Databricks learning portfolio.  
The goal is to build a basic data engineering pipeline using real data from the Brazilian Central Bank API.

## Project Objective

The objective of this project is to ingest, transform, and analyze daily Selic interest rate data using Databricks.

This project was built to practice core Databricks concepts such as API ingestion, PySpark transformations, Delta Tables, SQL analysis, and GitHub version control.

The pipeline follows a simple medallion architecture:

```text
API Source
   ↓
Raw Table
   ↓
Silver Table
   ↓
Gold Table
   ↓
SQL Analysis
```

## Data Source

The data is collected from the Brazilian Central Bank public API.

The project uses the SGS time series:

```text
Series code: 11
Series name: Daily Selic interest rate
```

The API returns the data in JSON format, which is then transformed and stored as Delta tables in Databricks.

## Technologies Used

- Databricks
- Python
- PySpark
- SQL
- Delta Tables
- Brazilian Central Bank API
- GitHub

## Project Structure

```text
databricks-bcb-economic-indicators/
├── notebooks/
│   ├── 01_ingest_bcb_api.ipynb
│   ├── 02_transform_selic_data.ipynb
│   ├── 03_create_monthly_selic_gold.ipynb
│   └── 04_sql_analysis.ipynb
└── README.md
```

## Pipeline Steps

### 1. API Ingestion

The notebook `01_ingest_bcb_api` connects to the Brazilian Central Bank API, retrieves daily Selic data in JSON format, converts it into a Spark DataFrame, and saves it as a Delta table.

Created table:

```text
portfolio.bcb_economic_indicators.raw_selic_daily
```

This table represents the raw ingestion layer of the pipeline.

### 2. Data Transformation

The notebook `02_transform_selic_data` reads the raw table and applies basic transformations, including:

- Renaming columns to English
- Converting date fields
- Creating year, month, and day columns
- Rounding numeric values
- Keeping ingestion metadata

Created table:

```text
portfolio.bcb_economic_indicators.silver_selic_daily
```

This table represents the curated silver layer of the pipeline.

### 3. Gold Analytical Table

The notebook `03_create_monthly_selic_gold` aggregates the daily Selic data into a monthly analytical table.

The table includes:

- Total records by month
- Average Selic rate
- Minimum Selic rate
- Maximum Selic rate

Created table:

```text
portfolio.bcb_economic_indicators.gold_monthly_selic
```

This table represents the gold layer, ready for analytics and dashboarding.

### 4. SQL Analysis

The notebook `04_sql_analysis` contains SQL queries to analyze the monthly Selic data.

The analysis includes:

- Monthly Selic evolution
- Annual average Selic rate
- Highest monthly Selic rates
- Lowest monthly Selic rates

## Databricks Tables

The following Delta tables were created:

```text
portfolio.bcb_economic_indicators.raw_selic_daily
portfolio.bcb_economic_indicators.silver_selic_daily
portfolio.bcb_economic_indicators.gold_monthly_selic
```

## Example SQL Queries

Monthly Selic evolution:

```sql
SELECT *
FROM portfolio.bcb_economic_indicators.gold_monthly_selic
ORDER BY year, month;
```

Annual average Selic rate:

```sql
SELECT
  year,
  ROUND(AVG(avg_selic_rate), 4) AS annual_avg_selic_rate,
  MIN(min_selic_rate) AS annual_min_selic_rate,
  MAX(max_selic_rate) AS annual_max_selic_rate
FROM portfolio.bcb_economic_indicators.gold_monthly_selic
GROUP BY year
ORDER BY year;
```

Highest monthly Selic rates:

```sql
SELECT
  year,
  month,
  avg_selic_rate
FROM portfolio.bcb_economic_indicators.gold_monthly_selic
ORDER BY avg_selic_rate DESC
LIMIT 5;
```

## Key Learnings

This project helped me practice:

- Consuming a public REST API with Python
- Transforming JSON data into structured tables
- Working with PySpark DataFrames
- Creating Delta tables in Databricks
- Organizing data using raw, silver, and gold layers
- Running SQL analysis on curated data
- Structuring a basic data engineering project
- Versioning a Databricks project with GitHub

## Conclusion

This project demonstrates a basic data engineering workflow in Databricks using real public data from an API.

It is a simple but practical foundation for learning data ingestion, transformation, Delta Tables, SQL analytics, and project versioning with GitHub.
