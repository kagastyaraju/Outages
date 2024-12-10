# Analyzing Power Outages
**Project for DSC 80 at UCSD**

By [Kaushik Agastyaraju]

---

## Introduction
Welcome! My name is Kaushik Agastyaraju and I recently completed analyis on a dataset of power outages. The following will be an explanation of my processes and analysis.

This project was mean to investigates a dataset of major power outages in the U.S. that took place from January 2000 to July 2016. These power outages, as defined by the Department of Energy, were included in the dataset if they impacted at least 50,000 customers or caused an unplanned energy demand loss of at least 300 megawatts. The dataset includes many features of data including: geographical, climatic, economic, and land-use data about the states affected. And by conducting open ended analysis I attempted to uncover a very small subset of the trends buried within this dataset. 

Uplpading and understanding the data was the first step in this process. I downloaded the data from Purdue University’s Laboratory for Advancing Sustainable Critical Infrastructure, at https://engineering.purdue.edu/LASCI/research-data/outages.

From there I took steps to clean the dataset which would then allow me to perform exploratory data analysis, first through varaible analsysis, then through variable missingness and finally through statistical testing. After loading in the data and observing the features of power outages I decided on my main research question for this analysis:

### Research Question
The primary question explored is:
**What are the key factors influencing outage duration and its severity, and how can we predict the cause of an outage?**

Understanding how long power outages last is essential for helping communities recover and build resilience. The length of an outage doesn’t just show how severe the issue was—it also reveals how quickly and efficiently power is restored. Longer outages can have ripple effects, disrupting vital services like healthcare and communication, impacting local economies, and even putting public safety at risk. By examining what causes these delays, energy companies can uncover key challenges, such as aging infrastructure, delays in deploying repair teams, or the sheer size of the incident. This understanding can guide better planning, ensuring resources are in place to respond faster and reduce the impact on everyday life.


### Dataset Details

The dataset contains **1534 rows** and focuses on the following relevant columns for analysis:

| **Column**               | **Description**                                                                                           | **Data Type**       |
|---------------------------|-----------------------------------------------------------------------------------------------------------|---------------------|
| `YEAR`                   | Year when the outage occurred                                                                             | Integer            |
| `MONTH`                  | Month when the outage occurred                                                                            | Integer            |
| `U.S._STATE`             | State where the outage occurred                                                                           | String             |
| `NERC.REGION`            | North American Electric Reliability Corporation (NERC) region involved in the outage                     | String             |
| `CLIMATE.REGION`         | U.S. Climate regions as specified by the National Centers for Environmental Information                   | String             |
| `ANOMALY.LEVEL`          | Oceanic El Niño/La Niña (ONI) index for the cold and warm episodes by season                              | Float              |
| `CLIMATE.CATEGORY`       | Classification of the climate episode as "Warm," "Cold," or "Normal" based on ONI threshold              | String             |
| `OUTAGE.START.DATE`      | Day of the year when the outage started                                                                   | Date               |
| `OUTAGE.START.TIME`      | Time of day when the outage started                                                                       | Time               |
| `OUTAGE.RESTORATION.DATE`| Day of the year when power was restored                                                                   | Date               |
| `OUTAGE.RESTORATION.TIME`| Time of day when power was restored                                                                       | Time               |
| `OUTAGE.DURATION`        | Duration of the outage (in minutes)                                                                       | Integer/Float      |
| `CAUSE.CATEGORY`         | Broad category of the cause of the outage                                                                 | String             |
| `CAUSE.CATEGORY.DETAIL`  | Detailed description of the specific cause of the outage                                                  | String             |
| `HURRICANE.NAMES`        | Name of the hurricane (if the outage was hurricane-related)                                               | String (Nullable)  |
| `DEMAND.LOSS.MW`         | Amount of peak demand lost during the outage (in megawatts)                                               | Float              |
| `CUSTOMERS.AFFECTED`     | Number of customers affected by the outage                                                                | Integer            |
| `POPULATION`             | Population in the state where the outage occurred                                                        | Integer            |
| `POPPCT_URBAN`           | Percentage of the state's population living in urban areas                                                | Float              |
| `POPDEN_URBAN`           | Population density of urban areas (persons per square mile)                                               | Float              |
| `POPDEN_RURAL`           | Population density of rural areas (persons per square mile)                                               | Float              |
| `TOTAL.PRICE`            | Average monthly electricity price in the state (cents/kilowatt-hour)                                      | Float              |
| `RES.PRICE`              | Monthly electricity price in the residential sector (cents/kilowatt-hour)                                 | Float              |
| `TOTAL.SALES`            | Total electricity consumption in the state (megawatt-hour)                                                | Float              |
| `RES.SALES`              | Electricity consumption in the residential sector (megawatt-hour)                                         | Float              |
| `IND.SALES`              | Electricity consumption in the industrial sector (megawatt-hour)                                          | Float              |

This  selection of columns ensures focus on essential data for understanding and analyzing power outages, their duration, and their impacts across the U.S.


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

<iframe
  src="assets/outage_duration_distribution.html"
  width="800"
  height="650"
  frameborder="0"
></iframe>


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

