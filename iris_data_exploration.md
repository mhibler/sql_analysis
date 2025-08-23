# Iris Species Exploratory Data Analysis With SQL


## Introduction
The Iris dataset contains four measurements from iris flowers: sepal length, sepal width, petal length, and petal width, along with the species label (Setosa, Versicolor, Virginica).  
**Goal:**  Use SQL to explore the data, summarize key features, and draw insights about differences across species.
**Questions:**
1) Which measurements are strongly correlated?
2) How do measurements vary by species?

## Data Loading
The dataset was imported into PostgreSQL using a csv file.
```sql
DROP TABLE IF EXISTS Public.iris;
--creating the table
CREATE TABLE Public."iris" (sepal_length REAL, sepal_width REAL, petal_length REAL, petal_width REAL, species VARCHAR(100));
--copying the data into the table
COPY iris (sepal_length, sepal_width, petal_length, petal_width, species ) FROM 'C:\Users\thehi\Downloads\iris_data.csv' DELIMITER ',' CSV HEADER;
--displaying table to ensure the data was imported properly
SELECT * FROM iris;
```
## Exploring the Data
We start this exploration by examining the shape of the data, examining the data types, checking for missing values and examining the summary statistics. 
```sql
--counting the number of rows
SELECT COUNT(*) AS row_count FROM iris;
--counting the number of columns
SELECT COUNT(column_name) AS column_count
FROM information_schema.columns
WHERE table_schema = 'public'
  AND table_name = 'iris';
--showing the data type of each column
SELECT column_name, data_type
FROM information_schema.columns
WHERE table_schema = 'public'
  AND table_name = 'iris';
--counting all the NULL values
SELECT COUNT(*) AS null_rows
FROM public.iris
WHERE (sepal_length IS NULL
       OR sepal_width IS NULL
       OR petal_length IS NULL
       OR petal_width IS NULL
	   Or species IS NULL);
```
Now we examine the summary summary statistics:
```sql
--sepal_length statistics
WITH stats_sepal_length AS (
    SELECT 
		AVG(sepal_length) AS mean_val,
		PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY sepal_length) AS median_val,
		MODE() WITHIN GROUP (ORDER BY sepal_length) AS mode_val,
        MIN(sepal_length) AS min_val,
        MAX(sepal_length) AS max_val,
		MAX(sepal_length) - MIN(sepal_length) AS range_val,
		STDDEV(sepal_length) AS stddev_val,
		VARIANCE(sepal_length) AS variance_val,
        PERCENTILE_CONT(0.25) WITHIN GROUP (ORDER BY sepal_length) AS q1_val,
        PERCENTILE_CONT(0.75) WITHIN GROUP (ORDER BY sepal_length) AS q3_val,
		PERCENTILE_CONT(0.75) WITHIN GROUP (ORDER BY sepal_length) - PERCENTILE_CONT(0.25) WITHIN GROUP (ORDER BY sepal_length) AS iqr_val,
        3 * (AVG(sepal_length) - PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY sepal_length)) / NULLIF(STDDEV(sepal_length), 0) AS skewness_val
    FROM public.iris
)

SELECT 1 AS sno,'Mean' AS statistic, ROUND(mean_val::NUMERIC, 2) AS value FROM stats_sepal_length UNION ALL
SELECT 2,'Median', ROUND(median_val::NUMERIC, 2) FROM stats_sepal_length UNION ALL
SELECT 3,'Mode', ROUND(mode_val::NUMERIC, 2) FROM stats_sepal_length UNION ALL
SELECT 4, 'Minimum', ROUND(min_val::NUMERIC, 2) FROM stats_sepal_length UNION ALL
SELECT 5,'Maximum', ROUND(max_val::NUMERIC, 2) FROM stats_sepal_length UNION ALL
SELECT 6,'Range', ROUND(range_val::NUMERIC, 2) FROM stats_sepal_length UNION ALL
SELECT 7,'Standard Deviation', ROUND(stddev_val::NUMERIC, 2) FROM stats_sepal_length UNION ALL
SELECT 8,'Variance', ROUND(variance_val::NUMERIC, 2) FROM stats_sepal_length UNION ALL
SELECT 9,'Q1', ROUND(q1_val::NUMERIC, 2) FROM stats_sepal_length UNION ALL
SELECT 10,'Q3', ROUND(q3_val::NUMERIC, 2) FROM stats_sepal_length UNION ALL
SELECT 11,'IQR', ROUND(iqr_val::NUMERIC, 2) FROM stats_sepal_length UNION ALL
SELECT 12,'Skewness', ROUND(skewness_val::NUMERIC, 2) FROM stats_sepal_length;


--sepal_width statistics
WITH stats_sepal_width AS (
    SELECT 
		AVG(sepal_width) AS mean_val,
		PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY sepal_width) AS median_val,
		MODE() WITHIN GROUP (ORDER BY sepal_width) AS mode_val,
        MIN(sepal_width) AS min_val,
        MAX(sepal_width) AS max_val,
		MAX(sepal_width) - MIN(sepal_width) AS range_val,
		STDDEV(sepal_width) AS stddev_val,
		VARIANCE(sepal_width) AS variance_val,
        PERCENTILE_CONT(0.25) WITHIN GROUP (ORDER BY sepal_width) AS q1_val,
        PERCENTILE_CONT(0.75) WITHIN GROUP (ORDER BY sepal_width) AS q3_val,
		PERCENTILE_CONT(0.75) WITHIN GROUP (ORDER BY sepal_width) - PERCENTILE_CONT(0.25) WITHIN GROUP (ORDER BY sepal_width) AS iqr_val,
        3 * (AVG(sepal_width) - PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY sepal_width)) / NULLIF(STDDEV(sepal_width), 0) AS skewness_val
    FROM public.iris
)

SELECT 1 AS sno,'Mean' AS statistic, ROUND(mean_val::NUMERIC, 2) AS value FROM stats_sepal_width UNION ALL
SELECT 2,'Median', ROUND(median_val::NUMERIC, 2) FROM stats_sepal_width UNION ALL
SELECT 3,'Mode', ROUND(mode_val::NUMERIC, 2) FROM stats_sepal_width UNION ALL
SELECT 4, 'Minimum', ROUND(min_val::NUMERIC, 2) FROM stats_sepal_width UNION ALL
SELECT 5,'Maximum', ROUND(max_val::NUMERIC, 2) FROM stats_sepal_width UNION ALL
SELECT 6,'Range', ROUND(range_val::NUMERIC, 2) FROM stats_sepal_width UNION ALL
SELECT 7,'Standard Deviation', ROUND(stddev_val::NUMERIC, 2) FROM stats_sepal_width UNION ALL
SELECT 8,'Variance', ROUND(variance_val::NUMERIC, 2) FROM stats_sepal_width UNION ALL
SELECT 9,'Q1', ROUND(q1_val::NUMERIC, 2) FROM stats_sepal_width UNION ALL
SELECT 10,'Q3', ROUND(q3_val::NUMERIC, 2) FROM stats_sepal_width UNION ALL
SELECT 11,'IQR', ROUND(iqr_val::NUMERIC, 2) FROM stats_sepal_width UNION ALL
SELECT 12,'Skewness', ROUND(skewness_val::NUMERIC, 2) FROM stats_sepal_width;


--petal_length statistics
WITH stats_petal_length AS (
    SELECT 
		AVG(petal_length) AS mean_val,
		PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY petal_length) AS median_val,
		MODE() WITHIN GROUP (ORDER BY petal_length) AS mode_val,
        MIN(petal_length) AS min_val,
        MAX(petal_length) AS max_val,
		MAX(petal_length) - MIN(petal_length) AS range_val,
		STDDEV(petal_length) AS stddev_val,
		VARIANCE(petal_length) AS variance_val,
        PERCENTILE_CONT(0.25) WITHIN GROUP (ORDER BY petal_length) AS q1_val,
        PERCENTILE_CONT(0.75) WITHIN GROUP (ORDER BY petal_length) AS q3_val,
		PERCENTILE_CONT(0.75) WITHIN GROUP (ORDER BY petal_length) - PERCENTILE_CONT(0.25) WITHIN GROUP (ORDER BY petal_length) AS iqr_val,
        3 * (AVG(petal_length) - PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY petal_length)) / NULLIF(STDDEV(petal_length), 0) AS skewness_val
    FROM public.iris
)

SELECT 1 AS sno,'Mean' AS statistic, ROUND(mean_val::NUMERIC, 2) AS value FROM stats_petal_length UNION ALL
SELECT 2,'Median', ROUND(median_val::NUMERIC, 2) FROM stats_petal_length UNION ALL
SELECT 3,'Mode', ROUND(mode_val::NUMERIC, 2) FROM stats_petal_length UNION ALL
SELECT 4, 'Minimum', ROUND(min_val::NUMERIC, 2) FROM stats_petal_length UNION ALL
SELECT 5,'Maximum', ROUND(max_val::NUMERIC, 2) FROM stats_petal_length UNION ALL
SELECT 6,'Range', ROUND(range_val::NUMERIC, 2) FROM stats_petal_length UNION ALL
SELECT 7,'Standard Deviation', ROUND(stddev_val::NUMERIC, 2) FROM stats_petal_length UNION ALL
SELECT 8,'Variance', ROUND(variance_val::NUMERIC, 2) FROM stats_petal_length UNION ALL
SELECT 9,'Q1', ROUND(q1_val::NUMERIC, 2) FROM stats_petal_length UNION ALL
SELECT 10,'Q3', ROUND(q3_val::NUMERIC, 2) FROM stats_petal_length UNION ALL
SELECT 11,'IQR', ROUND(iqr_val::NUMERIC, 2) FROM stats_petal_length UNION ALL
SELECT 12,'Skewness', ROUND(skewness_val::NUMERIC, 2) FROM stats_petal_length;


--petal_width statistics
WITH stats_petal_width AS (
    SELECT 
		AVG(petal_width) AS mean_val,
		PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY petal_width) AS median_val,
		MODE() WITHIN GROUP (ORDER BY petal_width) AS mode_val,
        MIN(petal_width) AS min_val,
        MAX(petal_width) AS max_val,
		MAX(petal_width) - MIN(petal_width) AS range_val,
		STDDEV(petal_width) AS stddev_val,
		VARIANCE(petal_width) AS variance_val,
        PERCENTILE_CONT(0.25) WITHIN GROUP (ORDER BY petal_width) AS q1_val,
        PERCENTILE_CONT(0.75) WITHIN GROUP (ORDER BY petal_width) AS q3_val,
		PERCENTILE_CONT(0.75) WITHIN GROUP (ORDER BY petal_width) - PERCENTILE_CONT(0.25) WITHIN GROUP (ORDER BY petal_width) AS iqr_val,
        3 * (AVG(petal_width) - PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY petal_width)) / NULLIF(STDDEV(petal_width), 0) AS skewness_val
    FROM public.iris
)

SELECT 1 AS sno,'Mean' AS statistic, ROUND(mean_val::NUMERIC, 2) AS value FROM stats_petal_width UNION ALL
SELECT 2,'Median', ROUND(median_val::NUMERIC, 2) FROM stats_petal_width UNION ALL
SELECT 3,'Mode', ROUND(mode_val::NUMERIC, 2) FROM stats_petal_width UNION ALL
SELECT 4, 'Minimum', ROUND(min_val::NUMERIC, 2) FROM stats_petal_width UNION ALL
SELECT 5,'Maximum', ROUND(max_val::NUMERIC, 2) FROM stats_petal_width UNION ALL
SELECT 6,'Range', ROUND(range_val::NUMERIC, 2) FROM stats_petal_width UNION ALL
SELECT 7,'Standard Deviation', ROUND(stddev_val::NUMERIC, 2) FROM stats_petal_width UNION ALL
SELECT 8,'Variance', ROUND(variance_val::NUMERIC, 2) FROM stats_petal_width UNION ALL
SELECT 9,'Q1', ROUND(q1_val::NUMERIC, 2) FROM stats_petal_width UNION ALL
SELECT 10,'Q3', ROUND(q3_val::NUMERIC, 2) FROM stats_petal_width UNION ALL
SELECT 11,'IQR', ROUND(iqr_val::NUMERIC, 2) FROM stats_petal_width UNION ALL
SELECT 12,'Skewness', ROUND(skewness_val::NUMERIC, 2) FROM stats_petal_width;
```

OUT PUT
 
# Grouping Data By Species
Now we can aggregate the data to summarize measurements by species.
```sql
-- examining the setosa table
SELECT * FROM iris
WHERE species = 'Iris-setosa';
--examining the versicolor table
SELECT * FROM iris
WHERE species = 'Iris-versicolor';
--examining the virginica table
SELECT * FROM iris
WHERE species = 'Iris-virginica';
```
Now we calculate summry staticstics for each species. 
```sql
--setosa sepal length stats
WITH stats_sepal_length AS (
    SELECT 
		AVG(sepal_length) AS mean_val,
		PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY sepal_length) AS median_val,
		MODE() WITHIN GROUP (ORDER BY sepal_length) AS mode_val,
        MIN(sepal_length) AS min_val,
        MAX(sepal_length) AS max_val,
		MAX(sepal_length) - MIN(sepal_length) AS range_val,
		STDDEV(sepal_length) AS stddev_val,
		VARIANCE(sepal_length) AS variance_val,
        PERCENTILE_CONT(0.25) WITHIN GROUP (ORDER BY sepal_length) AS q1_val,
        PERCENTILE_CONT(0.75) WITHIN GROUP (ORDER BY sepal_length) AS q3_val,
		PERCENTILE_CONT(0.75) WITHIN GROUP (ORDER BY sepal_length) - PERCENTILE_CONT(0.25) WITHIN GROUP (ORDER BY sepal_length) AS iqr_val,
        3 * (AVG(sepal_length) - PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY sepal_length)) / NULLIF(STDDEV(sepal_length), 0) AS skewness_val
    FROM public.iris
	WHERE species = 'Iris-setosa'
)

SELECT 1 AS sno,'Mean' AS statistic, ROUND(mean_val::NUMERIC, 2) AS value FROM stats_sepal_length UNION ALL
SELECT 2,'Median', ROUND(median_val::NUMERIC, 2) FROM stats_sepal_length UNION ALL
SELECT 3,'Mode', ROUND(mode_val::NUMERIC, 2) FROM stats_sepal_length UNION ALL
SELECT 4, 'Minimum', ROUND(min_val::NUMERIC, 2) FROM stats_sepal_length UNION ALL
SELECT 5,'Maximum', ROUND(max_val::NUMERIC, 2) FROM stats_sepal_length UNION ALL
SELECT 6,'Range', ROUND(range_val::NUMERIC, 2) FROM stats_sepal_length UNION ALL
SELECT 7,'Standard Deviation', ROUND(stddev_val::NUMERIC, 2) FROM stats_sepal_length UNION ALL
SELECT 8,'Variance', ROUND(variance_val::NUMERIC, 2) FROM stats_sepal_length UNION ALL
SELECT 9,'Q1', ROUND(q1_val::NUMERIC, 2) FROM stats_sepal_length UNION ALL
SELECT 10,'Q3', ROUND(q3_val::NUMERIC, 2) FROM stats_sepal_length UNION ALL
SELECT 11,'IQR', ROUND(iqr_val::NUMERIC, 2) FROM stats_sepal_length UNION ALL
SELECT 12,'Skewness', ROUND(skewness_val::NUMERIC, 2) FROM stats_sepal_length;

--setosa petal length stats
WITH stats_petal_length AS (
    SELECT 
		AVG(petal_length) AS mean_val,
		PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY petal_length) AS median_val,
		MODE() WITHIN GROUP (ORDER BY petal_length) AS mode_val,
        MIN(petal_length) AS min_val,
        MAX(petal_length) AS max_val,
		MAX(petal_length) - MIN(petal_length) AS range_val,
		STDDEV(petal_length) AS stddev_val,
		VARIANCE(petal_length) AS variance_val,
        PERCENTILE_CONT(0.25) WITHIN GROUP (ORDER BY petal_length) AS q1_val,
        PERCENTILE_CONT(0.75) WITHIN GROUP (ORDER BY petal_length) AS q3_val,
		PERCENTILE_CONT(0.75) WITHIN GROUP (ORDER BY petal_length) - PERCENTILE_CONT(0.25) WITHIN GROUP (ORDER BY petal_length) AS iqr_val,
        3 * (AVG(petal_length) - PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY petal_length)) / NULLIF(STDDEV(petal_length), 0) AS skewness_val
    FROM public.iris
	WHERE species = 'Iris-setosa'
)

SELECT 1 AS sno,'Mean' AS statistic, ROUND(mean_val::NUMERIC, 2) AS value FROM stats_petal_length UNION ALL
SELECT 2,'Median', ROUND(median_val::NUMERIC, 2) FROM stats_petal_length UNION ALL
SELECT 3,'Mode', ROUND(mode_val::NUMERIC, 2) FROM stats_petal_length UNION ALL
SELECT 4, 'Minimum', ROUND(min_val::NUMERIC, 2) FROM stats_petal_length UNION ALL
SELECT 5,'Maximum', ROUND(max_val::NUMERIC, 2) FROM stats_petal_length UNION ALL
SELECT 6,'Range', ROUND(range_val::NUMERIC, 2) FROM stats_petal_length UNION ALL
SELECT 7,'Standard Deviation', ROUND(stddev_val::NUMERIC, 2) FROM stats_petal_length UNION ALL
SELECT 8,'Variance', ROUND(variance_val::NUMERIC, 2) FROM stats_petal_length UNION ALL
SELECT 9,'Q1', ROUND(q1_val::NUMERIC, 2) FROM stats_petal_length UNION ALL
SELECT 10,'Q3', ROUND(q3_val::NUMERIC, 2) FROM stats_petal_length UNION ALL
SELECT 11,'IQR', ROUND(iqr_val::NUMERIC, 2) FROM stats_petal_length UNION ALL
SELECT 12,'Skewness', ROUND(skewness_val::NUMERIC, 2) FROM stats_petal_length;

--versicolor sepal length stats
WITH stats_sepal_length AS (
    SELECT 
		AVG(sepal_length) AS mean_val,
		PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY sepal_length) AS median_val,
		MODE() WITHIN GROUP (ORDER BY sepal_length) AS mode_val,
        MIN(sepal_length) AS min_val,
        MAX(sepal_length) AS max_val,
		MAX(sepal_length) - MIN(sepal_length) AS range_val,
		STDDEV(sepal_length) AS stddev_val,
		VARIANCE(sepal_length) AS variance_val,
        PERCENTILE_CONT(0.25) WITHIN GROUP (ORDER BY sepal_length) AS q1_val,
        PERCENTILE_CONT(0.75) WITHIN GROUP (ORDER BY sepal_length) AS q3_val,
		PERCENTILE_CONT(0.75) WITHIN GROUP (ORDER BY sepal_length) - PERCENTILE_CONT(0.25) WITHIN GROUP (ORDER BY sepal_length) AS iqr_val,
        3 * (AVG(sepal_length) - PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY sepal_length)) / NULLIF(STDDEV(sepal_length), 0) AS skewness_val
    FROM public.iris
	WHERE species = 'Iris-versicolor'
)

SELECT 1 AS sno,'Mean' AS statistic, ROUND(mean_val::NUMERIC, 2) AS value FROM stats_sepal_length UNION ALL
SELECT 2,'Median', ROUND(median_val::NUMERIC, 2) FROM stats_sepal_length UNION ALL
SELECT 3,'Mode', ROUND(mode_val::NUMERIC, 2) FROM stats_sepal_length UNION ALL
SELECT 4, 'Minimum', ROUND(min_val::NUMERIC, 2) FROM stats_sepal_length UNION ALL
SELECT 5,'Maximum', ROUND(max_val::NUMERIC, 2) FROM stats_sepal_length UNION ALL
SELECT 6,'Range', ROUND(range_val::NUMERIC, 2) FROM stats_sepal_length UNION ALL
SELECT 7,'Standard Deviation', ROUND(stddev_val::NUMERIC, 2) FROM stats_sepal_length UNION ALL
SELECT 8,'Variance', ROUND(variance_val::NUMERIC, 2) FROM stats_sepal_length UNION ALL
SELECT 9,'Q1', ROUND(q1_val::NUMERIC, 2) FROM stats_sepal_length UNION ALL
SELECT 10,'Q3', ROUND(q3_val::NUMERIC, 2) FROM stats_sepal_length UNION ALL
SELECT 11,'IQR', ROUND(iqr_val::NUMERIC, 2) FROM stats_sepal_length UNION ALL
SELECT 12,'Skewness', ROUND(skewness_val::NUMERIC, 2) FROM stats_sepal_length;

--versicolor petal length stats
WITH stats_petal_length AS (
    SELECT 
		AVG(petal_length) AS mean_val,
		PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY petal_length) AS median_val,
		MODE() WITHIN GROUP (ORDER BY petal_length) AS mode_val,
        MIN(petal_length) AS min_val,
        MAX(petal_length) AS max_val,
		MAX(petal_length) - MIN(petal_length) AS range_val,
		STDDEV(petal_length) AS stddev_val,
		VARIANCE(petal_length) AS variance_val,
        PERCENTILE_CONT(0.25) WITHIN GROUP (ORDER BY petal_length) AS q1_val,
        PERCENTILE_CONT(0.75) WITHIN GROUP (ORDER BY petal_length) AS q3_val,
		PERCENTILE_CONT(0.75) WITHIN GROUP (ORDER BY petal_length) - PERCENTILE_CONT(0.25) WITHIN GROUP (ORDER BY petal_length) AS iqr_val,
        3 * (AVG(petal_length) - PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY petal_length)) / NULLIF(STDDEV(petal_length), 0) AS skewness_val
    FROM public.iris
	WHERE species = 'Iris-versicolor'
)

SELECT 1 AS sno,'Mean' AS statistic, ROUND(mean_val::NUMERIC, 2) AS value FROM stats_petal_length UNION ALL
SELECT 2,'Median', ROUND(median_val::NUMERIC, 2) FROM stats_petal_length UNION ALL
SELECT 3,'Mode', ROUND(mode_val::NUMERIC, 2) FROM stats_petal_length UNION ALL
SELECT 4, 'Minimum', ROUND(min_val::NUMERIC, 2) FROM stats_petal_length UNION ALL
SELECT 5,'Maximum', ROUND(max_val::NUMERIC, 2) FROM stats_petal_length UNION ALL
SELECT 6,'Range', ROUND(range_val::NUMERIC, 2) FROM stats_petal_length UNION ALL
SELECT 7,'Standard Deviation', ROUND(stddev_val::NUMERIC, 2) FROM stats_petal_length UNION ALL
SELECT 8,'Variance', ROUND(variance_val::NUMERIC, 2) FROM stats_petal_length UNION ALL
SELECT 9,'Q1', ROUND(q1_val::NUMERIC, 2) FROM stats_petal_length UNION ALL
SELECT 10,'Q3', ROUND(q3_val::NUMERIC, 2) FROM stats_petal_length UNION ALL
SELECT 11,'IQR', ROUND(iqr_val::NUMERIC, 2) FROM stats_petal_length UNION ALL
SELECT 12,'Skewness', ROUND(skewness_val::NUMERIC, 2) FROM stats_petal_length;

--virginica sepal length stats
WITH stats_sepal_length AS (
    SELECT 
		AVG(sepal_length) AS mean_val,
		PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY sepal_length) AS median_val,
		MODE() WITHIN GROUP (ORDER BY sepal_length) AS mode_val,
        MIN(sepal_length) AS min_val,
        MAX(sepal_length) AS max_val,
		MAX(sepal_length) - MIN(sepal_length) AS range_val,
		STDDEV(sepal_length) AS stddev_val,
		VARIANCE(sepal_length) AS variance_val,
        PERCENTILE_CONT(0.25) WITHIN GROUP (ORDER BY sepal_length) AS q1_val,
        PERCENTILE_CONT(0.75) WITHIN GROUP (ORDER BY sepal_length) AS q3_val,
		PERCENTILE_CONT(0.75) WITHIN GROUP (ORDER BY sepal_length) - PERCENTILE_CONT(0.25) WITHIN GROUP (ORDER BY sepal_length) AS iqr_val,
        3 * (AVG(sepal_length) - PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY sepal_length)) / NULLIF(STDDEV(sepal_length), 0) AS skewness_val
    FROM public.iris
	WHERE species = 'Iris-virginica'
)

SELECT 1 AS sno,'Mean' AS statistic, ROUND(mean_val::NUMERIC, 2) AS value FROM stats_sepal_length UNION ALL
SELECT 2,'Median', ROUND(median_val::NUMERIC, 2) FROM stats_sepal_length UNION ALL
SELECT 3,'Mode', ROUND(mode_val::NUMERIC, 2) FROM stats_sepal_length UNION ALL
SELECT 4, 'Minimum', ROUND(min_val::NUMERIC, 2) FROM stats_sepal_length UNION ALL
SELECT 5,'Maximum', ROUND(max_val::NUMERIC, 2) FROM stats_sepal_length UNION ALL
SELECT 6,'Range', ROUND(range_val::NUMERIC, 2) FROM stats_sepal_length UNION ALL
SELECT 7,'Standard Deviation', ROUND(stddev_val::NUMERIC, 2) FROM stats_sepal_length UNION ALL
SELECT 8,'Variance', ROUND(variance_val::NUMERIC, 2) FROM stats_sepal_length UNION ALL
SELECT 9,'Q1', ROUND(q1_val::NUMERIC, 2) FROM stats_sepal_length UNION ALL
SELECT 10,'Q3', ROUND(q3_val::NUMERIC, 2) FROM stats_sepal_length UNION ALL
SELECT 11,'IQR', ROUND(iqr_val::NUMERIC, 2) FROM stats_sepal_length UNION ALL
SELECT 12,'Skewness', ROUND(skewness_val::NUMERIC, 2) FROM stats_sepal_length;

--virginica petal length stats
WITH stats_petal_length AS (
    SELECT 
		AVG(petal_length) AS mean_val,
		PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY petal_length) AS median_val,
		MODE() WITHIN GROUP (ORDER BY petal_length) AS mode_val,
        MIN(petal_length) AS min_val,
        MAX(petal_length) AS max_val,
		MAX(petal_length) - MIN(petal_length) AS range_val,
		STDDEV(petal_length) AS stddev_val,
		VARIANCE(petal_length) AS variance_val,
        PERCENTILE_CONT(0.25) WITHIN GROUP (ORDER BY petal_length) AS q1_val,
        PERCENTILE_CONT(0.75) WITHIN GROUP (ORDER BY petal_length) AS q3_val,
		PERCENTILE_CONT(0.75) WITHIN GROUP (ORDER BY petal_length) - PERCENTILE_CONT(0.25) WITHIN GROUP (ORDER BY petal_length) AS iqr_val,
        3 * (AVG(petal_length) - PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY petal_length)) / NULLIF(STDDEV(petal_length), 0) AS skewness_val
    FROM public.iris 
	WHERE species = 'Iris-virginica'
)

SELECT 1 AS sno,'Mean' AS statistic, ROUND(mean_val::NUMERIC, 2) AS value FROM stats_petal_length UNION ALL
SELECT 2,'Median', ROUND(median_val::NUMERIC, 2) FROM stats_petal_length UNION ALL
SELECT 3,'Mode', ROUND(mode_val::NUMERIC, 2) FROM stats_petal_length UNION ALL
SELECT 4, 'Minimum', ROUND(min_val::NUMERIC, 2) FROM stats_petal_length UNION ALL
SELECT 5,'Maximum', ROUND(max_val::NUMERIC, 2) FROM stats_petal_length UNION ALL
SELECT 6,'Range', ROUND(range_val::NUMERIC, 2) FROM stats_petal_length UNION ALL
SELECT 7,'Standard Deviation', ROUND(stddev_val::NUMERIC, 2) FROM stats_petal_length UNION ALL
SELECT 8,'Variance', ROUND(variance_val::NUMERIC, 2) FROM stats_petal_length UNION ALL
SELECT 9,'Q1', ROUND(q1_val::NUMERIC, 2) FROM stats_petal_length UNION ALL
SELECT 10,'Q3', ROUND(q3_val::NUMERIC, 2) FROM stats_petal_length UNION ALL
SELECT 11,'IQR', ROUND(iqr_val::NUMERIC, 2) FROM stats_petal_length UNION ALL
SELECT 12,'Skewness', ROUND(skewness_val::NUMERIC, 2) FROM stats_petal_length;
```
## Analyzing the Data
Heatmaps shows that sepal_length and petal_length are strignly correlated
```sql 
SELECT CORR(sepal_length, petal_length) FROM iris;
```

## Modeling the Data
