# Loan Default Analysis Dashboard (Power BI)

### Dashboard Link : https://app.powerbi.com/groups/87c60d52-30e2-4207-baf1-db822b2b4d61/reports/0aeb12df-a8d0-4f59-bcab-d35d7d193569/422f62610036194b0301?experience=power-bi

## Problem Statement

Banks lose money when borrowers default. This project analyses ~2.55 lakh (255,347) loan records to help bank officials decide **who to lend to, how much, and on what terms**. The dashboard links borrower demographics (age, income, education, employment, marital status) and financial profile (credit score, loan amount, DTI, mortgage, dependents) with default behaviour, so risk can be spotted early and lending decisions become data-driven.

## Business Questions Answered

- Which loan purposes account for the largest loan amounts?
- Which employment types have the highest default rate and the highest average income?
- How do loan amounts and defaults vary by age group, credit score band, education and marital status?
- How is the default rate trending year over year?
- How does loan volume change year on year, and what is the year-to-date (YTD) position by credit band and marital status?
- Which income bracket / employment type drives most of the loan amount?

## Tech Stack & Architecture

`Excel (source)` → `Microsoft SQL Server (database: loan)` → `Power BI Dataflow Gen1 (Service)` → `Power BI Desktop (model, DAX, report)` → `Published to Power BI Service`

## Dataset

Borrower-level loan data: Loan ID, Age, Income, Loan Amount, Credit Score, Months Employed, Number of Credit Lines, Interest Rate, Loan Term, DTI Ratio, Education, Employment Type, Marital Status, Has Mortgage, Has Dependents, Loan Purpose, Has Cosigner, **Default (target)**, Loan Date.

## Steps Followed

- **Step 1 : Data loading** – Imported the Excel file into SQL Server (`loan` database) using the Import Flat File wizard.
- **Step 2 : Dataflow** – Created a Dataflow Gen1 in Power BI Service connected to SQL Server so cleaned, shared data is available centrally to all analysts.
- **Step 3 : Connect to Desktop** – Loaded the table in Power BI Desktop via *Get Data → Dataflows*.
- **Step 4 : Data profiling & cleaning** – Enabled column quality / distribution / profile on the entire dataset. No errors or blanks found; verified data types and date format.
- **Step 5 : Calculated columns (DAX)**
  - `Year` – extracted from Loan Date
  - `Age Group` – nested `IF` (Teen, Adults, Middle Age Adults, Senior Citizens)
  - `Credit Score Bins` – credit score bands
  - `Income Bracket` – `SWITCH` (Low < 30K, Medium 30K–60K, High ≥ 60K)
- **Step 6 : Measures (DAX)** – Created separate measure tables per page. Functions used: `SUMX`, `FILTER`, `ISBLANK`, `CALCULATE`, `AVERAGE`, `AVERAGEX`, `VALUES`, `MEDIANX`, `ALL`, `ALLEXCEPT`, `COUNTROWS`, `DIVIDE`, `DATESYTD`, `YEAR`, `MAX`, variables.
  - Loan Amount by Purpose
  - Average Income by Employment Type
  - Default Rate by Employment Type (%)
  - Average Loan by Age Group
  - Default Rate by Year (%)
  - Median Loan by Credit Score Bins
  - Total Loan (Adults) by Credit Bins
  - YoY Loan Amount Change
  - YTD Loan Amount
- **Step 7 : Report design** – Built a 3-page report with consistent theme, header shapes, formatted line/area charts, donut, clustered column, ribbon chart and decomposition tree.
- **Step 8 : Data validation** – Cross-checked every key visual three ways: (a) a plain table visual without DAX, (b) Excel pivot tables on the original source, (c) manual median calculation in Power Query (index column method). Numbers matched.
- **Step 9 : Publish & refresh** – Published to Power BI Service, configured scheduled refresh, and explored incremental refresh (converted date to datetime; needs Premium capacity).

## Report Pages

1. **Loan Default – Overview** – Loan amount by purpose, average income by employment type, default rate by employment type, average loan by age group, default rate by year.
2. **Applicant Demographics & Financial Profile** – Median loan by credit score category, average loan by age group & marital status (donut), total loan (adults) by credit score bin, loan for middle-age adults by mortgage/dependents, loans by education type.
3. **Financial Risk Matrix** – YoY loan amount change, YoY default/outstanding change, YTD loan amount by credit score bin & marital status (ribbon), decomposition tree by income bracket and employment type.

## Dashboard Snapshots

### Page 1: Loan Default Overview
![Loan Default Overview](https://github.com/user-attachments/assets/f3cecdb6-5c4c-47c9-8abd-156ea8fa6021)

### Page 2: Applicant Demographics & Financial Profile
![Applicant Demographics](https://github.com/user-attachments/assets/ee7394bc-4851-46fd-ac89-90f347778df5)

### Page 3: Financial Risk Matrix
![Financial Risk Matrix](https://github.com/user-attachments/assets/62a00514-f211-4b25-8043-2c917155a9da)

## Key Insights

- Dataset: **255,347** loan records, borrower ages **18–69**, loan years **2013–2018**.
- Median loan amount is **~127,556**.
- Default rate by employment type is low overall (e.g., Unemployed ~3.39%, Part-time ~3.01%)
- Lending is concentrated in high-income borrowers (about two-thirds) and in Medium and High credit bins.
- The overall default rate is stable at about 11.6%, with a small peak in 2016.
- Unemployed and Part-time borrowers have the highest default share, and Full-time the lowest.
- Most other splits are nearly flat (purpose, income, marital status, mortgage, dependents).

## Recommendations (Business Value)

- Tighten lending criteria for segments with consistently higher default rates.
- Use credit-score band and income bracket together for loan limits and pricing.
- Monitor YoY and YTD trends to catch rising risk early.
- Use the shared dataflow as a single source of truth for the analyst team.

## Key Learnings

DAX filter-context control (`ALL` vs `ALLEXCEPT`), time intelligence (YoY, YTD), dataflows, scheduled & incremental refresh, and a systematic data-validation approach.
