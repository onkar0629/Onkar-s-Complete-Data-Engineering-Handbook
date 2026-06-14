# 🛒 E-Commerce Real-Time Data Pipeline & Analytics Platform

[![Python](https://img.shields.io/badge/Python-3.9%2B-blue)](https://www.python.org/)
[![Apache Spark](https://img.shields.io/badge/Apache_Spark-PySpark-orange)](https://spark.apache.org/)
[![dbt](https://img.shields.io/badge/dbt-Data_Build_Tool-FF69B4)](https://www.getdbt.com/)
[![Apache Airflow](https://img.shields.io/badge/Apache_Airflow-Orchestration-017CEE)](https://airflow.apache.org/)
[![Snowflake](https://img.shields.io/badge/Snowflake-Cloud_Data_Warehouse-29B5E8)](https://www.snowflake.com/)

## 📝 Project Overview
This project is an end-to-end, fully automated data engineering pipeline designed to handle real-time e-commerce transaction data. It simulates streaming sales events, processes the data through a **Medallion Architecture (Bronze, Silver, Gold)**, ensures data quality via automated testing, and serves optimized datasets for BI dashboards.

**Business Objective:** To provide a scalable architecture that delivers sub-hour latency on e-commerce metrics (revenue, active users, inventory) to business stakeholders without compromising data integrity.

## 🏗️ Architecture & Data Flow

![Architecture Diagram](link-to-your-architecture-image.png)
*(Note: Use a free tool like draw.io or Excalidraw to export an image of your architecture and link it here.)*

1. **Ingestion (Streaming):** A custom Python generator streams mock e-commerce transactions into **[Apache Kafka / AWS Kinesis]**.
2. **Bronze Layer (Raw):** Raw JSON events are dumped directly into **[AWS S3 / Azure Data Lake]** for an immutable historical record.
3. **Silver Layer (Cleaned):** **Apache Spark (PySpark)** reads the raw data, enforces schemas, handles nulls/duplicates, and writes back to storage in optimized **Parquet** format.
4. **Gold Layer (Analytics):** Data is loaded into **[Snowflake / BigQuery]**. **dbt (data build tool)** is used to transform the flat data into a Star Schema (Fact and Dimension tables).
5. **Orchestration:** **Apache Airflow** schedules and monitors the batch processing and dbt models.
6. **Visualization:** A **[Power BI / Tableau]** dashboard connects via DirectQuery to the Gold layer for real-time reporting.

## 🛠️ Technology Stack
* **Language:** Python, SQL
* **Streaming/Ingestion:** Apache Kafka / Faker Library
* **Storage:** AWS S3 (or Azure ADLS Gen2)
* **Processing:** Apache Spark (PySpark)
* **Transformation & Testing:** dbt (Data Build Tool)
* **Data Warehouse:** Snowflake (or Google BigQuery)
* **Orchestration:** Apache Airflow
* **Containerization:** Docker & Docker Compose
* **Visualization:** Power BI / Tableau

## 📂 Repository Structure

```text
├── dags/                   # Airflow Directed Acyclic Graphs (DAGs)
├── data_generator/         # Python scripts utilizing Faker for stream generation
├── dbt_ecommerce/          # dbt models (staging, core, marts) and tests
├── spark_jobs/             # PySpark scripts for Bronze-to-Silver transformations
├── docker-compose.yml      # Local infrastructure setup (Kafka, Airflow, Postgres)
├── requirements.txt        # Python dependencies
└── README.md
