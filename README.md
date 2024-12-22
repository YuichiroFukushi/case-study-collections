Bellabeat Case Study with SQL and R
================
Yuichiro Fukushi<br>12-22-2024

# 📚 Table of Contents

---

## **🏢 Company and Data Overview**  
- 🧑‍💼 [**Background Information**](#-background-information) – Overview of the company and its growth.
- 📊 [**Business Task**](#-business-task) – Overview of the main business problem.
- 📋 [**About the Data**](#-about-the-data) – Description of the dataset used in the analysis.
- ⚠️ [**Data Limitations**](#-data-limitations) – Constraints and challenges faced in the analysis.

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

**1.** The datasets were downloaded in a zip file, from which 18 CSV files were extracted, each representing a separate dataset. For this analysis, the following four datasets will be used:

- `dailyactivity_merged`
- `dailycalories_merged`
- `dailyIntensities_merged`
- `dailysteps_merged`
- `sleepday_merged`
- `weightLogInfo_merged`
- `dailysteps_merged`
- `dailysteps_merged`
- `dailysteps_merged`


**2.** I will begin by transferring the datasets to BigQuery to initiate the data cleaning process. The cleaning steps for each dataset are outlined below:

- 2.a `dailycalories_merged`, `dailyIntensities_merged`, and `dailysteps_merged`:
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

### Query Result:
| Row | calories | calories_1 |
|-----|----------|------------|
| 1   | 2690     | 2690       |
| 2   | 3226     | 3226       |
| 3   | 3300     | 3300       |
| 4   | 3108     | 3108       |
| 5   | 3846     | 3846       |

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

### Query Result:
| Row | SedentaryMinutes | LightlyActiveMinutes | FairlyActiveMinutes | VeryActiveMinutes | SedentaryMinutes_1 | LightlyActiveMinutes_1 | FairlyActiveMinutes_1 | VeryActiveMinutes_1 |
|-----|------------------|----------------------|---------------------|-------------------|--------------------|------------------------|-----------------------|----------------------|
| 1   | 950              | 174                  | 0                   | 0                 | 950                | 174                    | 0                     | 0                    |
| 2   | 774              | 206                  | 0                   | 0                 | 774                | 206                    | 0                     | 0                    |
| 3   | 1141             | 236                  | 2                   | 61                | 1141               | 236                    | 2                     | 61                   |
| 4   | 1131             | 217                  | 19                  | 73                | 1131               | 217                    | 19                    | 73                   |
| 5   | 708              | 214                  | 27                  | 53                | 708                | 214                    | 27                    | 53                   |

```sql
SELECT 
  activity.TotalSteps,
  steps.StepTotal
FROM `verdant-legacy-441410-t2.FitBit_Fitness_Tracker_data.dailyactivity` activity
INNER JOIN `verdant-legacy-441410-t2.FitBit_Fitness_Tracker_data.dailysteps` steps
ON activity.id = steps.id 
AND activity.ActivityDate = steps.ActivityDay -- Note: The ActivityDate column in the dailyactivity table is named ActivityDay in the dailysteps table
```

### Query Result:
| Row | TotalSteps | StepTotal |
|-----|------------|-----------|
| 1   | 36019      | 36019     |
| 2   | 11037      | 11037     |
| 3   | 11256      | 11256     |
| 4   | 9405       | 9405      |
| 5   | 18213      | 18213     |

<span style="color: green;">**Note:** The tables above display only the first 5 rows for visualization purposes. The results from the selected columns across all 940 rows were consistent, indicating that the data is identical throughout.</span>



