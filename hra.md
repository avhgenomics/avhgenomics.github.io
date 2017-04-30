---
layout: page
title: Human Resources Analytics
subtitle: Data Exploration of the Kaggle HRA dataset
bigimg: /img/path.jpg
published: no
---

This is [Kaggle's Human Resources Analytics Dataset](https://www.kaggle.com/ludobenistant/hr-analytics).  
If you are unfamiliar with Kaggle, it is an excellent site for learning Data Science:  

* They provide open datasets, real and simulated.  
* Competitions are sponsored for these projects with awards / prizes for best performance.  
* The community posts their code (good for reproducibility and **for learning new methods**).  

# Background of the Dataset

## Description of the Variables

* satisfaction_level,Level of satisfaction (0-1),Numeric
* last_evaluation,Evaluation of employee performance (0-1),Numeric
* number_project,Number of projects completed while at work,Numeric
* average_montly_hours,Average monthly hours at workplace,Numeric
* time_spend_company,Number of years spent in the company,Numeric
* Work_accident,Whether the employee had a workplace accident,Numeric
* left,Whether the employee left the workplace or not (1 or 0) Factor,Numeric
* promotion_last_5years,Whether the employee was promoted in the last five years,Numeric
* sales,Department in which they work for,String
* salary,Relative level of salary (high),String

## State of the Data (Size, Completeness)

This data contains:

* 14,999 observations (rows)  
* 10 Features (variables / columns)  
* NA or Infs present: 0  

This data, as downloaded, is a complete dataset with no missing values.

# Exploratory Phase

Getting to know the data set.

## View of the Company Population

![Workforce Histogram](\img\data\hra\sales_histo.png)

![Workforce Histogram](\img\data\hra\left_by_sales_histo.png)
