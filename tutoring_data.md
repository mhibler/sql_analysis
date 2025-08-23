# Tutoring Website Data Analysis SQL Queries

## Introduction
This file contains all SQL queries used to analyze activity on a tutoring platform.     
Goal: Allocate tutors efficiently to ensure adequate support while minimizing costs.   
Questions: What grade level and math courses see the most activity?
## Data Loading 
Creates a table to then import the sample data set into.
```sql
CREATE TABLE tutoring_data (
    User_ID UUID PRIMARY KEY,
    Age_in_Months INT,
    Gender VARCHAR(50),
    Location VARCHAR(100),
    Grade VARCHAR(50),
    Logins_per_Month INT,
    Days_Completed_Activity INT,
    Exercises_Started NUMERIC(10,6),
    Total_Time_Spent_In_Minutes NUMERIC(10,6),
    Course_Name VARCHAR(100),
    Course_Category VARCHAR(50),
    Completion_Rate NUMERIC(5,2),
    Average_Score NUMERIC(5,2),
    Course_Rating INT,
    Recommendation_Likelihood INT,
    Exercises_Completed INT,
    Points_Earned NUMERIC(10,2),
    Subscription_Tier VARCHAR(50),
    Subscription_Cost NUMERIC(10,2),
    Subscription_Length_in_Months INT,
    Renewal_Status BOOLEAN,
    Tutoring BOOLEAN,
    Referrals INT,
    Academic_Grade VARCHAR(2)
);
```
The csv file contianing the data was imported into pgadmin.
Testing to ensure data was properly imported into the table.
```sql
Select * FROM tutoring_data
    LIMIT 10;
```

## Data Cleaning
Creating a temporary table with data relevant to question being asked, ensuring the data does not contain any NULL values. 
```sql
DROP table tutoring_clean;
CREATE TEMP TABLE tutoring_clean AS
SELECT user_id, age_in_months, grade, logins_per_month, total_time_spent_in_minutes, course_name 
FROM tutoring_data 
WHERE course_category = 'Math'
	AND user_id IS NOT NULL
	AND age_in_months IS NOT NULL
	AND grade IS NOT NULL
	AND logins_per_month IS NOT NULL
    AND total_time_spent_in_minutes IS NOT NULL 
	AND course_name IS NOT NULL;
```
## Data Transformation
Aggreagting the data to examine total logins and total time.
Grouping the data by grade level
```sql
SELECT grade, SUM(logins_per_month) AS total_logins, SUM(total_time_spent_in_minutes) AS total_time
	FROM tutoring_clean 
	GROUP BY grade
	ORDER BY total_logins DESC, total_time DESC;
```
Grouping the data by course name
```sql 
SELECT course_name, SUM(logins_per_month) AS total_logins, SUM(total_time_spent_in_minutes) AS total_time
	FROM tutoring_clean 
	GROUP BY course_name
	ORDER BY total_logins DESC, total_time DESC;
```

