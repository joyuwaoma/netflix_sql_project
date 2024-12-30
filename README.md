# Netflix Movies and TV Shows Data Analysis using SQL

![Netflix Logo](https://github.com/joyuwaoma/netflix_sql_project/blob/main/netflix-5947489_1280.png)

## Overview
This project uses SQL to analyze Netflix movies and TV shows data comprehensively. The goal is to extract valuable insights and answer various business questions based on the dataset. The following README provides a detailed account of the project's objectives, business problems, solutions, findings, and conclusions.

## Objectives
* Analyze the distribution of content types (movies vs TV shows). 
* Identify the most common ratings for movies and TV shows.
* List and analyze content based on release years, countries, and durations.
* Explore and categorize content based on specific criteria and keywords.

## Dataset
The data for this project was obtained from [Kaggle](https://www.kaggle.com/datasets/shivamb/netflix-shows?resource=download) 

## Schema
```sql
CREATE TABLE netflix
(
	show_id VARCHAR(6),
	type    VARCHAR(10),
	title   VARCHAR(150),
	director VARCHAR(208),
	casts    VARCHAR(1000),
	country VARCHAR(150),
	date_added  VARCHAR(50),
	release_year INT,
	rating  VARCHAR(10),
	duration  VARCHAR(15),
	listed_in VARCHAR(100),
	description VARCHAR(250)
);

```
## 15 Business Problems and Solutions
#### 1. Count the Number of Movies vs TV Shows.
```sql
SELECT 
	type,
	COUNT (*) AS total_content
FROM netflix
GROUP BY type
```
#### 2. Find the most common rating for movies and TV shows.
```sql
SELECT
	type,
	rating
FROM
(
	SELECT 
		type,
		rating,
		COUNT(*),
		RANK() OVER(PARTITION BY type ORDER BY COUNT(*) DESC) AS ranking
	FROM netflix
	GROUP BY 1, 2
) as t1
WHERE ranking = 1
 ```
#### 3. List all movies released in a specific year (e.g., 2020)
```sql
SELECT * FROM netflix
WHERE
	type = 'Movie'
	AND 
	release_year = 2020
```
#### 4. Find the top 5 countries with the most content on Netflix.
```sql
SELECT 
	UNNEST(STRING_TO_ARRAY(country, ',')) AS new_country,
	COUNT(show_id) AS total_content
FROM netflix
GROUP BY 1
ORDER BY total_content DESC
LIMIT 5
```
#### 5. Identify the longest movie or TV show duration.
```sql
SELECT * FROM netflix
WHERE
	type = 'Movie' 
	AND duration = (SELECT MAX(duration) FROM netflix)
```
#### 6. Find content added in the last 5 years.
```sql
SELECT * FROM netflix
WHERE 
	TO_DATE(date_added, 'Month DD, YYYY') >= CURRENT_DATE - INTERVAL '5 years'
```
#### 7. Find all the movies/TV shows by director 'Rajiv Chilaka'!
```sql
SELECT * FROM netflix
WHERE director ILIKE '%Rajiv Chilaka%'
```
#### 8. List all TV shows with more than 5 seasons.
```sql
SELECT * FROM netflix
WHERE 
	type = 'TV Show'
	AND 
	SPLIT_PART(duration, ' ', 1)::numeric > 5
```
#### 9. Count the number of content items in each genre.
```sql
SELECT 
	UNNEST(STRING_TO_ARRAY(listed_in, ',')) AS genre,
	COUNT(show_id) AS total_content
FROM netflix
GROUP BY 1
```
#### 10. Find each year and the average numbers of content release by india on netflix, return top 5 year with highest avg content release.
```sql
-- total content 333/972

SELECT 
	EXTRACT(YEAR FROMM TO_DATE(date_added, 'Month DD, YYYY')) AS year,
	COUNT(*),
	ROUND(
	COUNT(*)::numeric/(SELECT COUNT (*) FROM netflix WHERE country = 'India')::numeric * 100,2) AS avg_content_per_year
FROM netflix
WHERE country = 'India'
GROUP BY 1
```
#### 11. List all movies that are documentaries.
```sql
SELECT * FROM netflix
WHERE 
	listed_in ILIKE '%documentaries%'
```
#### 12. Find all content without a director.
```sql
SELECT * FROM netflix
WHERE 
	director IS NULL
```
#### 13. Find how many movies actor 'Salman Khan' has appeared in the last 10 years!
```sql
SELECT * FROM netflix
WHERE 
	casts ILIKE '%Salman Khan%'
	AND 
	release_year > EXTRACT(YEAR FROM CURRENT_DATE) - 10
```
#### 14. Find the top 10 actors who have appeared in the highest number of movies produced in.
```sql
SELECT 
UNNEST(STRING_TO_ARRAY(casts,',')) AS actors,
COUNT(*) AS total_content
FROM netflix
WHERE country ILIKE '%india'
GROUP BY 1
ORDER BY 2 DESC
LIMIT 10
```
#### 15. Categorize the content based on the presence of the keywords 'kill' and 'violence' in the description field. Label the content containing these keywords as 'Bad' and all other content as 'Good'. Count how many items fall into each category.
```sql
WITH new_table 
AS (
SELECT * ,
	CASE
	WHEN	
		description ILIKE '%kill%' OR
		description ILIKE '%violence%' THEN 'Bad_Content'
		ELSE 'Good_Content'
	END category
FROM netflix
)
SELECT
	category,
	COUNT(*) AS total_content
FROM new_table
GROUP BY 1
```
