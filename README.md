Bellabeat Case Study with SQL and R
================
Yuichiro Fukushi<br>12-22-2024

# 📚 Table of Contents

---

## **🏢 Company and Data Overview**  
- 🧑‍💼 [**Background Information**](#-background-information) – Overview of the company and its growth.
- 📊 [**Business Task**](#-business-task) – Overview of the main business problem.
- 📋 [**About the Data**](#-about-the-data) – Description of the dataset used in the analysis.
- ⚠️ [**Data Limitations**](#-data-limitations) – Limitations of the dataset in this case study.

---

## **🔧 Data Analysis**  
- 📋 [**Data Preparation**](#-data-preparation) – Preprocessing and cleaning the data.
- 🔍 [**Data Exploration**](#-data-exploration) – Identifying trends and key patterns.
- 📈 [**Data Visualization**](#-data-visualization) – Key visual insights and patterns.

---

## **💡 Final Insights**  
- 🎯 [**Recommendations**](#-recommendations) – Suggestions based on the analysis.

---

## **🏢 Company and Data Overview** 

### 🧑‍💼 Background Information

**Bellabeat** is a high-tech company founded by Urška Sršen and Sando Mur in 2013, specializing in health-focused smart products designed for women. Leveraging Sršen's background as an artist, Bellabeat has created beautifully designed technology that empowers women by providing insights into their health and habits, including activity, sleep, stress, and reproductive health data.

Since its inception, Bellabeat has grown rapidly, opening offices worldwide and launching a variety of products. The company has expanded its reach through online retailers and its own e-commerce platform. Bellabeat utilizes both traditional advertising methods and a strong digital marketing strategy, focusing on Google Search, social media engagement, and video ads to connect with consumers.

Bellabeat's marketing efforts are driven by data, and the company is keen to analyze user behavior and trends in smart device usage. Sršen has tasked the marketing analytics team with analyzing consumer data to uncover opportunities for growth and to inform future marketing strategies.

### 📊 Business Task

Sršen requests an analysis of smart device usage data to understand how consumers use non-Bellabeat devices. Based on these insights, she asks for a focus on one Bellabeat product in the presentation. This case study will address the following questions:

1.  What are some trends in smart device usage?
2.  How could these trends apply to Bellabeat customers?
3.  How can these trends help influence Bellabeat marketing strategy?

### 📋 About the Data

Sršen suggested using public data on smart device users' daily habits for analysis and directed me to a specific dataset: **[Fitbit Fitness Tracker Data](https://www.kaggle.com/datasets/arashnic/fitbit)**. This dataset, available on Kaggle, includes personal fitness tracker data from eligible Fitbit users who consented to share their data. It contains minute-level data on physical activity, heart rate, and sleep monitoring, offering insights into daily activity, steps, and heart rate to explore users' habits. 

I will be using this dataset to begin the data analysis process: first by cleaning the data using SQL, and then transferring the cleaned data to RStudio for deeper analysis and visualization.

### ⚠️ Data Limitations

Does this data **ROCCC** (a framework evaluating Reliability, Originality, Comprehensiveness, Currency, and Citation)?

- Reliability – The dataset's small sample size of only 30 participants is insufficient to accurately represent the broader population of female Fitbit users.
- Originality – The data is not original, as it was obtained from a third-party provider, Amazon Mechanical Turk, rather than through direct collection.
- Comprehensiveness – The dataset lacks critical demographic details such as gender, age, and health conditions. Its non-random nature and inherent bias further diminish its accuracy and objectivity.
- Currency – The data is outdated, having been collected seven years ago, making it unsuitable for analyzing current trends.
- Citation – The source is not well-documented, as the dataset is attributed to an unspecified third-party provider (Amazon Mechanical Turk).

Based on these factors, the dataset **DOES NOT** meet the ROCCC criteria.

---

## **🔧 Data Analysis** 

### 📋 Data Preparation

**1.** The datasets were downloaded in a zip file, from which 18 CSV files were extracted, each representing a separate dataset. For this analysis, the following eight datasets will be used:

- `dailyactivity_merged`
- `dailycalories_merged`
- `dailyIntensities_merged`
- `dailysteps_merged`
- `hourlyintensities_merged`
- `hourlycalories_merged`
- `hourlysteps_merged`
- `sleepday_merged`

**2.** I will begin by transferring the datasets to BigQuery to initiate the data cleaning process. The cleaning steps for each dataset are showned below:

- 2.a `dailyactivity_merged`:
  - I excluded the columns `LoggedActivitiesDistance` and `SedentaryActiveDistance` due to their limited entries, which made them irrelevant for the analysis. Out of 940 entries, `LoggedActivitiesDistance` had only 32 values, and `SedentaryActiveDistance` had 84. Additionally, I rounded all `FLOAT` data types to two decimal places to improve readability and maintain consistency.

```sql
SELECT 
  id,
  ActivityDate,
  TotalSteps,
  ROUND(TotalDistance, 2) AS TotalDistance,
  ROUND(TrackerDistance, 2) AS TrackerDistance,
  ROUND(VeryActiveDistance, 2) AS VeryActiveDistance,
  ROUND(ModeratelyActiveDistance, 2) AS ModeratelyActiveDistance,
  ROUND(LightActiveDistance, 2) AS LightActiveDistance,
  VeryActiveMinutes,
  FairlyActiveMinutes,
  SedentaryMinutes,
  Calories
FROM `verdant-legacy-441410-t2.FitBit_Fitness_Tracker_data.dailyactivity`
```

**Query Result:**
| Row | id          | ActivityDate | TotalSteps | TotalDistance | TrackerDistance | VeryActiveDistance | ModeratelyActiveDistance | LightActiveDistance | VeryActiveMinutes | FairlyActiveMinutes | SedentaryMinutes | Calories |
|-----|-------------|--------------|------------|---------------|-----------------|---------------------|--------------------------|--------------------|-------------------|---------------------|------------------|----------|
| 1   | 1624580081  | 2016-05-01   | 36019      | 28.03         | 28.03           | 21.92               | 4.19                     | 1.91               | 186               | 63                  | 1020             | 2690     |
| 2   | 1644430081  | 2016-04-14   | 11037      | 8.02          | 8.02            | 0.36                | 2.56                     | 5.1                | 5                 | 58                  | 1125             | 3226     |
| 3   | 1644430081  | 2016-04-19   | 11256      | 8.18          | 8.18            | 0.36                | 2.53                     | 5.3                | 5                 | 58                  | 1099             | 3300     |
| 4   | 1644430081  | 2016-04-28   | 9405       | 6.84          | 6.84            | 0.2                 | 2.32                     | 4.31               | 3                 | 53                  | 1157             | 3108     |
| 5   | 1644430081  | 2016-04-30   | 18213      | 13.24         | 13.24           | 0.63                | 3.14                     | 9.46               | 9                 | 71                  | 816              | 3846     |

***Note:*** The table above display only the first 5 rows for visualization purposes.

- 2.b `dailycalories_merged`, `dailyIntensities_merged`, and `dailysteps_merged`:
  - Since the information in `dailycalories_merged`, `dailyIntensities_merged`, and `dailysteps_merged` is already included in `dailyactivity_merged`, these datasets will be excluded from further processing. I used the `INNER JOIN` statement to check if the data matched based on user IDs and activity dates. Below is the query I executed:

```sql
SELECT 
  activity.calories,
  calories.calories
FROM `verdant-legacy-441410-t2.FitBit_Fitness_Tracker_data.dailyactivity` activity
INNER JOIN `verdant-legacy-441410-t2.FitBit_Fitness_Tracker_data.dailycalories` calories
ON activity.id = calories.id 
AND activity.ActivityDate = calories.ActivityDay -- Note: The ActivityDate column in the dailyactivity table is named ActivityDay in the dailycalories table
```

**Query Result:**
| Row | calories | calories_1 |
|-----|----------|------------|
| 1   | 2690     | 2690       |
| 2   | 3226     | 3226       |
| 3   | 3300     | 3300       |
| 4   | 3108     | 3108       |
| 5   | 3846     | 3846       |

***Note:*** The table above display only the first 5 rows for visualization purposes. The results from the selected columns across all 940 rows were consistent, indicating that the data is identical throughout.

```sql
SELECT 
  activity.SedentaryMinutes,
  activity.LightlyActiveMinutes,
  activity.FairlyActiveMinutes,
  activity.VeryActiveMinutes,
  intensities.SedentaryMinutes,
  intensities.LightlyActiveMinutes,
  intensities.FairlyActiveMinutes,
  intensities.VeryActiveMinutes
FROM `verdant-legacy-441410-t2.FitBit_Fitness_Tracker_data.dailyactivity` activity
INNER JOIN `verdant-legacy-441410-t2.FitBit_Fitness_Tracker_data.dailyintensities` intensities
ON activity.id = intensities.id 
AND activity.ActivityDate = intensities.ActivityDay -- Note: The ActivityDate column in the dailyactivity table is named ActivityDay in the dailyintensities table
```

**Query Result:**
| Row | SedentaryMinutes | LightlyActiveMinutes | FairlyActiveMinutes | VeryActiveMinutes | SedentaryMinutes_1 | LightlyActiveMinutes_1 | FairlyActiveMinutes_1 | VeryActiveMinutes_1  |
|-----|------------------|----------------------|---------------------|-------------------|--------------------|------------------------|-----------------------|----------------------|
| 1   | 950              | 174                  | 0                   | 0                 | 950                | 174                    | 0                     | 0                    |
| 2   | 774              | 206                  | 0                   | 0                 | 774                | 206                    | 0                     | 0                    |
| 3   | 1141             | 236                  | 2                   | 61                | 1141               | 236                    | 2                     | 61                   |
| 4   | 1131             | 217                  | 19                  | 73                | 1131               | 217                    | 19                    | 73                   |
| 5   | 708              | 214                  | 27                  | 53                | 708                | 214                    | 27                    | 53                   |

***Note:*** The table above display only the first 5 rows for visualization purposes. The results from the selected columns across all 940 rows were consistent, indicating that the data is identical throughout.

```sql
SELECT 
  activity.TotalSteps,
  steps.StepTotal
FROM `verdant-legacy-441410-t2.FitBit_Fitness_Tracker_data.dailyactivity` activity
INNER JOIN `verdant-legacy-441410-t2.FitBit_Fitness_Tracker_data.dailysteps` steps
ON activity.id = steps.id 
AND activity.ActivityDate = steps.ActivityDay -- Note: The ActivityDate column in the dailyactivity table is named ActivityDay in the dailysteps table
```

**Query Result:**
| Row | TotalSteps | StepTotal |
|-----|------------|-----------|
| 1   | 36019      | 36019     |
| 2   | 11037      | 11037     |
| 3   | 11256      | 11256     |
| 4   | 9405       | 9405      |
| 5   | 18213      | 18213     |

***Note:*** The table above display only the first 5 rows for visualization purposes. The results from the selected columns across all 940 rows were consistent, indicating that the data is identical throughout.

- 2.c - `hourlyintensities_merged`, `hourlycalories_merged`, and `hourlysteps_merged`:
  - For the datasets `hourlyintensities_merged`, `hourlycalories_merged`, and `hourlysteps_merged`, I addressed an issue with the `Activityhour` column, which contained both date and time in an AM/PM format unsupported by BigQuery. To resolve this, I used the `INT` function in Google Sheets to convert the date into an integer. This is necessary because Google Sheets stores dates as serial numbers, where the integer represents the number of days since a specific starting date. After converting the date into an integer, I used Google Sheets' `DATE` function to convert the integer back into a proper date format. For the time, I subtracted the `Activityhour` values from the newly separated `Activitydate` column and applied the `DATE` function to transform the result into a time format. I named this new column `Activityhour` and deleted the original `Activityhour` column to ensure the data was clean and avoid confusion. Before deleting the original columns, I used "Paste Special" to paste the newly separated date and time as values only, preventing any `#REF!` errors after the original columns were deleted. Below is the before and after transformation.

**Before:**
| 1   | Id         | ActivityHour           | Calories |
|-----|------------|------------------------|----------|
| 2   | 1503960366 | 4/12/2016 12:00:00 AM  | 81       |
| 3   | 1503960366 | 4/12/2016 1:00:00 AM   | 61       |
| 4   | 1503960366 | 4/12/2016 2:00:00 AM   | 59       |
| 5   | 1503960366 | 4/12/2016 3:00:00 AM   | 47       |
| 6   | 1503960366 | 4/12/2016 4:00:00 AM   | 48       |

| 1   | Id         | Activityday           | TotalIntensity | AverageIntensity |
|-----|------------|-----------------------|----------------|------------------|
| 2   | 1503960366 | 4/12/2016 12:00:00 AM | 20             | 0.33             |
| 3   | 1503960366 | 4/12/2016 1:00:00 AM  | 8              | 0.13             |
| 4   | 1503960366 | 4/12/2016 2:00:00 AM  | 7              | 0.12             |
| 5   | 1503960366 | 4/12/2016 3:00:00 AM  | 0              | 0                |
| 6   | 1503960366 | 4/12/2016 4:00:00 AM  | 0              | 0                |

| 1   | Id         | ActivityHour           | StepTotal |
|-----|------------|------------------------|-----------|
| 2   | 1503960366 | 4/12/2016 12:00:00 AM  | 373       |
| 3   | 1503960366 | 4/12/2016 1:00:00 AM   | 160       |
| 4   | 1503960366 | 4/12/2016 2:00:00 AM   | 151       |
| 5   | 1503960366 | 4/12/2016 3:00:00 AM   | 0         |
| 6   | 1503960366 | 4/12/2016 4:00:00 AM   | 0         |

**After:**
| 1   | Id         | ActivityDate | ActivityHour | Calories |
|-----|------------|--------------|--------------|----------|
| 2   | 1503960366 | 4/12/2016    | 0:00:00      | 81       |
| 3   | 1503960366 | 4/12/2016    | 1:00:00      | 61       |
| 4   | 1503960366 | 4/12/2016    | 2:00:00      | 59       |
| 5   | 1503960366 | 4/12/2016    | 3:00:00      | 47       |
| 6   | 1503960366 | 4/12/2016    | 4:00:00      | 48       |

| 1   | Id         | ActivityDate | ActivityHour | TotalIntensity | AverageIntensity |
|-----|------------|--------------|--------------|----------------|------------------|
| 2   | 1503960366 | 4/12/2016    | 0:00:00      | 20             | 0.33             |
| 3   | 1503960366 | 4/12/2016    | 1:00:00      | 8              | 0.13             |
| 4   | 1503960366 | 4/12/2016    | 2:00:00      | 7              | 0.12             |
| 5   | 1503960366 | 4/12/2016    | 3:00:00      | 0              | 0.00             |
| 6   | 1503960366 | 4/12/2016    | 4:00:00      | 0              | 0.00             |

| 1   | Id         | ActivityDate | ActivityHour | StepTotal |
|-----|------------|--------------|--------------|-----------|
| 2   | 1503960366 | 4/12/2016    | 0:00:00      | 373       |
| 3   | 1503960366 | 4/12/2016    | 1:00:00      | 160       |
| 4   | 1503960366 | 4/12/2016    | 2:00:00      | 151       |
| 5   | 1503960366 | 4/12/2016    | 3:00:00      | 0         |
| 6   | 1503960366 | 4/12/2016    | 4:00:00      | 0         |

***Note:*** The table above display only the first 5 rows for visualization purposes.

- 2.d `sleepday_merged`:
  - I excluded the column TotalSleepRecords from the analysis as it was not relevant to the study I am conducting.

```sql
SELECT 
  Id,
  SleepDay,
  TotalMinutesAsleep,
  TotalTimeInBed
FROM `verdant-legacy-441410-t2.FitBit_Fitness_Tracker.sleepday`
```
| Row | Id        | SleepDay   | TotalSleepRecords | TotalMinutesAsleep | TotalTimeInBed |
|-----|-----------|------------|-------------------|--------------------|----------------|
| 1   | 1503960366| 2016-04-12 | 1                 | 327                | 346            |
| 2   | 1503960366| 2016-04-15 | 1                 | 412                | 442            |
| 3   | 1503960366| 2016-04-17 | 1                 | 700                | 712            |
| 4   | 1503960366| 2016-04-19 | 1                 | 304                | 320            |
| 5   | 1503960366| 2016-04-20 | 1                 | 360                | 377            |

***Note:*** The table above display only the first 5 rows for visualization purposes.

### 🔍 Data Exploration

1. Now that I've cleaned the data needed for analysis, I will load the datasets (in CSV format) into RStudio, along with the necessary packages, to begin the exploration process.

```r
# Load necessary packages
suppressPackageStartupMessages(library(tidyverse))

# Load datasets
daily_activity <- read_csv("dailyactivity.csv")
hourly_intensities <- read_csv("hourlyintensities.csv")
hourly_calories <- read_csv("hourlycalories.csv")
hourly_steps <- read_csv("hourlysteps.csv")
sleep_day <- read_csv("sleepday.csv")
```

2. Let's start with the `dailyactivity` dataset. I want to know how many users are 
