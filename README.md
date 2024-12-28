# Netflix Movies and TV Shows Data Analysis using SQL

![Netflix Logo](https://github.com/joyuwaoma/netflix_sql_project/blob/main/netflix-5947489_1280.png)

## Overview
This project uses SQL to comprehensively analyze data on India's Netflix movies and TV shows. The goal is to extract valuable insights and answer various business questions based on the dataset. The following README provides a detailed account of the project's objectives, business problems, solutions, findings, and conclusions.

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
