# Analyzing Power Outages
**Project for DSC 80 at UCSD**

By [Your Name]

---

## Introduction
This project investigates a dataset of major power outages in the U.S. from January 2000 to July 2016. These outages, as defined by the Department of Energy, impacted at least 50,000 customers or caused an unplanned energy demand loss of at least 300 megawatts. The dataset includes geographical, climatic, economic, and land-use data about the states affected.

### Research Question
The primary question explored is:
**What are the key factors influencing outage duration and its severity, and how can we predict the cause of an outage?**

Understanding these factors can help energy companies prepare for outages caused by weather, infrastructure failures, or intentional attacks.

### Dataset Details
The original dataset contains **1534 rows** and **57 columns**, with a focus on the following key features:

| **Column**             | **Description**                                                                                      |
|------------------------|----------------------------------------------------------------------------------------------------|
| `YEAR`                | Year an outage occurred                                                                           |
| `MONTH`               | Month an outage occurred                                                                          |
| `U.S._STATE`          | State where the outage occurred                                                                   |
| `NERC.REGION`         | North American Electric Reliability Corporation (NERC) regions involved in the outage            |
| `CLIMATE.REGION`      | U.S. Climate regions as defined by the National Centers for Environmental Information              |
| `CAUSE.CATEGORY`      | Categories describing the cause of the outage                                                     |
| `OUTAGE.DURATION`     | Duration of the outage (in minutes)                                                               |
| `CUSTOMERS.AFFECTED`  | Number of customers affected                                                                      |
| `DEMAND.LOSS.MW`      | Amount of energy demand lost during the outage (in megawatts)                                     |

---

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning
The dataset was cleaned using the following steps:
1. **Column Selection:** Irrelevant columns were dropped, focusing on features listed above.
2. **Date-Time Conversion:** Combined `OUTAGE.START.DATE` and `OUTAGE.START.TIME` into a single column. Similarly, combined restoration date and time.
3. **Handling Missing Values:** Zero values in `OUTAGE.DURATION`, `CUSTOMERS.AFFECTED`, and `DEMAND.LOSS.MW` were replaced with `NaN` since these represent missing data.
4. **Urban Feature Engineering:** Combined urban population statistics (`POPPCT_URBAN`, `POPDEN_URBAN`, and `AREAPCT_URBAN`) into a single column representing urban characteristics.

### Exploratory Data Analysis

#### Univariate Analysis
- **Outage Duration:** Most outages lasted under 2000 minutes, but a few extreme cases exceeded 3000 minutes.
- **Cause Categories:** Severe weather accounted for the majority of outages.

#### Bivariate Analysis
1. **Outage Duration vs. Customers Affected:** 
   - Positive correlation, though some short outages affected many customers.
2. **Outage Duration vs. Cause Category:** 
   - Severe weather caused longer outages than intentional attacks.

#### Grouping and Aggregates
- **By NERC Region:** Grouped data showed average outage durations and customers affected by region.
- **By Climate Region and Cause:** Pivot table revealed severe weather as the primary cause in most regions.

---

## Assessment of Missingness

### NMAR Analysis
`CUSTOMERS.AFFECTED` is likely **Not Missing at Random (NMAR)** because its missingness depends on whether reporting agencies provided this information during the outages.

### Dependency Analysis
1. **Cause Category:**
   - Observed Total Variation Distance (TVD): **0.469**
   - P-value: **0.0**
   - Conclusion: Missingness in `OUTAGE.DURATION` depends on `CAUSE.CATEGORY`.

2. **Month:**
   - Observed TVD: **0.143**
   - P-value: **0.176**
   - Conclusion: Missingness in `OUTAGE.DURATION` does not depend on `MONTH`.

---

## Hypothesis Testing
### Hypothesis
- **Null Hypothesis:** The mean duration of severe weather outages equals intentional attack outages.
- **Alternative Hypothesis:** Severe weather outages have a longer mean duration.

### Results
- Observed difference in means: [Add value].
- P-value: **0.0**
- Conclusion: Severe weather outages last significantly longer than intentional attack outages.

---

## Framing a Prediction Problem

### Problem Statement
This project predicts the **cause of a power outage** as a binary classification problem: severe weather vs. intentional attacks.

### Features
- `NERC.REGION`, `CLIMATE.REGION`, `YEAR`, `MONTH`, `TOTAL.PRICE`, `TOTAL.SALES`, `URBAN`

### Metric
The **F1 Score** accounts for class imbalance.

---

## Baseline Model
### Features
- `NERC.REGION`, `ANOMALY.LEVEL`, `YEAR`, `URBAN`
### Performance
- F1 Score: **0.76**

---

## Final Model
### Features
- Added: `CLIMATE.REGION`, `MONTH`, `TOTAL.PRICE`, `TOTAL.SALES`, `TOTAL.CUSTOMERS`
- Algorithm: Decision Tree Classifier
- Tuning: GridSearchCV
- Best Parameters:
  - `criterion`: `entropy`
  - `max_depth`: 10
  - `min_samples_split`: 5
- F1 Score: **0.89**

---

## Fairness Analysis
### Groups
- Group X: Outages >3000 minutes
- Group Y: Outages ≤3000 minutes

### Results
- P-value: **0.01**
- Conclusion: Model performance significantly differs across groups.

---

This README file is structured for clear communication of analysis, methods, and findings while aligning with the structure provided in your Jupyter notebook. You can copy and paste this directly into your README file. Let me know if you need further refinements!

