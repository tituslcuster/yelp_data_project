# Data Scientist Role Play: Profiling and Analyzing the Yelp Dataset Coursera Worksheet

This SQL project was a final assignment for the Couersera course "[SQL for Data Science](https://www.coursera.org/account/accomplishments/verify/K93FFFBPQ9AC)". 

The project is made up of two major parts:
+ Part 1 -- Yelp Dataset Profiling and Understanding
+ Part 2 -- Inferences and Analysis

Consider part 1 to be a very wide comprehensive exploratory data analysis about the entire dataset. This section is full of summary statistics about the dataset as a whole trying to understand data types and the scope of the dataset. Part 2 also contains explorartory data analysis principals, but instead it is a much more focused and practical 'eda' within a few tables in order to answer specific business questions that I came up with.

![The ER Diagram included with this assignment](https://i.imgur.com/2y8nPne.png)

The structure of the initial questions through out all of part 1 and aspects of part 2 originate from the course, but for the purposes of compiling my work I have resummarized and edited the questions for clear and consise readability. For a full look at the questions from the original assignment and my responses, check out [YelpDataProject_TitusCuster.sql](https://github.com/tituslcuster/SQL/blob/main/YelpDataProject_TitusCuster.sql).

  
## **Part 1: Yelp Dataset Profiling and Understanding**

### 1. Profiling the total number of records for each of the tables below: 

To begin our exploration of this dataset and how tables relate to each other, I totaled the count of rows to give insight into the size of each key table. It is notable that each of the tables had an even 10,000 records, but I included only a selection of the eleven total tables.

1. 'attribute' table: 10,000 records
```sql
    SELECT COUNT(*) 
    FROM attribute; 
```
2. 'business' table: 10,000 records
```sql 
    SELECT COUNT(*) 
    FROM business; 
```
3. 'category' table: 10,000 records
```sql 
    SELECT COUNT(*) 
    FROM category; 
```
4. 'checkin' table: 10,000 records
```sql    
    SELECT COUNT(*) 
    FROM checkin; 
```

### 2. Finding the total distinct records by either the foreign key or primary key for each table, and parsing different foreign keys
```sql
        SELECT COUNT(*) 
        FROM friend; 
```
1. Business = 10,000 
2. Hours = 1,562
3. Category = 2,643
4. Attribute = 1,115
5. Review = 10,000 
6. Checkin = 493 
7. Photo = 6,493 
8. Tip = 3,979 (business_id), 537 (user_id)
9. User = 10,000 
10. Friend = 11
11. Elite_years = 2,780 

### 3. Are there any columns with null values in the 'user'' table?

Searching for columns with null values to identify if  'user' needed to be cleaned. Null values impede future querying and don't add value to our analysis within the scope of this project.

```sql
	SELECT *
	FROM user
	WHERE id IS NULL
		OR name IS NULL
		OR review_count IS NULL
		OR yelping_since IS NULL
		OR useful IS NULL
		OR funny IS NULL
		OR cool IS NULL
		OR fans IS NULL
		OR average_stars IS NULL
		OR compliment_hot IS NULL
		OR compliment_more IS NULL
		OR compliment_profile IS NULL
		OR compliment_cute IS NULL
		OR compliment_list IS NULL
		OR compliment_note IS NULL
		OR compliment_plain IS NULL
		OR compliment_cool IS NULL
		OR compliment_funny IS NULL
		OR compliment_writer IS NULL
		OR compliment_photos IS NULL;
```
	
### 4. What is the minimum, maximum, and average of the following columns:

1. Review.Stars

+ min: 1		
+ max: 5		
+ avg: 3.7082
	
2. Business.Stars

+ min: 1.0		
+ max: 5.0		
+ avg: 3.6549 
	
3. Tip.Likes

+ min: 0		
+ max: 2		
+ avg: 0.0144 
	
4. Checkin.Count

+ min: 1		
+ max: 53		
+ avg:  1.9414
	
5.  User.Review_count

+ min: 0		
+ max: 2000		
+ avg: 24.2995 

### 5. List the cities with the most reviews in descending order:

```sql
    SELECT city,
    review_count
    FROM business
    GROUP BY city
    ORDER BY review_count DESC
```

    +------------+--------------+
    | city       | review_count |
    +------------+--------------+
    | Las Vegas  |         3873 |
    | Montréal   |         1757 |
    | Gilbert    |         1549 |
    | Las Vegas  |         1410 |
    | Las Vegas  |         1389 |
    | Las Vegas  |         1252 |
    | Las Vegas  |         1116 |
    | Las Vegas  |         1084 |
    | Las Vegas  |          961 |
    | Gilbert    |          902 |
    | Las Vegas  |          864 |
    | Scottsdale |          823 |
    | Las Vegas  |          821 |
    | Las Vegas  |          786 |
    | Henderson  |          785 |
    | Toronto    |          778 |
    | Las Vegas  |          768 |
    | Las Vegas  |          758 |
    | Scottsdale |          726 |
    | Cleveland  |          723 |
    | Las Vegas  |          720 |
    | Charlotte  |          715 |
    | Phoenix    |          711 |
    | Las Vegas  |          706 |
    | Phoenix    |          700 |
    +------------+--------------+
    (Output limit exceeded, 25 of 10000 total rows shown)

### 6. Finding the distribution of star ratings to businesses in Avon and Beachwood:

1. Avon
```sql
    SELECT city,
        AVG(stars) AS star_rating,
        review_count
    FROM business
    WHERE city = 'Avon'
    GROUP BY city;
```
    +------+-------------+--------------+
    | city | star_rating | review_count |
    +------+-------------+--------------+
    | Avon |        3.45 |           17 |
    +------+-------------+--------------+

2. Beachwood

```sql
    SELECT city,
        AVG(stars) AS star_rating,
        review_count
    FROM business
    WHERE city = 'Beachwood'
    GROUP BY city;
```
    +-----------+---------------+--------------+
    | city      |   star_rating | review_count |
    +-----------+---------------+--------------+
    | Beachwood | 3.96428571429 |            4 |
    +-----------+---------------+--------------+		


### 7. Identifying the top 3 users based on their total number of reviews:
```sql
    SELECT name,
        review_count
    FROM user
    ORDER BY review_count DESC
    LIMIT 3
```	
    +--------+--------------+
    | name   | review_count |
    +--------+--------------+
    | Gerald |         2000 |
    | Sara   |         1629 |
    | Yuri   |         1339 |
    +--------+--------------+

 ### 8. Does posting more reviews correlate with more fans?

Posting more reviews did not directly correlate with more fans. The three users with the highest number	of fans had all posted fewer reviews than the three users with the most reviews. What's more is that the three users with the most reviews did not even appear in the top 10 of users with the most fans.
		
The top ten users with the most reviews:
```
    +-----------+--------------+------+
    | name      | review_count | fans |
    +-----------+--------------+------+
    | Gerald    |         2000 |  253 |
    | Sara      |         1629 |   50 |
    | Yuri      |         1339 |   76 |
    | .Hon      |         1246 |  101 |
    | William   |         1215 |  126 |
    | Harald    |         1153 |  311 |
    | eric      |         1116 |   16 |
    | Roanna    |         1039 |  104 |
    | Mimi      |          968 |  497 |
    | Christine |          930 |  173 |
    +-----------+--------------+------+
```
The top ten users with the most fans:
```
    +-----------+--------------+------+
    | name      | review_count | fans |
    +-----------+--------------+------+
    | Amy       |          609 |  503 |
    | Mimi      |          968 |  497 |
    | Harald    |         1153 |  311 |
    | Gerald    |         2000 |  253 |
    | Christine |          930 |  173 |
    | Lisa      |          813 |  159 |
    | Cat       |          377 |  133 |
    | William   |         1215 |  126 |
    | Fran      |          862 |  124 |
    | Lissa     |          834 |  120 |
    +-----------+--------------+------+
```
### 9. Are there more reviews with the word "love" or with the word "hate" in them?

There were far more who used the word "love" in their review (1780) compared to the number that used "hate" (232).

```sql	
    SELECT COUNT(text) AS NumberofHate
    FROM review
    WHERE text LIKE '%hate%';
```
```sql
    SELECT COUNT(text) AS NumberofLove
    FROM review
    WHERE text LIKE '%love%';
```
### 10. Find the top 10 users with the most fans:
```sql		
        SELECT name,
			review_count,
			fans
		FROM user
		ORDER BY fans DESC
		LIMIT 10;
```
Listed here are the top 10 users with the most fans:	
```    
    +-----------+--------------+------+
    | name      | review_count | fans |
    +-----------+--------------+------+
    | Amy       |          609 |  503 |
    | Mimi      |          968 |  497 |
    | Harald    |         1153 |  311 |
    | Gerald    |         2000 |  253 |
    | Christine |          930 |  173 |
    | Lisa      |          813 |  159 |
    | Cat       |          377 |  133 |
    | William   |         1215 |  126 |
    | Fran      |          862 |  124 |
    | Lissa     |          834 |  120 |
    +-----------+--------------+------+
```	