# PhonePe Pulse --- Digital Payments Analytics Dashboard

## Project Overview

This project analyzes **PhonePe digital payment transactions in India**
using data published through the official **PhonePe Pulse** initiative.

The project follows an end-to-end analytics workflow:

**Official PhonePe Pulse Dataset → Python/Jupyter ETL → Data Cleaning &
Transformation → Excel → Power BI → KPIs & Visualizations → Business
Insights**

The objective is to understand transaction volume, transaction value,
transaction categories, quarterly growth, average transaction value, and
geographic patterns across Indian states.

------------------------------------------------------------------------

## Business Questions

The analysis is designed to answer the following business questions:

1.  Which transaction type dominates PhonePe?
2.  How has transaction volume changed over time?
3.  How has transaction value changed over time?
4.  Which quarter has the highest transaction volume?
5.  Which quarter has the highest transaction value?
6.  Is transaction value growing faster than transaction count?
7.  What is the average transaction value?
8.  Which states contribute the most transactions?
9.  Which states have the highest transaction value?
10. Which states have high transaction volume but relatively low
    transaction value?
11. How has PhonePe adoption evolved geographically?
12. Which transaction categories show the strongest growth?

------------------------------------------------------------------------

## Data Source

The transaction data was downloaded from the official **PhonePe Pulse**
platform.

**Source:** PhonePe Pulse --- https://www.phonepe.com/pulse/

The raw dataset is organized by year and quarter and contains
transaction-level/category-level and geographic information.

The analysis uses the India transaction data and, where applicable,
state-wise data.

------------------------------------------------------------------------

## Tools & Technologies

  Tool               Purpose
  ------------------ -------------------------------------------
  Python             Data extraction, ETL and analysis
  Jupyter Notebook   Interactive data preparation and analysis
  Pandas             Data manipulation and transformation
  JSON               Raw PhonePe Pulse data format
  Excel              Prepared analytical datasets
  Power BI           Interactive dashboard and visualization
  DAX                KPI and analytical measures
  Git/GitHub         Project version control and documentation

------------------------------------------------------------------------

## Project Workflow

### 1. Data Collection

The raw JSON files were downloaded from the official PhonePe Pulse
dataset.

The data is structured approximately as:

``` text
data/
└── aggregated/
    └── transaction/
        └── country/
            └── india/
                ├── 2018/
                │   ├── 1.json
                │   ├── 2.json
                │   ├── 3.json
                │   └── 4.json
                ├── 2019/
                ├── 2020/
                └── ...
```

Each quarter contains transaction categories and their corresponding
transaction count and transaction amount.

------------------------------------------------------------------------

### 2. Python / Jupyter ETL

Python was used to automatically iterate through all available year and
quarter JSON files.

The ETL process:

1.  Reads each year directory.
2.  Identifies valid quarterly JSON files.
3.  Loads JSON data using Python.
4.  Extracts transaction categories.
5.  Extracts transaction count.
6.  Extracts transaction amount.
7.  Adds year and quarter information.
8.  Combines all records into a Pandas DataFrame.

The resulting analytical structure contains fields such as:

``` text
transaction_type
transaction_count
transaction_amount
year
quarter
```

State-level datasets were additionally prepared with fields such as:

``` text
state
year
quarter
transactions
amount
transaction_growth
```

------------------------------------------------------------------------

## Core Python Transformation

The raw JSON records were converted into a tabular DataFrame so that
they could be analyzed and exported.

Conceptually:

``` python
all_rows = []

for year in years:
    for quarter in quarters:
        read_json_file()
        extract_transaction_data()
        append_records()

df = pd.DataFrame(all_rows)
```

This approach ensures that the final DataFrame contains records
collected across the available year-quarter JSON files rather than
relying on manual file-by-file processing.

------------------------------------------------------------------------

## Data Preparation

The prepared data was checked for:

-   Correct year and quarter values
-   Numeric transaction counts
-   Numeric transaction amounts
-   Consistent transaction category names
-   State and geographic fields
-   Duplicate or invalid records
-   Correct chronological ordering

A chronological `YearQuarter` field was also used for time-series
analysis.

Example:

``` text
2018 Q1
2018 Q2
2018 Q3
2018 Q4
2019 Q1
...
```

A separate sorting key can be created using:

``` text
YearQuarterSort = Year × 10 + Quarter
```

This prevents Power BI from sorting quarter labels alphabetically.

------------------------------------------------------------------------

## Excel Output

After ETL and transformation, the prepared DataFrames were exported to
Excel for easy inspection and Power BI ingestion.

Example:

``` python
df.to_excel("phonepe_transactions.xlsx", index=False)
```

The Excel workbook contains prepared datasets for:

-   Quarterly transaction analysis
-   Transaction growth
-   Amount/value growth
-   State-level analysis
-   State-year analysis
-   State-quarter analysis

------------------------------------------------------------------------

# Power BI Dashboard

The Power BI dashboard converts the prepared datasets into an
interactive business intelligence report.

The dashboard is organized into multiple analytical sections.

------------------------------------------------------------------------

## Dashboard Page 1 --- Transaction Overview

### KPI Cards

The main dashboard contains KPIs such as:

### Total Transaction Volume

``` dax
Total Transaction Volume =
SUM(Quarterly_Transactions[transaction_count])
```

This represents the total number of transactions in the selected
period/filter context.

### Total Transaction Value

``` dax
Total Transaction Value =
SUM(Quarterly_Transactions[transaction_amount])
```

This represents the total monetary value processed.

### Average Transaction Value

``` dax
Average Transaction Value =
DIVIDE(
    [Total Transaction Value],
    [Total Transaction Volume]
)
```

This gives the average monetary value per transaction.

### Key Dashboard Visuals

-   Transaction volume over time
-   Transaction value over time
-   Transaction type contribution
-   Highest-volume quarter
-   Highest-value quarter
-   Transaction count vs value growth

------------------------------------------------------------------------

# Dashboard Page 2 --- Geographic Analysis

The geographic section focuses on state-level PhonePe activity.

### Visualizations

-   Top states by transaction volume
-   Top states by transaction value
-   State-wise transaction map
-   Transaction volume vs transaction value scatter plot
-   Geographic adoption over time

### High Volume / Low Value Analysis

A scatter plot is used with:

**X-axis:** Transaction Volume

**Y-axis:** Transaction Value

**Details:** State

This helps identify states where transaction frequency is high but the
average monetary value per transaction is comparatively lower.

------------------------------------------------------------------------

# Dashboard Page 3 --- Growth & Category Analysis

This page focuses on changes in transaction behavior.

### Key analyses

-   Transaction count growth
-   Transaction value growth
-   Category-wise growth
-   Average transaction value
-   Fastest-growing transaction categories

Comparing transaction count growth with transaction value growth helps
determine whether payment activity is increasing mainly because of more
transactions or because individual transactions are becoming larger.

------------------------------------------------------------------------

# Key KPIs

The dashboard focuses on the following KPIs:

  -----------------------------------------------------------------------
  KPI                                 Purpose
  ----------------------------------- -----------------------------------
  Total Transaction Volume            Measures overall payment activity

  Total Transaction Value             Measures monetary value processed

  Average Transaction Value           Measures average value per
                                      transaction

  Transaction Growth %                Measures change in transaction
                                      count

  Value Growth %                      Measures change in transaction
                                      value

  Top Transaction Category            Identifies the dominant payment
                                      category

  Highest Volume Quarter              Identifies peak transaction
                                      activity

  Highest Value Quarter               Identifies peak monetary activity

  Top State by Volume                 Identifies strongest transaction
                                      activity geographically

  Top State by Value                  Identifies strongest monetary
                                      contribution geographically
  -----------------------------------------------------------------------

------------------------------------------------------------------------

# Insights & Business Interpretation

The dashboard is designed to generate insights rather than only display
raw numbers.

### 1. Transaction activity

The time-series analysis shows how PhonePe transaction activity evolves
across quarters and years, allowing periods of acceleration, slowdown,
and peak activity to be identified.

### 2. Transaction value

Transaction value provides a complementary view to transaction count. A
period can have high transaction volume without necessarily having the
highest monetary value.

### 3. Dominant transaction categories

Category-wise analysis identifies which payment categories contribute
the largest share of transactions and helps distinguish high-frequency
categories from high-value categories.

### 4. Average transaction value

Average transaction value helps explain the relationship between
transaction count and transaction value.

A rising average value can indicate that transaction value is increasing
faster than transaction frequency, while a falling average can indicate
increasing transaction frequency with comparatively smaller transaction
sizes.

### 5. Geographic concentration

State-level analysis highlights where PhonePe activity is concentrated
and identifies the states contributing the largest transaction volumes
and values.

### 6. High-volume / low-value states

The scatter plot helps identify states where transaction frequency is
relatively high but monetary value is comparatively low. These regions
can represent strong everyday/digital payment usage with smaller average
transaction sizes.

### 7. Geographic adoption

Year and quarter filters allow the evolution of PhonePe adoption across
Indian states to be examined over time.

### 8. Growth opportunities

Transaction-category growth analysis highlights categories with strong
growth and can help identify areas of increasing digital payment
adoption.

------------------------------------------------------------------------

# Dashboard Interactivity

The Power BI report can be explored using filters/slicers such as:

-   Year
-   Quarter
-   Transaction Type
-   State

Selecting a filter dynamically updates the relevant KPIs and
visualizations.

This makes it possible to compare:

``` text
Year → Quarter → Transaction Category → State
```

and understand how payment behavior changes across dimensions.

------------------------------------------------------------------------

# Project Structure

A recommended repository structure is:

``` text
PhonePe-Pulse-Analytics/
│
├── data/
│   ├── raw/
│   │   └── phonepe_pulse/
│   │
│   └── processed/
│       └── phonepe_transactions.xlsx
│
├── notebooks/
│   └── phonepe_pulse_analysis.ipynb
│
├── powerbi/
│   └── PhonePe_Pulse_Dashboard.pbix
│
├── README.md
│
└── requirements.txt
```

------------------------------------------------------------------------

# Major Steps involved

### Step 1 --- Obtain the dataset

### Step 2 --- Place the raw data in the project directory

### Step 3 --- Run the Jupyter Notebook

### Step 4 --- Export the processed datasets

### Step 5 --- Open Power BI

### Step 6 --- Build/refresh the dashboard

------------------------------------------------------------------------

# Conclusion

This project demonstrates an end-to-end **Data Analytics and Business
Intelligence workflow** using a real-world digital payments dataset.

It combines:

**Data Collection → Python ETL → Pandas Analysis → Excel → Power BI →
DAX KPIs → Visualization → Business Insights**

The project provides a structured view of PhonePe's transaction trends,
category performance, growth patterns and geographic adoption across
India.

------------------------------------------------------------------------

## Author

**Rekhchand Sahu**\
**Final Year, NIT Raipur**

------------------------------------------------------------------------

## Project Highlights

-   Real-world digital payments dataset
-   Official PhonePe Pulse data source
-   Automated JSON-to-DataFrame ETL
-   Python and Jupyter Notebook analysis
-   Excel-based processed datasets
-   Power BI interactive dashboard
-   DAX-based KPIs
-   Time-series transaction analysis
-   Category-wise growth analysis
-   State-wise geographic analysis
-   Business-oriented insights
