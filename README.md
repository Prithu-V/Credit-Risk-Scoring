# Credit Risk Scoring & Loan Analytics

A simple end-to-end data analytics project that simulates a credit-risk database, builds an ETL pipeline using Python, loads the data into MySQL, and performs business analysis using SQL.

The project uses **Python, Pandas, NumPy, and Faker** to generate realistic synthetic banking data and **MySQL** for data storage and analysis.

> **Note:** This is a synthetic data project. No real customer or financial data is used.

---

## Project Overview

Financial institutions manage large amounts of data related to customers, loans, credit history, loan officers, and repayments.

This project simulates that environment by creating a small relational banking dataset and moving it through a basic ETL workflow:

```text
Python + Faker
      ↓
Synthetic Data Generation
      ↓
Pandas DataFrames
      ↓
ETL / Data Cleaning
      ↓
CSV / MySQL
      ↓
MySQL Workbench
      ↓
SQL Analysis
```

The goal was not to build a sophisticated machine-learning credit scoring system, but to practice the **data pipeline and analytical workflow** involved in a credit-risk domain.

---

## Tech Stack

* **Python** — Data generation and ETL
* **Pandas** — Data manipulation
* **NumPy** — Numerical calculations and probability-based generation
* **Faker** — Synthetic customer and employee data
* **MySQL** — Data storage and analysis
* **MySQL Workbench** — SQL development and database management

---

## Dataset

The dataset is generated entirely using Python.

The project contains five main entities:

### 1. Loan Officers

Information about employees responsible for handling loan applications.

Example attributes:

* Officer ID
* Name
* Branch
* Region
* Joining date

### 2. Applicants

Synthetic customer information including:

* Applicant ID
* Age
* Gender
* City
* State
* Employment type
* Annual income
* Education level
* Years employed
* Marital status

The generated dataset contains **2,000 applicants**.

### 3. Credit Bureau

Credit-history information associated with each applicant:

* Credit score
* Existing loan count
* Existing debt
* Missed payments
* Bankruptcies
* Bureau pull date

The credit score is generated using income and credit-history variables rather than being completely random.

### 4. Loans

The loan table contains:

* Loan ID
* Applicant ID
* Officer ID
* Loan type
* Loan amount
* Interest rate
* Tenure
* Application date
* Approval date
* Loan status
* Default flag

The project generates **2,500 loans**, allowing some applicants to have multiple loans.

Loan types include:

* Personal
* Home
* Auto
* Education

### 5. Repayments

Repayment-level data is generated for loans that were not denied.

It contains:

* Repayment ID
* Loan ID
* Due date
* Paid date
* Amount due
* Amount paid
* Payment status

Payment statuses include:

* Paid
* Late
* Missed

---

## Synthetic Data Generation

The data is intentionally generated with basic relationships that resemble real-world credit data.

For example:

* Higher income can contribute to a higher credit score.
* Missed payments reduce the generated credit score.
* Bankruptcies significantly reduce the credit score.
* Lower credit scores result in higher interest rates.
* Higher debt-to-income ratios increase default probability.
* Large loans relative to income increase default probability.
* Longer loan tenures increase exposure to default.

This makes the dataset more useful for analysis than completely random data.

---

## ETL Pipeline

A basic Python ETL process was created to move the generated data into the database.

### Extract

Synthetic records are generated using Faker, NumPy, Pandas, and Python's random module.

### Transform

The generated data is transformed into separate relational datasets for:

```text
Loan Officers
Applicants
Credit Bureau
Loans
Repayments
```

IDs and relationships between the tables are created during this process.

### Load

The processed datasets are exported and loaded into **MySQL**, where the relational data can be queried using SQL.

---

## Database Relationships

The main relationships can be represented as:

```text
                 ┌─────────────────┐
                 │ Loan Officers   │
                 │                 │
                 │ officer_id      │
                 └────────┬────────┘
                          │
                          │
                          ▼
┌──────────────┐    ┌──────────────┐
│  Applicants  │───▶│    Loans     │
│              │    │              │
│ applicant_id │    │ loan_id      │
└──────┬───────┘    └──────┬───────┘
       │                   │
       ▼                   ▼
┌──────────────┐    ┌──────────────┐
│ Credit       │    │ Repayments   │
│ Bureau       │    │              │
│              │    │ repayment_id │
└──────────────┘    └──────────────┘
```

---

## SQL Analysis

After loading the data into MySQL, SQL was used to analyze the generated banking data.

The analysis focuses on questions such as:

* What is the overall loan default rate?
* Which loan types have the highest default rates?
* How does credit score relate to loan defaults?
* How does employment type relate to default risk?
* Which regions generate the most loans?
* What is the average loan amount by loan type?
* How does income compare with loan amounts?
* Which borrowers have high existing debt?
* What is the repayment performance of different loan categories?
* Which loan officers handle the most applications?
* What factors appear to be associated with loan defaults?

---

## Example Analytical Workflow

A typical analysis might join applicant, bureau, and loan information:

```sql
SELECT
    l.loan_type,
    COUNT(*) AS total_loans,
    SUM(l.default_flag) AS defaults,
    ROUND(
        SUM(l.default_flag) * 100.0 / COUNT(*),
        2
    ) AS default_rate
FROM loans l
GROUP BY l.loan_type
ORDER BY default_rate DESC;
```

This allows the business to compare default rates across different loan products.

---

## Project Structure

```text
Credit-Risk-Scoring/
│
├── data/
│   └── generated datasets
│
├── python/
│   ├── data_generation.py
│   └── etl.py
│
├── sql/
│   └── analysis queries
│
├── mysql/
│   └── database schema / SQL scripts
│
└── README.md
```

*The structure above should be adjusted if the repository uses different folder/file names.*

---

## What I Practiced

This project was primarily built to practice the fundamentals of a real-world data workflow.

### Python

* Pandas DataFrames
* NumPy
* Faker
* Random data generation
* Functions
* Data transformation
* Relational dataset generation

### ETL

* Extracting generated data
* Transforming datasets
* Creating relationships between entities
* Preparing data for database loading

### SQL

* SELECT and filtering
* GROUP BY
* Aggregations
* JOINs
* Subqueries
* CASE statements
* Date analysis
* Business metrics

### Database

* Relational data modelling
* Primary and foreign keys
* Loading datasets into MySQL
* Querying multiple related tables

---

## Important Limitation

This project uses **synthetically generated data**.

Although the data generation process introduces relationships between variables such as income, credit score, debt, missed payments, and default probability, these relationships are defined by the simulation logic rather than learned from real banking data.

Therefore, the results should **not** be interpreted as actual evidence about credit-risk behaviour in the real world.

The project is intended to demonstrate:

> **Data generation → ETL → Database → SQL analysis**

rather than a production-grade credit scoring system.

---

## Future Improvements

Possible improvements include:

* Automating the Python-to-MySQL loading process
* Adding data validation checks to the ETL pipeline
* Creating a proper database schema script
* Adding more complex SQL analysis
* Building a Power BI dashboard on top of the MySQL database
* Adding data-quality monitoring
* Scheduling the ETL pipeline
* Introducing a real credit-risk dataset for comparison
* Building a machine-learning model as a separate layer

---

## Author

**Prithu V**

GitHub: [Prithu-V](https://github.com/Prithu-V)
