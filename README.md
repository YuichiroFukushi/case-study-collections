Bellabeat Case Study with SQL and R
================
Yuichiro Fukushi<br>12-22-2024

# 📚 Case Study Table of Contents

---

## **Company and Data Overview**  
- 🧑‍💼 [**Background Information**](#background-information) – Overview of the company and its growth.
- 📊 [**Business Task**](#business-task) – Overview of the main business problem.
- 📋 [**About the Data**](#about-the-data) – Description of the dataset used in the analysis.
- ⚠️ [**Limitations**](#limitations) – Constraints and challenges faced in the analysis.

---

## **🔧 Data Analysis**
- 📋 [**Data Preparation**](#data-preparation) – Preprocessing and cleaning the data.
- 🔍 [**Data Exploration**](#data-exploration) – Identifying trends and key patterns.
- 📈 [**Data Visualization**](#data-visualization) – Key visual insights and patterns.

---

## **💡 Final Insights**
- 🎯 [**Recommendations**](#recommendations) – Suggestions based on the analysis.

---

## 🧑‍💼 Background Information

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

### ⚠️ Limitations

- The dataset is limited as it lacks demographic details such as gender, age, and location, which are crucial for creating targeted marketing strategies.
- The sample size of 30 participants is too small to represent all Fitbit users effectively.
- The data is outdated, covering only a one-month period in 2016. For a more accurate analysis of trends, data from the current year and spanning an entire year would be preferable to observe seasonal variations.

---




