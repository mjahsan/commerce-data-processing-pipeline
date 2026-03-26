# 🚀 COMMERCE DATA PROCESSING PIPELINE (S3 → LAMBDA → GLUE → S3)

---

## 📌 OVERVIEW
This project implements an event-driven data pipeline on AWS that ingests raw JSON data, validates files, performs transformations using Spark SQL, and produces analytics-ready Parquet output using a Bronze–Silver–Gold lakehouse pattern.
The pipeline simulates a real-world commerce data processing workflow using fully managed AWS services and production-oriented design practices.

---

## 🏗️ ARCHITECTURE:

### 🔄 Flow:
- Raw JSON files are uploaded to the Bronze layer in Amazon S3
- An S3 event triggers an AWS Lambda function
- Lambda performs file-level validation (format & naming)
- Valid files are promoted to the Silver layer
- Once all required datasets are present, Lambda triggers an AWS Glue Spark job
- Glue performs transformations using Spark SQL
- Final aggregated output is written to the Gold layer in Parquet format


![commerce-data-processing-pipeline](./architecture/pipeline_architecture.png)

---

## 📊 DATA MODELS:

### Input Datasets:
- users.json
- events.json
- orders.json

Each dataset contains ~1 million records, generated using Python to test scalability and performance.

---

## 🔄 TRANSFORMATIONS:

### The AWS Lambda job:
- Validates the incoming files from bronze/raw layer for file types and empty files
- Handles the empty and non-relevant files by skipping them
- Transfers the approved files to the silver layer
- Triggers the downstream Glue job when all the files have been transferred to the silver layer
  
All transformations are implemented using Python

### The AWS Glue job:
- Handles null values
- Joins users, events, and orders datasets
- Aggregates user-level activity metrics
- Produces a single user_activity dataset
- Writes output as Parquet to the Gold layer
  
All transformations are implemented using Spark SQL for clarity, performance, and maintainability.

---

## 📁 REPOSITORY STRUCTURE:

```text
commerce-data-pipeline/
│
├── README.md
│
├── architecture/
│   └── pipeline_architecture.png
│
├── data/
│   └── sample_data/
│       ├── users.json
│       ├── events.json
│       └── orders.json
│
├── lambda/
│   └── bronze_to_silver/
│       └── commerce_data_validation.py
│       
│
├── glue/
│   └── silver_to_gold/
│       └── user_activity_job.py
│
├── iam/
│   ├── lambda_execution_policy.json
│   └── glue_execution_policy.json
│
├── kms/
│   └── key_policy.json
│
├── scripts/
│   └── generate_sample_data.py
│
├── config/
│   └── pipeline_config.yaml
│
└── .gitignore
```
---

## 🔐 SECURITY:

- IAM execution roles created for Lambda and Glue
- Least-privilege policies defined in iam/
- S3 data encrypted using AWS KMS
- KMS key policies included for reference
- No credentials stored in the repository
  
This project follows security best practices aligned with production-grade AWS pipelines.

---

## ▶️ HOW TO RUN (HIGH LEVEL):

- Deploy IAM roles and policies
- Create an S3 bucket with Bronze / Silver / Gold structure
- Create a Lambda function for Bronze → Silver validation with an S3 upload trigger
- Upload sample data to the Bronze layer
- Lambda automatically validates and promotes data
- Glue job is triggered automatically
- Final Parquet output is available in the Gold layer

---

## 🛠️ TECH STACK:

- Amazon S3
- AWS Lambda (Python)
- AWS Glue (PySpark, Spark SQL)
- IAM
- AWS KMS
- AWS CloudWatch

---

## 🎯 WHAT THIS PROJECT DEMONSTRATES:

- Event-driven data ingestion
- Lakehouse design principles (Bronze / Silver / Gold)
- Spark-based data transformations
- Cloud security fundamentals
- Production-oriented pipeline structuring
