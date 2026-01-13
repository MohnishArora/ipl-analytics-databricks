# 🏏 IPL Data Pipeline: End-to-End Lakehouse Engineering

**Tech Stack:** Databricks (Unity Catalog), PySpark, AWS S3 (IAM Roles), Spark SQL.

### 🚀 Project Overview
This project implements a scalable **Data Lakehouse** architecture to process Indian Premier League (IPL) match data (2008–2017). It demonstrates a production-grade ingestion pipeline that enforces schema validation, handles complex transformations (window functions), and exposes clean data layers for SQL analytics.

### 🏗 Architecture & Engineering Highlights
* **Secure Ingestion (AWS IAM + Unity Catalog):**
    * Configured **AWS IAM Roles** with custom trust policies to allow secure, temporary credential access from Databricks to private S3 buckets.
    * Implemented **Unity Catalog** External Locations to govern data access, replacing legacy access key methods.
* **Schema Enforcement (Data Quality):**
    * Rejected `inferSchema` in favor of **Explicit StructType/StructField definitions** for all 5 datasets (Match, Player, Team, etc.) to prevent data type drift and ensure production reliability.
    * Handled complex data types (Decimal, DateType) and nullability constraints at the ingestion layer.
* **Transformation Logic (PySpark):**
    * **Window Functions:** Calculated "Running Total Runs" per match/inning using `Window.partitionBy`.
    * **Conditional Logic:** Flagged "High Impact Balls" (Wickets or >6 runs) using `when/col` logic for downstream analysis.
    * **Data Cleaning:** Normalized player names using regex (`regexp_replace`) and handled missing values (`na.fill`) for batting/bowling skills.
* **Serving Layer (Spark SQL):**
    * Created `TempViews` to enable low-latency SQL querying for analysts.
    * Optimized queries for "Economical Bowlers" and "Top Scorers" using aggregated GROUP BY and HAVING clauses.

### 📂 Dataset Volume
* **Source:** 5 Relational CSVs (Ball-by-Ball, Match, Player, Player_Match, Team).
* **Scale:** Processed complete historical match data involving complex joins across 5 tables to derive "Player Performance" metrics.

### 📊 Key Insights Derived
* **Powerplay Efficiency:** Identified top 10 most economical bowlers in powerplay overs (overs 1–6) with a minimum sample size of 120 balls.
* **Veteran Impact:** Classified players as "Veterans" (Age > 35) to analyze longevity and performance trends.
