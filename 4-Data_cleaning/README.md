# Data Cleaning & Meaningful Insights

[svg](https://github.com/waris-idrees/Machine-learning_journey/blob/main/4-Data-Cleaning/README.md#data-cleaning--meaningful-insights)

Data Cleaning is the process of understanding, inspecting, detecting, investigating, correcting, standardizing, validating, and documenting problems in raw data.

The goal is not simply to make the dataset "clean".

The goal is to transform **raw data → reliable data → meaningful information → useful insights**.

---

## Data Cleaning & Insight Workflow

[svg](https://github.com/waris-idrees/Machine-learning_journey/blob/main/4-Data-Cleaning/README.md#data-cleaning--insight-workflow)

```text
Raw Dataset
     ↓
Understand the Dataset
     ↓
Inspect the Data
     ↓
Identify Data Quality Problems
     ↓
Investigate Problems
     ↓
Find Root Cause
     ↓
Clean & Transform Data
     ↓
Validate Cleaned Data
     ↓
Explore the Clean Dataset
     ↓
Analyze Patterns & Relationships
     ↓
Calculate Important Metrics
     ↓
Generate Meaningful Insights
     ↓
Communicate Findings
     ↓
Business / Data-Driven Decisions
```

---

# Data Cleaning Steps

[svg](https://github.com/waris-idrees/Machine-learning_journey/blob/main/4-Data-Cleaning/README.md#data-cleaning-steps)

## 1. Understand the Dataset

Before cleaning, understand what the dataset represents.

Ask:

* Where did the data come from?
* Why was it collected?
* What is the purpose of the dataset?
* What does one row represent?
* What does each column represent?
* Which columns are identifiers?
* Which columns are numerical?
* Which columns are categorical?
* Which columns contain dates?
* Which columns should be unique?
* What business rules apply?

> Never start changing data before understanding what the data means.

---

## 2. Load the Dataset

```python
import pandas as pd

df = pd.read_csv("data.csv")
```

For Excel:

```python
df = pd.read_excel("data.xlsx")
```

---

## 3. Check Dataset Shape

```python
df.shape
```

This tells you:

```text
Rows × Columns
```

Example:

```text
(10000, 12)
```

Meaning:

* 10,000 rows
* 12 columns

---

## 4. Check Column Names

```python
df.columns
```

Look for:

* Incorrect names
* Duplicate names
* Unclear names
* Extra spaces
* Inconsistent naming

Example:

```text
Customer ID
customer_id
CUSTOMER_ID
```

These may need standardization.

---

## 5. Inspect Sample Records

```python
df.head()
```

```python
df.tail()
```

Random sample:

```python
df.sample(10)
```

The purpose is to understand how the data actually looks.

---

## 6. Check Data Types

```python
df.dtypes
```

Also:

```python
df.info()
```

Check whether columns have appropriate types.

Examples:

```text
age → integer
salary → numeric
city → string/category
signup_date → datetime
```

---

## 7. Check Missing Values

```python
df.isnull().sum()
```

Percentage:

```python
df.isnull().mean() * 100
```

Important:

> Missing data is not automatically bad data.

A missing middle name may be acceptable.

A missing transaction amount may be a serious problem.

---

## 8. Check Empty and Special Values

Common representations of missing information include:

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

Check categorical columns:

```python
for col in df.select_dtypes(include="object"):
    print(col)
    print(df[col].value_counts(dropna=False).head(10))
```

Do not automatically replace every special value.

First understand what it represents.

---

## 9. Check Duplicate Records

```python
df.duplicated().sum()
```

View duplicates:

```python
df[df.duplicated(keep=False)]
```

But remember:

> Duplicate rows and duplicate entities are not always the same thing.

If the dataset contains orders, the same customer can appear many times.

---

## 10. Check Duplicate Keys

If `customer_id` should be unique:

```python
df["customer_id"].duplicated().sum()
```

If `order_id` should be unique:

```python
df["order_id"].duplicated().sum()
```

Always confirm the expected uniqueness from the dataset's business definition.

---

## 11. Check Unique Values

```python
df.nunique()
```

For a specific column:

```python
df["city"].unique()
```

Frequency:

```python
df["city"].value_counts()
```

This helps identify:

* Unexpected categories
* Spelling errors
* Inconsistent values
* Rare categories
* Incorrect entries

---

## 12. Check Spelling and Capitalization

Example:

```text
Lahore
lahore
LAHORE
Lahor
```

Check:

```python
df["city"].value_counts()
```

Possible standardization:

```python
df["city"] = df["city"].str.strip().str.title()
```

But investigate spelling errors before replacing them.

---

## 13. Remove Extra Spaces

```python
df["name"] = df["name"].str.strip()
```

For multiple text columns:

```python
text_columns = df.select_dtypes(include="object").columns

for col in text_columns:
    df[col] = df[col].str.strip()
```

---

## 14. Check Incorrect Data Types

Convert numeric values:

```python
df["age"] = pd.to_numeric(
    df["age"],
    errors="coerce"
)
```

Convert dates:

```python
df["date"] = pd.to_datetime(
    df["date"],
    errors="coerce"
)
```

Be careful with identifiers.

For example:

```text
customer_id = "001245"
```

may need to remain a string.

---

## 15. Check Invalid Values

Examples:

```text
Age = -10
Quantity = -5
Marks = 150
Discount = 200%
```

Example:

```python
df[
    (df["age"] < 0) |
    (df["age"] > 120)
]
```

---

## 16. Check Numerical Ranges

Use domain knowledge to define valid ranges.

Example:

```python
df[
    (df["marks"] < 0) |
    (df["marks"] > 100)
]
```

For percentage:

```python
df[
    (df["attendance"] < 0) |
    (df["attendance"] > 100)
]
```

A valid range should come from:

* Business rules
* Domain knowledge
* Documentation
* Data dictionary
* Subject-matter experts

---

## 17. Check Date Problems

Convert dates:

```python
df["date"] = pd.to_datetime(
    df["date"],
    errors="coerce"
)
```

Check minimum and maximum:

```python
df["date"].min()
df["date"].max()
```

Look for:

* Future dates
* Impossible dates
* Missing dates
* Incorrect formats
* Wrong date interpretation

---

## 18. Check Outliers

Outliers are unusual values.

They are **not automatically errors**.

Use IQR:

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

Investigate outliers before removing them.

---

## 19. Check Impossible Values

Examples:

```text
Age = -5
Quantity = -20
Marks = 150
Birth Date > Admission Date
Delivery Date < Order Date
```

These can indicate data-quality problems.

---

## 20. Check Relationships Between Columns

Some problems only appear when columns are compared.

Example:

```text
quantity × unit_price = total_amount
```

Check:

```python
expected_total = (
    df["quantity"] *
    df["unit_price"]
)

df[
    df["total_amount"] != expected_total
]
```

---

## 21. Check Logical Relationships

Examples:

### Employee

```text
Joining Date < Leaving Date
```

### Student

```text
Enrollment Date < Graduation Date
```

### Healthcare

```text
Admission Date < Discharge Date
```

### E-Commerce

```text
Order Date < Delivery Date
```

---

## 22. Check Business Rules

Business rules describe what should be allowed.

Example:

```text
Discount cannot exceed 50%.
```

```python
df[df["discount"] > 50]
```

Another example:

```text
Exam marks must be between 0 and 100.
```

```python
df[
    (df["marks"] < 0) |
    (df["marks"] > 100)
]
```

---

# Investigate Before Cleaning

[svg](https://github.com/waris-idrees/Machine-learning_journey/blob/main/4-Data-Cleaning/README.md#investigate-before-cleaning)

Never immediately delete or replace suspicious values.

Ask:

1. Is the value actually wrong?
2. Could it be a legitimate unusual value?
3. Is the problem isolated?
4. Does the problem affect many records?
5. Can another column confirm the correct value?
6. Can the original source be checked?
7. Can the correct value be recovered?
8. Is there a documented business rule?
9. Did the source system recently change?

---

# Find the Root Cause

[svg](https://github.com/waris-idrees/Machine-learning_journey/blob/main/4-Data-Cleaning/README.md#find-the-root-cause)

Finding a bad value is only part of the problem.

Ask:

```text
Where was the value created?
        ↓
Who or what entered it?
        ↓
Was it manually entered?
        ↓
Was it imported?
        ↓
Was it transformed?
        ↓
Was it mapped incorrectly?
        ↓
Did the source system change?
```

A professional does not only fix the data.

They try to prevent the same problem from happening again.

---

# Decide the Cleaning Action

[svg](https://github.com/waris-idrees/Machine-learning_journey/blob/main/4-Data-Cleaning/README.md#decide-the-cleaning-action)

| Action            | When to Use                               |
| ----------------- | ----------------------------------------- |
| Keep              | Value is unusual but valid                |
| Correct           | Correct value is known                    |
| Standardize       | Same meaning has different formats        |
| Replace           | Reliable replacement exists               |
| Remove            | Record is invalid and cannot be recovered |
| Recover           | Correct value exists in another source    |
| Ask Data Owner    | Meaning is unclear                        |
| Fix Source System | Problem originates upstream               |

---

# Clean and Transform the Data

[svg](https://github.com/waris-idrees/Machine-learning_journey/blob/main/4-Data-Cleaning/README.md#clean-and-transform-the-data)

## Handle Missing Values

Remove rows when appropriate:

```python
df = df.dropna()
```

Fill with a meaningful value:

```python
df["age"] = df["age"].fillna(df["age"].median())
```

For categorical data:

```python
df["city"] = df["city"].fillna("Unknown")
```

Do not use these methods automatically.

The correct method depends on why the value is missing.

---

## Remove Confirmed Duplicates

```python
df = df.drop_duplicates()
```

For a specific key:

```python
df = df.drop_duplicates(
    subset=["customer_id"]
)
```

Only do this when the business definition confirms that the records should be unique.

---

## Standardize Text

```python
df["city"] = (
    df["city"]
    .str.strip()
    .str.title()
)
```

---

## Standardize Categories

```python
mapping = {
    "Lahor": "Lahore",
    "lahore": "Lahore",
    "LAHORE": "Lahore"
}

df["city"] = df["city"].replace(mapping)
```

---

## Convert Data Types

```python
df["age"] = pd.to_numeric(
    df["age"],
    errors="coerce"
)
```

```python
df["signup_date"] = pd.to_datetime(
    df["signup_date"],
    errors="coerce"
)
```

---

## Handle Outliers

Possible actions:

* Keep
* Investigate
* Correct
* Cap/Winsorize
* Transform
* Remove

Never remove an outlier simply because it is large or small.

---

# Validate the Cleaned Dataset

[svg](https://github.com/waris-idrees/Machine-learning_journey/blob/main/4-Data-Cleaning/README.md#validate-the-cleaned-dataset)

After cleaning, run the checks again.

```python
df.shape
```

```python
df.isna().sum()
```

```python
df.duplicated().sum()
```

```python
df.dtypes
```

```python
df.describe()
```

Check business rules again.

Check relationships again.

Compare before and after.

Example:

| Quality Check  |  Before |  After |
| -------------- | ------: | -----: |
| Rows           | 100,000 | 99,950 |
| Missing Values |   8,000 |  1,200 |
| Duplicates     |     500 |      0 |
| Invalid Ages   |     120 |      0 |
| Invalid Dates  |      35 |      0 |

---

# From Clean Data to Meaningful Insights

[svg](https://github.com/waris-idrees/Machine-learning_journey/blob/main/4-Data-Cleaning/README.md#from-clean-data-to-meaningful-insights)

Cleaning the data is **not the end**.

The next goal is to convert clean data into useful information and insights.

The process is:

```text
Clean Data
    ↓
Ask Questions
    ↓
Define Metrics
    ↓
Aggregate Data
    ↓
Compare Groups
    ↓
Analyze Trends
    ↓
Analyze Distributions
    ↓
Analyze Relationships
    ↓
Identify Patterns
    ↓
Investigate Anomalies
    ↓
Generate Insights
    ↓
Communicate Findings
```

---

# 1. Understand the Business Problem

Before calculating anything, ask:

* What problem are we trying to solve?
* Who will use the analysis?
* What decision should the analysis support?
* What questions should the data answer?

Do not start with charts.

Start with a question.

---

# 2. Define Analytical Questions

Examples:

### E-Commerce

* Which products generate the most revenue?
* Which cities have the most customers?
* Which months have the highest sales?
* Which products have low sales?
* What is the average order value?

### Banking

* Which customers have the highest transaction volume?
* What is the average account balance?
* Which transaction types are most common?

### Education

* Which students have low performance?
* Does attendance relate to marks?
* Which subjects have the highest average scores?

---

# 3. Define Important Metrics

Common metrics include:

### Count

```python
df["customer_id"].nunique()
```

### Sum

```python
df["revenue"].sum()
```

### Average

```python
df["revenue"].mean()
```

### Median

```python
df["revenue"].median()
```

### Minimum

```python
df["revenue"].min()
```

### Maximum

```python
df["revenue"].max()
```

---

# 4. Group and Aggregate Data

Grouping helps answer business questions.

Example:

```python
df.groupby("city")["revenue"].sum()
```

Average revenue:

```python
df.groupby("city")["revenue"].mean()
```

Customer count:

```python
df.groupby("city")["customer_id"].nunique()
```

Multiple metrics:

```python
df.groupby("city").agg({
    "revenue": ["sum", "mean"],
    "customer_id": "nunique"
})
```

---

# 5. Compare Groups

Compare categories such as:

```text
City
Gender
Department
Product
Customer Segment
Age Group
Region
```

Example:

```python
df.groupby("department")["salary"].mean()
```

The goal is to identify meaningful differences between groups.

---

# 6. Analyze Trends Over Time

Extract useful date features:

```python
df["year"] = df["date"].dt.year
df["month"] = df["date"].dt.month
```

Monthly revenue:

```python
monthly_sales = (
    df.groupby("month")["revenue"]
    .sum()
)
```

Look for:

* Increasing trends
* Decreasing trends
* Seasonal patterns
* Sudden changes
* Unusual periods

---

# 7. Analyze Distributions

Use:

* Histogram
* Box Plot
* Density Plot
* Bar Plot

Example:

```python
df["age"].hist()
```

Understand:

* Typical values
* Spread
* Skewness
* Outliers
* Rare values

---

# 8. Analyze Relationships

Compare variables.

Example:

```python
df.plot.scatter(
    x="advertising_spend",
    y="sales"
)
```

Questions:

* Does one variable increase with another?
* Is there a negative relationship?
* Is there no obvious relationship?
* Are there unusual groups?

---

# 9. Analyze Correlation

```python
df.corr(numeric_only=True)
```

Visualize:

```python
import seaborn as sns
import matplotlib.pyplot as plt

sns.heatmap(
    df.corr(numeric_only=True),
    annot=True
)

plt.show()
```

Correlation can identify relationships between numerical variables.

> Correlation does not automatically mean causation.

---

# 10. Create Useful Visualizations

Common visualization techniques:

* Histogram
* Box Plot
* Bar Plot
* Count Plot
* Scatter Plot
* Line Plot
* Heatmap
* Pair Plot

Choose the visualization based on the question.

Do not create charts simply because you can.

---

# 11. Identify Patterns

Look for:

* Highest-performing groups
* Lowest-performing groups
* Increasing/decreasing trends
* Seasonal behavior
* Strong relationships
* Unusual clusters
* Outliers
* Significant differences
* Repeated patterns

---

# 12. Investigate Anomalies

If you find:

```text
Sales suddenly dropped by 70%.
```

Do not immediately conclude:

> "The business performed badly."

Investigate:

* Was there a holiday?
* Was the system down?
* Did the product become unavailable?
* Did the data collection process change?
* Was the currency changed?
* Are records missing?

An anomaly is a signal for investigation.

---

# 13. Convert Findings Into Insights

A number is not automatically an insight.

### Weak Finding

> Lahore generated 50 million in sales.

### Better Finding

> Lahore generated the highest total sales among all cities.

### Meaningful Insight

> Lahore generated the highest sales and accounted for a large share of total revenue, suggesting that the city is an important market and may deserve greater sales and marketing focus.

A strong insight contains:

```text
Observation
    +
Context
    +
Meaning
    +
Potential Action
```

---

# 14. Example: Raw Data → Insight

Suppose the data shows:

```text
City       Revenue
Lahore     50M
Karachi    35M
Islamabad  20M
Multan     10M
```

### Observation

Lahore has the highest revenue.

### Comparison

Lahore generates more revenue than every other city.

### Interpretation

Lahore is currently the strongest market in the dataset.

### Possible Action

Investigate what drives Lahore's performance and determine whether successful strategies can be applied to other cities.

---

# 15. Create an Insight Summary

A professional analysis should finish with clear findings.

Example:

### Key Insights

1. **Lahore generated the highest revenue**, making it the strongest market in the dataset.
2. **Sales increased during the final quarter**, suggesting a possible seasonal pattern.
3. **A small group of products generated a large share of total revenue**, indicating product concentration.
4. **Several high-value transactions were identified as outliers**, but investigation showed that they were legitimate business transactions.
5. **Customer spending was higher in certain segments**, suggesting opportunities for targeted marketing.

---

# 16. Separate Facts From Assumptions

Always distinguish between:

### Fact

> Revenue increased by 20% between January and March.

### Interpretation

> The increase may indicate stronger demand.

### Hypothesis

> The increase may have been caused by the marketing campaign.

Do not present a hypothesis as a confirmed fact unless the data supports it.

---

# 17. Final Data Analysis Workflow

```text
Raw Data
    ↓
Understand
    ↓
Inspect
    ↓
Clean
    ↓
Validate
    ↓
Explore
    ↓
Ask Questions
    ↓
Define Metrics
    ↓
Aggregate
    ↓
Compare
    ↓
Analyze Trends
    ↓
Analyze Relationships
    ↓
Visualize
    ↓
Identify Patterns
    ↓
Investigate Anomalies
    ↓
Generate Insights
    ↓
Communicate Findings
    ↓
Support Decisions
```

---

# Professional Data Cleaning Checklist

## Understand

* [ ] Identify the data source
* [ ] Understand the purpose
* [ ] Understand what one row represents
* [ ] Understand every column
* [ ] Identify keys
* [ ] Identify expected data types
* [ ] Understand business rules

## Inspect

* [ ] Check shape
* [ ] Check columns
* [ ] Check data types
* [ ] Inspect sample rows
* [ ] Review statistics
* [ ] Review unique values

## Detect

* [ ] Missing values
* [ ] Empty strings
* [ ] Special values
* [ ] Duplicates
* [ ] Invalid values
* [ ] Incorrect types
* [ ] Spelling errors
* [ ] Capitalization problems
* [ ] Extra spaces
* [ ] Format problems
* [ ] Date problems
* [ ] Range problems
* [ ] Outliers
* [ ] Impossible values
* [ ] Conflicting columns
* [ ] Relationship problems
* [ ] Business-rule violations
* [ ] Unexpected categories

## Investigate

* [ ] Determine whether the value is actually wrong
* [ ] Check whether unusual values are legitimate
* [ ] Find patterns
* [ ] Check related columns
* [ ] Check historical data
* [ ] Check source system
* [ ] Consult the data owner

## Clean

* [ ] Handle missing values
* [ ] Remove confirmed duplicates
* [ ] Correct confirmed errors
* [ ] Standardize values
* [ ] Convert data types
* [ ] Standardize formats
* [ ] Handle confirmed invalid values
* [ ] Document changes

## Validate

* [ ] Check row counts
* [ ] Check missing values
* [ ] Check duplicates
* [ ] Check data types
* [ ] Check ranges
* [ ] Check categories
* [ ] Check dates
* [ ] Check relationships
* [ ] Re-run business rules
* [ ] Confirm no new problems

## Generate Insights

* [ ] Understand the business question
* [ ] Define analytical questions
* [ ] Define metrics
* [ ] Aggregate data
* [ ] Compare groups
* [ ] Analyze trends
* [ ] Analyze distributions
* [ ] Analyze relationships
* [ ] Analyze correlation
* [ ] Visualize important patterns
* [ ] Identify anomalies
* [ ] Investigate unusual findings
* [ ] Generate meaningful insights
* [ ] Separate facts from assumptions
* [ ] Communicate findings clearly
* [ ] Suggest possible actions

---

# Goal

The goal of Data Cleaning and Analysis is to transform:

```text
Raw Data
    ↓
Clean & Reliable Data
    ↓
Structured Information
    ↓
Patterns & Relationships
    ↓
Meaningful Insights
    ↓
Better Decisions
```

> **Data Cleaning makes the data trustworthy.**

> **Data Analysis makes the data understandable.**

> **Meaningful Insights turn understanding into action.**
