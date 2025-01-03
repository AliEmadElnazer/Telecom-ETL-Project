
# ETL Project for Telecom 

This project involves designing and implementing an **ETL (Extract, Transform, Load)** process for a telecom company using **SQL Server Integration Services (SSIS)**. The goal is to process customer event data and store it in a structured format for analysis and reporting. Below is an overview of the project's components, data transformation rules, and implementation steps.

---

## Project Overview

The telecom company maintains event data in `.csv` files, updated every 5 minutes. This project automates the process of extracting, transforming, and loading this data into a SQL database while adhering to specific business rules.

---

## Data Schema

### Source File Example
| Column Name  | Data Type | Length | Nullable | Sample Data          |
|--------------|-----------|--------|----------|----------------------|
| ID           | Int       | -      | False    | 156                  |
| IMSI         | String    | 9      | False    | 310120265            |
| IMEI         | String    | 14     | True     | 490154203237518      |
| CELL         | Int       | -      | False    | 123                  |
| LAC          | Int       | -      | False    | 12                   |
| EVENT_TYPE   | String    | 1      | True     | 1                    |
| EVENT_TS     | Timestamp | -      | False    | 16/1/2020 7:45:43    |

### Data Transformation Rules
| Column Name  | Transformation Rules                                           | Target Column    |
|--------------|----------------------------------------------------------------|------------------|
| ID           | As-is                                                          | Transaction_id   |
| IMSI         | As-is; reject record if null                                   | IMSI             |
| IMSI         | Join with reference data to get `subscriber_id`; set -99999 if null | subscriber_id |
| IMEI         | Extract first 8 characters; set -99999 if null or < 15 chars   | TAC              |
| IMEI         | Extract last 6 characters; set -99999 if null or < 15 chars    | SNR              |
| IMEI         | As-is                                                          | IMEI             |
| CELL         | As-is; reject record if null                                   | CELL             |
| LAC          | As-is; reject record if null                                   | LAC              |
| EVENT_TYPE   | As-is                                                          | EVENT_TYPE       |
| EVENT_TS     | Validate format `YYYY-MM-DD HH:MM:SS`; reject record if null   | EVENT_TS         |

---

## Rejected Records
- Rejected records are logged into a separate table for review.
- A `.csv` file is also maintained to document rejected data.

---

## Quality Assurance
The process includes verification steps to ensure data quality:
1. Count of records in the source `.csv` file.
2. Count of records successfully stored in the database.
3. Count of rejected records that did not meet validation criteria.
4. Cross-validation of database records with the source `.csv` file.

---

## Implementation Steps
1. Extract data from `.csv` files.
2. Apply transformation rules.
3. Load validated data into the SQL database.
4. Log rejected records in a separate table and `.csv` file.
5. Verify the data storage process.



Feel free to provide feedback or ask questions about this project!
