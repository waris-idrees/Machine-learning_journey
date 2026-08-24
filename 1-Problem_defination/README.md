# Data Cleaning & Data Quality: A Real-World Guide

> **A practical guide to understanding, investigating, cleaning, validating, and documenting raw data — before using it for analysis, reporting, dashboards, or machine learning.**

---

## Table of Contents

1. [What Data Cleaning Actually Means](#1-what-data-cleaning-actually-means)
2. [The Professional Mindset](#2-the-professional-mindset)
3. [The Real-World Data Quality Workflow](#3-the-real-world-data-quality-workflow)
4. [Step 1 — Understand the Dataset](#4-step-1--understand-the-dataset)
5. [Step 2 — Inspect the Dataset](#5-step-2--inspect-the-dataset)
6. [Step 3 — Understand Rows and Columns](#6-step-3--understand-rows-and-columns)
7. [Step 4 — Detect Data Quality Problems](#7-step-4--detect-data-quality-problems)
8. [Step 5 — Investigate Before Changing Anything](#8-step-5--investigate-before-changing-anything)
9. [Step 6 — Find the Root Cause](#9-step-6--find-the-root-cause)
10. [Step 7 — Decide What Action to Take](#10-step-7--decide-what-action-to-take)
11. [Step 8 — Clean the Data](#11-step-8--clean-the-data)
12. [Step 9 — Validate the Cleaned Data](#12-step-9--validate-the-cleaned-data)
13. [Step 10 — Document Everything](#13-step-10--document-everything)
14. [Industry Examples](#14-industry-examples)
15. [Data Problem vs Data-Quality Problem vs Business-Rule Problem vs Source-System Problem](#15-data-problem-vs-data-quality-problem-vs-business-rule-problem-vs-source-system-problem)
16. [Professional Data Cleaning Checklist](#16-professional-data-cleaning-checklist)
17. [Final Principles](#17-final-principles)

---

# 1. What Data Cleaning Actually Means

Data cleaning is **not simply removing missing values, duplicates, and outliers**.

In a real-world project, data cleaning means:

> **Understanding what the data is supposed to represent, identifying where the actual data violates that meaning, investigating why the problem exists, deciding what should happen to the affected records, applying controlled changes, and proving that the resulting data is fit for its intended use.**

A raw dataset may contain:

* Missing information
* Incorrect formats
* Invalid values
* Unexpected categories
* Typographical errors
* Duplicate records
* Wrong data types
* Impossible dates
* Impossible numerical values
* Conflicting columns
* Broken relationships
* Business-rule violations
* System-generated errors
* Legitimate unusual observations

The important point is:

> **Not everything unusual is wrong.**

For example:

```text
Customer age = 95
```

This looks unusual.

But it is not automatically invalid.

A 95-year-old customer can exist.

Compare that with:

```text
Customer age = 250
```

That is much more suspicious.

The correct professional approach is not:

```text
"If it looks unusual → delete it."
```

Instead:

```text
"If it looks unusual → investigate why."
```

---

# 2. The Professional Mindset

A junior approach often looks like:

```text
Load data
↓
Find NaN
↓
Remove duplicates
↓
Remove outliers
↓
Convert data types
↓
Done
```

A senior approach looks more like:

```text
What does this dataset represent?
        ↓
What does one row represent?
        ↓
What does every column mean?
        ↓
What should valid data look like?
        ↓
What problems actually exist?
        ↓
Why do these problems exist?
        ↓
Are unusual values actually wrong?
        ↓
What is the safest action?
        ↓
How will I prove the cleaned data is correct?
```

The difference is **reasoning**.

A good data professional does not blindly modify data.

They build an understanding of the data first.

---

# 3. The Real-World Data Quality Workflow

A useful general workflow is:

```text
Understand
    ↓
Inspect
    ↓
Detect
    ↓
Investigate
    ↓
Find Root Cause
    ↓
Decide
    ↓
Clean
    ↓
Validate
    ↓
Document
```

## What each stage means

| Stage           | Main Question                                                    |
| --------------- | ---------------------------------------------------------------- |
| Understand      | What is this dataset supposed to represent?                      |
| Inspect         | What does the data actually look like?                           |
| Detect          | What appears suspicious or incorrect?                            |
| Investigate     | Is the suspicious data really a problem?                         |
| Find Root Cause | Why did the problem happen?                                      |
| Decide          | What should be done about it?                                    |
| Clean           | Apply the approved transformation                                |
| Validate        | Did the cleaning improve the data without creating new problems? |
| Document        | Can another person understand what was changed and why?          |

This workflow applies to:

* Data analysis
* Business intelligence
* Data engineering
* Reporting
* Data warehouses
* Customer databases
* Financial systems
* Healthcare systems
* Machine learning
* ETL pipelines
* APIs
* CSV/Excel files
* Database migrations

---

# 4. Step 1 — Understand the Dataset

Before writing cleaning code, understand the dataset.

This is one of the most important steps.

## Ask these questions

### 1. Where did the data come from?

Examples:

* Database
* Excel file
* CSV export
* API
* Website
* CRM
* ERP
* Survey
* Hospital system
* Banking system
* School management system

The source gives you clues about possible problems.

---

### 2. Why was the data collected?

For example:

An e-commerce dataset may exist to analyze:

```text
Customers → Orders → Products → Revenue
```

A healthcare dataset may exist to analyze:

```text
Patients → Visits → Diagnoses → Treatments
```

A banking dataset may represent:

```text
Customers → Accounts → Transactions → Loans
```

Understanding the purpose helps determine what "correct" means.

---

### 3. What does one row represent?

This is critical.

A row could represent:

```text
One customer
```

or:

```text
One order
```

or:

```text
One transaction
```

or:

```text
One hospital visit
```

or:

```text
One employee
```

or:

```text
One student
```

Never assume.

A dataset can even contain multiple levels of information.

For example:

```text
customer_id = 101
order_id = 5001
```

One row may represent **one order**, not one customer.

Therefore customer `101` appearing multiple times may be perfectly valid.

---

# 5. Step 2 — Inspect the Dataset

Start with basic inspection.

```python
import pandas as pd

df = pd.read_csv("data.csv")

print(df.shape)
print(df.head())
print(df.tail())
print(df.info())
print(df.columns)
```

### Why?

You want to understand:

* Number of rows
* Number of columns
* Column names
* Data types
* Sample values
* Potential missing values
* Unexpected structures

---

## Statistical overview

```python
df.describe(include="all")
```

For numerical columns:

```python
df.describe()
```

This can reveal:

* Minimum
* Maximum
* Mean
* Standard deviation
* Percentiles

But remember:

> Statistics are clues, not proof of errors.

---

# 6. Step 3 — Understand Rows and Columns

Every column should have a business meaning.

Imagine:

```text
customer_id
name
age
city
signup_date
monthly_spending
```

You should be able to explain:

| Column           | Meaning                    | Expected Type   | Example    |
| ---------------- | -------------------------- | --------------- | ---------- |
| customer_id      | Unique customer identifier | Integer/String  | C1023      |
| name             | Customer name              | String          | Ali Khan   |
| age              | Customer age               | Integer         | 25         |
| city             | Customer's city            | Category/String | Lahore     |
| signup_date      | Registration date          | Date            | 2026-05-12 |
| monthly_spending | Monthly spending amount    | Numeric         | 45000      |

If you cannot explain a column, **do not clean it yet**.

First find its definition.

Useful sources include:

* Data dictionary
* Database schema
* Documentation
* API documentation
* Data owner
* Subject-matter expert
* Source-system documentation

---

# 7. Step 4 — Detect Data Quality Problems

There are many types of data-quality problems.

Do not limit your inspection to missing values and duplicates.

---

## 7.1 Missing Values

```python
df.isna().sum()
```

Percentage:

```python
df.isna().mean() * 100
```

But ask:

> **Should this field actually be populated?**

Example:

```text
middle_name = NULL
```

This may be perfectly valid.

But:

```text
transaction_amount = NULL
```

could be much more serious.

### Important

Missing does not automatically mean wrong.

Possible reasons:

* Not collected
* Not applicable
* User skipped the field
* System failure
* Data lost during migration
* Value intentionally unavailable

---

## 7.2 Empty Strings and Special Missing Values

Missing information is not always stored as `NaN`.

You may encounter:

```text
""
" "
"NA"
"N/A"
"null"
"None"
"-"
"Unknown"
"?"
```

Inspect:

```python
for col in df.select_dtypes(include="object"):
    print(col, df[col].value_counts(dropna=False).head(10))
```

Before replacing these values, determine whether each one really means "missing".

For example:

```text
"Unknown"
```

may represent an actual category rather than missing data.

---

# 7.3 Duplicate Records

Basic duplicate detection:

```python
df.duplicated().sum()
```

View them:

```python
df[df.duplicated(keep=False)]
```

But there is an important distinction:

> **Duplicate rows are not always duplicate entities.**

Suppose:

```text
customer_id
101
101
101
```

This does not necessarily mean duplicates.

The customer may have placed three orders.

Therefore, first understand the row's grain.

---

## Duplicate business keys

For example:

```python
df["customer_id"].duplicated().sum()
```

If `customer_id` is supposed to be unique, duplicates may indicate a problem.

If the dataset represents orders, repeated customer IDs may be completely valid.

---

# 7.4 Incorrect Data Types

Inspect:

```python
df.dtypes
```

Example:

```text
age
"21"
"25"
"30"
```

A numeric column stored as text may cause problems.

Convert carefully:

```python
df["age"] = pd.to_numeric(df["age"], errors="coerce")
```

Dates:

```python
df["signup_date"] = pd.to_datetime(
    df["signup_date"],
    errors="coerce"
)
```

But do not convert blindly.

A field such as:

```text
customer_id = "001234"
```

may look numeric but should remain a string because leading zeros are meaningful.

---

# 7.5 Invalid Values

Example:

```text
gender = "ABC"
```

If the valid categories are:

```text
Male
Female
Other
```

then `"ABC"` is suspicious.

Find categories:

```python
df["gender"].value_counts(dropna=False)
```

But unexpected values require investigation.

---

# 7.6 Inconsistent Values

You may see:

```text
Lahore
lahore
LAHORE
 Lahore
Lahore 
```

These may represent the same city.

Find them:

```python
df["city"].value_counts(dropna=False)
```

Standardization:

```python
df["city"] = df["city"].str.strip().str.title()
```

However, do not standardize blindly.

For example, `.title()` may not be appropriate for:

* Product codes
* Email addresses
* Usernames
* Abbreviations
* Official identifiers

---

# 7.7 Spelling and Capitalization Problems

Example:

```text
Punjab
punjab
PUNJAB
Panjab
```

Some are formatting differences.

Some may be actual spelling errors.

A reference list can help:

```python
valid_cities = ["Lahore", "Karachi", "Islamabad"]

df[~df["city"].isin(valid_cities)]
```

But before correcting `"Panjab"` to `"Punjab"`, investigate whether the value came from:

* Manual entry
* Different source
* Legacy system
* Mapping issue
* Actual location name

---

# 7.8 Extra Spaces

Detect:

```python
df["name"].astype(str).str.contains(r"^\s|\s$", regex=True).sum()
```

Standardize:

```python
df["name"] = df["name"].str.strip()
```

Extra spaces can create false categories:

```text
"Lahore"
"Lahore "
" Lahore"
```

These may appear as different values to a computer.

---

# 7.9 Incorrect Formats

Examples:

```text
Phone:
03001234567
+923001234567
0300-1234567
```

Or:

```text
Date:
2026-08-20
20/08/2026
08-20-2026
```

The values may represent the same information but use different formats.

The goal is usually to standardize representation.

---

# 7.10 Date Problems

Dates are especially important.

Inspect:

```python
df["date"] = pd.to_datetime(
    df["date"],
    errors="coerce"
)
```

Then investigate:

```python
df["date"].min()
df["date"].max()
```

Potential problems:

* Future dates
* Impossible dates
* Wrong day/month interpretation
* Missing dates
* Inconsistent formats
* Incorrect timezone
* Dates outside the business period

Example:

```text
Employee joining date = 2090-01-01
```

This may be invalid.

But investigate first.

Perhaps the field actually means something different.

---

# 7.11 Numerical Range Problems

Suppose:

```text
age
```

should normally represent human age.

Check:

```python
df["age"].min()
df["age"].max()
```

Potentially suspicious:

```text
age = -5
age = 250
```

You can detect them:

```python
df[(df["age"] < 0) | (df["age"] > 120)]
```

But a range must come from **domain knowledge**.

Do not invent arbitrary rules.

---

# 7.12 Outliers

An outlier is an observation that is unusually far from the rest of the data.

Example:

```text
Monthly spending:
20,000
22,000
25,000
21,000
23,000
1,500,000
```

The last value is unusual.

But unusual does not mean wrong.

It could represent:

* Luxury purchase
* Business customer
* Bulk order
* Fraud
* Data-entry error
* Currency conversion issue

### IQR method

```python
Q1 = df["amount"].quantile(0.25)
Q3 = df["amount"].quantile(0.75)

IQR = Q3 - Q1

lower = Q1 - 1.5 * IQR
upper = Q3 + 1.5 * IQR

outliers = df[
    (df["amount"] < lower) |
    (df["amount"] > upper)
]
```

The purpose of this calculation is **detection**, not automatic deletion.

---

# 7.13 Impossible Values

Examples:

```text
Age = -10
Quantity = -5
Temperature = -500°C
Birth date after death date
Exam score = 150 / 100
```

Some are mathematically impossible or violate domain constraints.

Example:

```python
df[df["exam_score"].between(0, 100) == False]
```

But again:

> Confirm the business definition before changing the value.

---

# 7.14 Conflicting Values Between Columns

Some problems only appear when multiple columns are compared.

Example:

```text
age = 20
date_of_birth = 1980
```

These values conflict.

Another example:

```text
order_quantity = 10
unit_price = 500
total_amount = 500
```

If:

```text
quantity × price = total
```

should be true, then this record violates the expected relationship.

Python:

```python
expected_total = df["quantity"] * df["unit_price"]

df[df["total_amount"] != expected_total]
```

The key idea:

> **Data quality is not only about individual columns. Relationships between columns matter too.**

---

# 7.15 Relationship and Logic Problems

Examples:

### Employee data

```text
joining_date > leaving_date
```

### Student data

```text
graduation_date < enrollment_date
```

### Banking

```text
loan_balance < 0
```

when negative balances are not allowed.

### E-commerce

```text
delivered_date < order_date
```

### Healthcare

```text
discharge_date < admission_date
```

These are often more important than simple missing-value checks.

---

# 7.16 Business-Rule Violations

A business rule is a rule defined by how the organization operates.

Example:

```text
Discount cannot exceed 50%.
```

Detection:

```python
df[df["discount_percent"] > 50]
```

Another:

```text
Employee overtime cannot be negative.
```

```python
df[df["overtime_hours"] < 0]
```

Another:

```text
Students cannot receive a score above 100.
```

```python
df[df["marks"] > 100]
```

The important question is:

> **Where did the rule come from?**

A professional rule should ideally come from:

* Business documentation
* Policy
* Data contract
* Subject-matter expert
* Database constraint
* System specification

---

# 7.17 Unexpected Categories

Check:

```python
df["department"].value_counts(dropna=False)
```

You may expect:

```text
IT
HR
Finance
Sales
```

but find:

```text
IT
HR
Finance
Sales
IT Department
Human Resources
fin
```

These may represent:

* Different naming conventions
* Mapping problems
* New legitimate categories
* Typographical errors
* Legacy values

Investigate before mapping them.

---

# 8. Step 5 — Investigate Before Changing Anything

This is one of the most important professional habits.

Suppose you find:

```text
age = 150
```

Do not immediately write:

```python
df["age"] = df["age"].clip(0, 100)
```

That changes the data without understanding it.

Instead ask:

### Question 1 — Is the value actually invalid?

Maybe the column does not represent age.

Maybe it represents:

```text
account age in months
```

---

### Question 2 — Is this an isolated record?

```python
df[df["age"] > 120]
```

If there is one record, it may be a data-entry error.

If there are 100,000 such records, there may be a systemic issue.

---

### Question 3 — Do other columns provide evidence?

For example:

```text
date_of_birth
age
customer_since
```

Compare them.

---

### Question 4 — Did the problem originate from the source?

Maybe the database exports:

```text
age in months
```

but someone interpreted it as years.

---

### Question 5 — Can the correct value be recovered?

Perhaps the original customer record contains the correct value.

---

# 9. Step 6 — Find the Root Cause

Finding the bad record is not always enough.

A professional asks:

> **Why did this happen?**

Consider:

```text
100,000 records
```

all contain:

```text
country = "PK"
```

instead of:

```text
Pakistan
```

You could replace `"PK"` with `"Pakistan"`.

But the deeper question is:

> Why did the source produce `"PK"`?

Possible root cause:

```text
Source system
    ↓
Uses ISO country codes
    ↓
Export process
    ↓
Data dictionary missing
    ↓
Analyst expects country names
```

The correct solution may not be simply replacing values.

The organization may need to fix the transformation process.

---

## Root Cause Questions

Ask:

1. Where was the value created?
2. Who or what entered it?
3. Was it manually entered?
4. Was it generated automatically?
5. Was it transformed?
6. Was it imported from another system?
7. Did the schema change?
8. Did a software update occur?
9. Was the business rule changed?
10. Is the problem isolated or widespread?
11. Does the same issue appear in historical data?
12. Does the problem occur at a particular date, location, department, or source?

---

# 10. Step 7 — Decide What Action to Take

After investigation, choose the appropriate action.

| Action                          | When to Use                                      |
| ------------------------------- | ------------------------------------------------ |
| **Keep**                        | Value is unusual but valid                       |
| **Correct**                     | Correct value is known with high confidence      |
| **Standardize**                 | Same meaning appears in different formats        |
| **Replace**                     | A reliable replacement value exists              |
| **Remove**                      | Record is invalid and cannot be safely recovered |
| **Recover from another source** | Reliable external/internal source exists         |
| **Ask the data owner**          | Meaning or correctness cannot be determined      |
| **Fix the source system**       | Problem originates upstream                      |

---

## Example

Suppose:

```text
city = "Lahor"
```

Possible actions:

### Correct

If there is strong evidence it means Lahore:

```text
Lahor → Lahore
```

### Ask the data owner

If `"Lahor"` could refer to another location.

### Fix source system

If thousands of records have the same problem because of a broken mapping.

---

# 11. Step 8 — Clean the Data

Only after investigation should transformation happen.

Common cleaning operations include:

```python
# Remove leading/trailing spaces
df["city"] = df["city"].str.strip()

# Standardize capitalization
df["city"] = df["city"].str.title()

# Convert numeric values
df["amount"] = pd.to_numeric(
    df["amount"],
    errors="coerce"
)

# Convert dates
df["date"] = pd.to_datetime(
    df["date"],
    errors="coerce"
)
```

For category mapping:

```python
city_mapping = {
    "Lahor": "Lahore",
    "lahore": "Lahore",
    "LAHORE": "Lahore"
}

df["city"] = df["city"].replace(city_mapping)
```

But every transformation should have a reason.

---

# 12. Step 9 — Validate the Cleaned Data

Cleaning is not finished when the code runs successfully.

You must validate the result.

---

## 12.1 Check row counts

Before:

```python
original_rows = len(df)
```

After:

```python
cleaned_rows = len(df)
```

If rows disappeared, determine why.

---

## 12.2 Check missing values again

```python
df.isna().sum()
```

Ask:

* Did missing values decrease?
* Did we accidentally create new missing values?
* Were missing values replaced appropriately?

---

## 12.3 Check duplicates again

```python
df.duplicated().sum()
```

---

## 12.4 Check data types again

```python
df.dtypes
```

---

## 12.5 Re-run business rules

Example:

```python
invalid_scores = df[
    (df["marks"] < 0) |
    (df["marks"] > 100)
]

print(len(invalid_scores))
```

The expected result should be zero if that rule is mandatory.

---

## 12.6 Validate relationships

```python
expected_total = df["quantity"] * df["unit_price"]

errors = df[
    df["total_amount"] != expected_total
]

print(len(errors))
```

---

## 12.7 Compare before and after

A professional cleaning process should answer:

```text
Rows before:              100,000
Rows after:                99,950
Missing customer emails:    8,000 → 1,200
Duplicate records:            500 → 0
Invalid ages:                 120 → 0
Invalid dates:                 35 → 0
```

But numbers alone are not enough.

You should also explain **why** the changes occurred.

---

# 13. Step 10 — Document Everything

A professional data-cleaning process should be reproducible.

Document:

* What problem was found
* How it was detected
* How it was investigated
* Root cause
* Decision
* Transformation applied
* Number of affected records
* Validation performed
* Remaining limitations

Example:

```text
Problem:
City values contained inconsistent capitalization and whitespace.

Investigation:
Values such as "lahore", "LAHORE", and " Lahore " represented the same city.

Action:
Trim whitespace and standardize city names.

Affected records:
4,281

Validation:
Unique city categories reduced from 31 to 24.

Status:
Resolved.
```

---

# 14. Industry Examples

## 14.1 E-Commerce

Dataset:

```text
order_id
customer_id
product_id
quantity
unit_price
discount
order_date
delivery_date
```

Potential problems:

* Negative quantity
* Zero/negative price
* Invalid discount
* Delivery before order
* Duplicate order IDs
* Unknown product IDs
* Incorrect total
* Missing customer ID
* Unexpected currencies

Relationship check:

```python
expected = df["quantity"] * df["unit_price"]

df[df["total"] != expected]
```

---

## 14.2 Banking

Dataset:

```text
customer_id
account_id
balance
transaction_amount
transaction_date
account_type
```

Potential problems:

* Negative values where prohibited
* Invalid account types
* Duplicate transactions
* Future transaction dates
* Incorrect account/customer relationships
* Currency inconsistencies
* Impossible transaction amounts
* Missing account IDs

A suspicious transaction should not automatically be deleted.

It could represent:

* Fraud
* Chargeback
* Reversal
* Refund
* Legitimate large transaction

Investigation is essential.

---

## 14.3 Healthcare

Dataset:

```text
patient_id
date_of_birth
admission_date
discharge_date
diagnosis
```

Potential problems:

```text
discharge_date < admission_date
```

or:

```text
date_of_birth > admission_date
```

Possible causes:

* Data-entry error
* Date-format problem
* Migration issue
* Incorrect patient ID
* Timezone issue

Healthcare data requires particularly careful handling because an incorrect assumption can have serious consequences.

---

## 14.4 Education

Dataset:

```text
student_id
course
marks
attendance
enrollment_date
graduation_date
```

Potential problems:

```text
marks > 100
attendance > 100%
attendance < 0%
graduation_date < enrollment_date
```

But always verify the institution's actual rules.

Some grading systems may use:

```text
marks out of 50
```

rather than 100.

---

## 14.5 Customer Database

Dataset:

```text
customer_id
name
email
phone
city
signup_date
```

Potential problems:

* Duplicate customers
* Multiple email formats
* Spaces
* Invalid phone formats
* Different city spellings
* Missing contact information
* Duplicate emails
* Invalid signup dates

Important question:

> Are two rows actually duplicates, or are they two records belonging to the same person?

That is an entity-resolution problem, not necessarily a simple duplicate-row problem.

---

## 14.6 Sales

Dataset:

```text
salesperson
region
product
quantity
revenue
sale_date
```

Potential problems:

* Negative sales
* Incorrect revenue
* Unexpected products
* Missing salesperson
* Invalid region
* Future sales dates
* Duplicate invoices

A large sale is not necessarily an error.

It could be a legitimate enterprise customer.

---

## 14.7 Employee Data

Dataset:

```text
employee_id
department
salary
joining_date
leaving_date
job_title
```

Potential problems:

```text
salary < 0
joining_date > leaving_date
unknown department
duplicate employee ID
missing employee ID
invalid job title
```

But again, an employee with a very high salary is not automatically an error.

It may be a senior executive.

---

# 15. Data Problem vs Data-Quality Problem vs Business-Rule Problem vs Source-System Problem

These concepts are related but different.

| Concept                   | Meaning                                               | Example                            |
| ------------------------- | ----------------------------------------------------- | ---------------------------------- |
| **Data Problem**          | Something unexpected or difficult about the data      | A column has mixed formats         |
| **Data-Quality Problem**  | Data does not meet expected quality characteristics   | Customer email is invalid          |
| **Business-Rule Problem** | Data violates a known business constraint             | Discount > allowed maximum         |
| **Source-System Problem** | The upstream system creates or exports incorrect data | CRM export truncates phone numbers |

---

## Example

Suppose you discover:

```text
customer_city = "Lahor"
```

### Data problem

The value differs from the expected spelling.

### Data-quality problem

The city value is inconsistent with the organization's standard city reference.

### Business-rule problem

If the system requires every customer to have a valid city from an approved list, it violates that rule.

### Source-system problem

If the CRM automatically generates `"Lahor"` due to a broken mapping table, the root cause is upstream.

The same record can therefore be viewed at multiple levels.

---

# 16. Professional Data Cleaning Checklist

Use this whenever you receive a new dataset.

## A. Understand

* [ ] Do I know where the data came from?
* [ ] Do I know why the data was collected?
* [ ] Do I know what the dataset is used for?
* [ ] Do I know what one row represents?
* [ ] Do I understand the grain of the data?
* [ ] Do I know what each column means?
* [ ] Do I have a data dictionary?
* [ ] Do I know which columns should be unique?
* [ ] Do I know the expected data types?
* [ ] Do I know the expected categories?
* [ ] Do I know the important business rules?

## B. Inspect

* [ ] Check number of rows and columns
* [ ] Inspect column names
* [ ] Inspect data types
* [ ] View sample records
* [ ] Review descriptive statistics
* [ ] Review unique values
* [ ] Check date ranges
* [ ] Check numerical ranges
* [ ] Check categorical distributions

## C. Detect

* [ ] Missing values
* [ ] Empty strings
* [ ] Special missing-value markers
* [ ] Duplicate records
* [ ] Duplicate business keys
* [ ] Incorrect data types
* [ ] Invalid values
* [ ] Unexpected categories
* [ ] Spelling differences
* [ ] Capitalization differences
* [ ] Extra spaces
* [ ] Incorrect formats
* [ ] Date problems
* [ ] Numerical range problems
* [ ] Outliers
* [ ] Impossible values
* [ ] Conflicting columns
* [ ] Broken relationships
* [ ] Business-rule violations

## D. Investigate

* [ ] Is the suspicious value actually wrong?
* [ ] Could it be a legitimate unusual value?
* [ ] Is the problem isolated?
* [ ] Does it affect many records?
* [ ] Does the problem occur in specific groups?
* [ ] Does it occur during a specific time period?
* [ ] Can another column confirm the correct value?
* [ ] Can the original source be checked?
* [ ] Can another reliable source confirm the value?
* [ ] Has the source system changed?
* [ ] Has the business rule changed?

## E. Find Root Cause

* [ ] Where was the value created?
* [ ] Was it entered manually?
* [ ] Was it generated automatically?
* [ ] Was it imported?
* [ ] Was it transformed?
* [ ] Did a mapping fail?
* [ ] Did a schema change?
* [ ] Did a system migration occur?
* [ ] Is the problem caused upstream?
* [ ] Is the issue still happening?

## F. Decide

For every problem, choose:

* [ ] Keep
* [ ] Correct
* [ ] Standardize
* [ ] Replace
* [ ] Remove
* [ ] Recover from another source
* [ ] Ask the data owner
* [ ] Fix the source system

## G. Clean

* [ ] Apply controlled transformations
* [ ] Preserve the original data
* [ ] Avoid irreversible changes when possible
* [ ] Record affected records
* [ ] Keep cleaning logic reproducible
* [ ] Avoid deleting suspicious data without justification

## H. Validate

* [ ] Compare row counts before and after
* [ ] Recheck missing values
* [ ] Recheck duplicates
* [ ] Recheck data types
* [ ] Recheck invalid values
* [ ] Recheck categories
* [ ] Recheck date ranges
* [ ] Recheck numerical ranges
* [ ] Recheck relationships
* [ ] Recheck business rules
* [ ] Confirm no new problems were introduced

## I. Document

* [ ] Record the original problem
* [ ] Record how it was detected
* [ ] Record the investigation
* [ ] Record the root cause
* [ ] Record the decision
* [ ] Record the transformation
* [ ] Record affected records
* [ ] Record validation results
* [ ] Record unresolved issues
* [ ] Record assumptions

---

# 17. Final Principles

A senior data professional follows a few fundamental principles.

## Principle 1 — Understand before cleaning

You cannot determine whether data is wrong if you do not understand what it represents.

---

## Principle 2 — Unusual does not mean incorrect

An outlier may be:

* An error
* A rare event
* A high-value customer
* A legitimate transaction
* A genuine business event

Investigate first.

---

## Principle 3 — Never blindly delete data

Deleting data is often irreversible.

Ask:

> Can I explain why this record should not exist?

If not, investigate further.

---

## Principle 4 — Context determines correctness

There is no universal definition of "clean data."

For example:

```text
Age = 100
```

may be unusual but valid.

```text
Discount = 70%
```

may be valid in one business and invalid in another.

---

## Principle 5 — Relationships matter

A dataset can look clean column-by-column while being logically wrong.

Always check relationships between fields.

```text
Quantity × Price = Total
```

```text
Join Date < Leave Date
```

```text
Birth Date < Admission Date
```

```text
Graduation Date > Enrollment Date
```

---

## Principle 6 — Fix the cause, not only the symptom

If a system creates 50,000 incorrect records, manually fixing those records is not the complete solution.

Find out why the system generated them.

The best solution may be:

```text
Fix source system
        ↓
Prevent future bad records
        ↓
Clean historical records
        ↓
Validate
```

---

## Principle 7 — Preserve evidence

Before changing data:

```text
Raw Data
   ↓
Cleaning Process
   ↓
Clean Data
```

Keep the raw data whenever possible.

Your cleaning process should be reproducible.

---

## Principle 8 — Every change should have a reason

A professional should be able to answer:

> "Why did you change this value?"

A strong answer looks like:

```text
The value violated the documented business rule.
The source system confirmed the value was incorrect.
The correct value was recovered from the authoritative database.
The transformation affected 342 records.
Validation confirmed that all affected records now satisfy the rule.
```

Not:

```text
"I changed it because it looked wrong."
```

---

# The Senior Data Problem-Solver Mindset

The ultimate goal of data cleaning is not to make a dataset **look clean**.

The goal is to make the data:

* Understandable
* Consistent
* Valid
* Reliable
* Traceable
* Fit for purpose
* Defensible

The most important habit is:

> **Do not ask only "How do I clean this data?" Ask "What is this data supposed to mean, what evidence tells me something is wrong, why did the problem happen, and what is the safest action?"**

That mindset transforms data cleaning from a collection of Pandas commands into **professional data-quality engineering**.

```text
Understand
    ↓
Inspect
    ↓
Detect
    ↓
Investigate
    ↓
Find Root Cause
    ↓
Decide
    ↓
Clean
    ↓
Validate
    ↓
Document
```

**Good data cleaning does not simply remove problems.**

**Good data cleaning creates evidence that the data is trustworthy enough for its intended purpose.**
