# Analyzing Power Outages
**Project for DSC 80 at UCSD**

By Kaushik Agastyaraju

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
 - Exploratory data analysis of the outages dataset allowed me to better understand the data I was working with: Understand the features and thier relations. These steps would allow me to stage more complex missingness and modeling questions later in the project

#### Univariate Analysis - Exploring single Variables

My univariatne analysis began with looking at the distribution of outages durations to understand the frequency of outage durations in the dataset

<iframe
  src="assets/outage_duration_distribution.html"
  width="600"
  height="450"
  frameborder="0"
></iframe>

- **Outage Duration:** A majority of outages lasted under 5000 minutes, less than 250 happened between 5000 and 20000, but a few extreme cases exceeded 3000 minutes.


From there I looked at the distribution of customers affected. This was to understand the frequency of customers affected for each otuages across the dataset

<iframe
  src="assets/customers_affected_distribution.html"
  width="600"
  height="450"
  frameborder="0"
></iframe>

- **Customers Affected:** The distribution of customers affected is heavily skewed right, meaning the majority outages affected less than 1 million people, with a few cases affecting more

#### Bivariate Analysis
1. **Outage Duration vs. Customers Affected:** 
   - Positive correlation, though some short outages affected many customers.` 
   
<iframe
  src="assets/scatter_outage_duration_vs_customers.html"
  width="600"
  height="450"
  frameborder="0"
></iframe>


2. **Outage Duration vs. Cause Category:** 
   - Severe weather caused longer outages than intentional attacks.

<iframe
  src="assets/scatter_outage_duration_by_cause.html"
  width="600"
  height="450"
  frameborder="0"
></iframe>

   

#### Grouping and Aggregates
- **By NERC Region:** Grouping the data by climate regeion allowed me to look at the sum and average of customers affect by each region.

<iframe
  src="assets/climate_agg_outages.html"
  width="100%"
  height="400"
  frameborder="0"
></iframe>

- **By Climate Region and Cause:** Creating a pivot table and grouping by cause category and climate region further grouped the data allowing me to look at primary causes of outages based on region which can reveal issues within any specific region.

<iframe
  src="assets/pivot_duration.html"
  width="100%"
  height="400"
  frameborder="0"
></iframe>
---

## Assessment of Missingness

### NMAR Analysis
`OUTAGE.DURATION` is likely **Not Missing at Random (NMAR)** because its missingness depends on whether reporting agencies provided this information during the outages.

### Dependency Analysis

Analyzing the dependency of missing values in the duration column provides valuable insights for better data imputation strategies. These dependencies can help identify patterns or relationships that contribute to missing data, enabling more informed and accurate imputation methods.

To better understand the missingness dependecy of the duration column I will test with cause category and customers affected. 

1. **Cause Category:**

The first step in missingness dependecy is to look at the distribution of cause category given the missingness of duration. similiarites in these bars may indiicate a dependency. 

<iframe
  src="assets/cause_category_vs_duration_missing.html"
  width="100%"
  height="400"
  frameborder="0"
></iframe>

**Null Hypothesis:** The distribution of cause category is the same when Duration is missing vs not missing

**Alternate Hypothesis:** The distribution of cause category is the different when Duration is missing vs not missing


<iframe
  src="assets/permutation_tvd_distribution.html"
  width="600"
  height="450"
  frameborder="0"
></iframe>

   - Observed Total Variation Distance (TVD): **0.469**
   - P-value: **0.0**
   - Conclusion: This p-value indicates rejection of the null hypothesis in favor of the alterhante meaning Missingness in `OUTAGE.DURATION` liekley depends on `CAUSE.CATEGORY`.


2. **Customers Affected**

   Here we look at a violin plot showing the distribution of Customers Affected given the missingness of duration. 

<iframe
  src="assets/customers_affected_violinplot.html"
  width="100%"
  height="400"
  frameborder="0"
></iframe>

**Null Hypothesis:** The distribution of customers affected is the same when Duration is missing vs not missing

**Alternate Hypothesis:** The distribution of customers affected is different when Duration is missing vs not missing


<iframe
  src="assets/permutation_mean_difference.html"
  width="600"
  height="450"
  frameborder="0"
></iframe>

   - Observed Difference in Means: **-7685.716**
   - P-value: **0.8980**
   - Conclusion: At This p-value we fail to reject the null hypothesis in favor of the alterhante meaning the distribtuion is not very different wehn duration is missing, meaning there is likely no dependency between the missingness of Duration and the Customers Affected

---

### Hypothesis Test: Outage Duration and Residential Electricity Prices

For this section I will be testing whetere outage duration differs between low and high residiantial price areas.

#### Hypotheses:
- **Null**: The mean outage duration is the same for low and high residential electricity price areas.
- **Alternative**: The mean outage duration differs between low and high residential electricity price areas.

#### Test Statistic and Significance Level:
- **Test Statistic**: I chose the difference in means because it directly compares the groups of interest.
- **Significance Level**: I used a standard significance level of 0.05.

#### Results:
- **Observed Difference**: \( 397.914 \) minutes.
- **P-value**: \( 0.1970 \).

#### Conclusion:
Since the p-value 0.1970  is greater than  0.05, I fail to reject the null hypothesis. There isn’t enough evidence to conclude that outage durations differ significantly between low and high residential electricity price areas. The observed difference could simply be due to random chance.

#### Visualization:
Below is a histogram showing the empirical distribution of permutation differences. The dashed red line represents the observed difference.

<iframe
  src="assets/permutation_diff_price_group.html"
  width="600"
  height="450"
  frameborder="0"
></iframe>



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

