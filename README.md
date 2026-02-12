# Amazon Sales - Row-Level Validation with Pydantic v2

This project demonstrates a sophisticated data validation pipeline for processing 128k+ e-commerce records. It showcases the transition from **Strict Validation** (Zero Tolerance) to **Flexible Validation** (Production Ready), ensuring data integrity without disrupting pipeline flow.

## 🎯 Project Overview

In real-world data engineering, datasets are often "messy." This project uses **Pydantic v2** to inspect every row of the Amazon Sales Report, segregating valid data and quarantining errors for manual review.

## ⚖️ Comparative Analysis: Strict vs. Flexible

I implemented two different validation strategies to demonstrate how to handle real-world data inconsistencies.

### 1. Strict Validation (The "Zero Tolerance" Approach)

* **Result:** High failure rate (Most rows marked as Invalid).
* **Observation:** The model rejected rows due to missing values in optional fields (like `currency` or `amount`) and minor date format variations.
* **Use Case:** Ideal for critical financial data where even a single missing byte is unacceptable.

### 2. Flexible Validation (The "Production-Ready" Approach)

* **Result:** 100% Success / High Validity.
* **Optimization:** * Used `Optional` fields and `ConfigDict(extra='ignore')`.
* Implemented `field_validator` with `pd.to_datetime` for smart date parsing.
* Applied default values for missing numerical fields.


* **Use Case:** Best for large-scale ETL pipelines where continuity is as important as quality.

| Strategy | Validation Screenshot | Result |
| --- | --- | --- |
| **Strict** | <img width="692" height="240" alt="Screenshot 2026-02-12 172201" src="https://github.com/user-attachments/assets/d563edc8-1e43-4a26-82c3-fa0226519041" />| ❌ High failure due to Nulls/Formats |
| **Flexible** | <img width="697" height="78" alt="Screenshot 2026-02-12 174811" src="https://github.com/user-attachments/assets/8fa320eb-ef8b-45ef-9ffc-e8d7297b8901" />| ✅ Success through Data Normalization |

## 🛠️ Tech Stack

* **Pydantic v2:** Schema enforcement and type safety.
* **Pandas:** Efficient processing of 128k+ CSV records.
* **Slack API:** Real-time incident reporting via Webhooks.

## 🚀 Key Features

* **Data Quarantine:** Invalid records are automatically diverted to `invalid_rows.csv` with specific error reasons (`loc`, `msg`, `type`).
* **Header Normalization:** Automatically converts messy CSV headers (e.g., `ship-country`) to clean Python attributes (`ship_country`).
* **Real-time Alerts:** Instant Slack notifications with failure counts.

## 📋 Data Schema

The validation checks for:

* `order_id`: Non-empty string.
* `qty`: Non-negative integer.
* `amount`: Non-negative float (auto-handles `NaN`).
* `date`: Standardized `datetime` object.

## 🚨 Example Slack Alert

> ✅ **Pydantic Validation Complete:**
> All 128,975 rows processed successfully!
> *(Or: Found X invalid rows. Check `invalid_rows.csv`)*
