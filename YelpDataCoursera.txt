/* Data Scientist Role Play: Profiling and Analyzing the Yelp Dataset Coursera Worksheet

For both parts of this assignment, use this "worksheet." It provides all the questions you are being asked, and your job will be to transfer your answers and SQL coding where indicated into this worksheet so that your peers can review your work. You should be able to use any Text Editor (Windows Notepad, Apple TextEdit, Notepad ++, Sublime Text, etc.) to copy and paste your answers. If you are going to use Word or some other page layout application, just be careful to make sure your answers and code are lined appropriately.
In this case, you may want to save as a PDF to ensure your formatting remains intact for you reviewer.
*/


  ---------------------------------------------------
--| Part 1: Yelp Dataset Profiling and Understanding |--
  ---------------------------------------------------

/* 1. Profile the data by finding the total number of records for each of the tables below: */

SELECT COUNT(*) 
FROM attribute; 
-- i. Attribute table = 10000 

SELECT COUNT(*) 
FROM business; 
-- ii. Business table = 10000 

SELECT COUNT(*) 
FROM category; 
-- iii. Category table = 10000 

SELECT COUNT(*) 
FROM checkin; 
-- iv. Checkin table = 10000 

SELECT COUNT(*) 
FROM elite_years; 
-- v. elite_years table = 10000 

SELECT COUNT(*) 
FROM friend; 
-- vi. friend table = 10000 

SELECT COUNT(*) 
FROM hours; 
-- vii. hours table = 10000 

SELECT COUNT(*) 
FROM photo; 
-- viii. photo table = 10000 

SELECT COUNT(*) 
FROM review; 
-- ix. review table = 10000 

SELECT COUNT(*) 
FROM tip; 
-- x. tip table = 10000 

SELECT COUNT(*) 
FROM user; 
-- xi. user table = 10000 
	

/* 2. Find the total distinct records by either the foreign key or primary key for each table. If two foreign keys are listed in the table, please specify which foreign key. */

SELECT COUNT(*) 
FROM friend; 
-- i. Business = 10000 
ii. Hours = 1562
iii. Category = 2643
iv. Attribute = 1115
v. Review = 10000 
vi. Checkin = 493 
vii. Photo = 6493 
viii. Tip = 3979 (business_id), 537 (user_id)
ix. User = 10000 
x. Friend = 11
xi. Elite_years = 2780 

Note: Primary Keys are denoted in the ER-Diagram with a yellow key icon.	



/* 3. Are there any columns with null values in the Users table? Indicate "yes," or "no." */

	Answer: no 
	
-- SQL code used to arrive at answer:
	
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
	
	
/* 4. For each table and column listed below, display the smallest (minimum), largest (maximum), and average (mean) value for the following fields: */

	-- i. Table: Review, Column: Stars
	
		min: 1		max: 5		avg: 3.7082
		
	
	-- ii. Table: Business, Column: Stars
	
		min: 1.0		max: 5.0		avg: 3.6549 
		
	
	-- iii. Table: Tip, Column: Likes
	
		min: 0		max: 2		avg: 0.0144 
		
	
	-- iv. Table: Checkin, Column: Count
	
		min: 1		max: 53		avg:  1.9414
		
	
	-- v. Table: User, Column: Review_count
	
		min: 0		max: 2000		avg: 24.2995 
		


/* 5. List the cities with the most reviews in descending order: */

	-- SQL code used to arrive at answer: 
	SELECT city,
		review_count
	FROM business
	GROUP BY city
	ORDER BY review_count DESC
	
	/* Copy and Paste the Result Below:
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
	*/
	
/* 6. Find the distribution of star ratings to the business in the following cities: */

	i. Avon

		-- SQL code used to arrive at answer:
		
			SELECT city,
				AVG(stars) AS star_rating,
				review_count
			FROM business
			WHERE city = 'Avon'
			GROUP BY city;

		-- Copy and Paste the Resulting Table Below (2 columns – star rating and count):
		
			+------+-------------+--------------+
			| city | star_rating | review_count |
			+------+-------------+--------------+
			| Avon |        3.45 |           17 |
			+------+-------------+--------------+

	ii. Beachwood

		-- SQL code used to arrive at answer:
		
			SELECT city,
				AVG(stars) AS star_rating,
				review_count
			FROM business
			WHERE city = 'Beachwood'
			GROUP BY city;

		-- Copy and Paste the Resulting Table Below (2 columns – star rating and count):

			+-----------+---------------+--------------+
			| city      |   star_rating | review_count |
			+-----------+---------------+--------------+
			| Beachwood | 3.96428571429 |            4 |
			+-----------+---------------+--------------+		


/* 7. Find the top 3 users based on their total number of reviews: */
		
	-- SQL code used to arrive at answer:
	
		SELECT name,
			review_count
		FROM user
		ORDER BY review_count DESC
		LIMIT 3
		
	-- Copy and Paste the Result Below:
		+--------+--------------+
		| name   | review_count |
		+--------+--------------+
		| Gerald |         2000 |
		| Sara   |         1629 |
		| Yuri   |         1339 |
		+--------+--------------+

/* 8. Does posing more reviews correlate with more fans? */

	-- Please explain your findings and interpretation of the results:
	/*	Posting more reviews does not directly correlate with more fans. 

		The three users with the highest number	of fans have all posted fewer reviews than the three users with the most reviews. 
		
		Whats more is that the three users with the most reviews do not even appear in the top 10 of users with the most fans.
		
		The top ten users with the most reviews:
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
		The top ten users with the most fans:
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
	*/

	
/* 9. Are there more reviews with the word "love" or with the word "hate" in them? */

	Answer: There were far more who used the word "love" in their review (1780) compared to the
			number that used "hate" (232)

	
	--SQL code used to arrive at answer:
		SELECT COUNT(text) AS NumberofHate
		FROM review
		WHERE text LIKE '%hate%';
	
		SELECT COUNT(text) AS NumberofLove
		FROM review
		WHERE text LIKE '%love%';
	
/* 10. Find the top 10 users with the most fans: */

	-- SQL code used to arrive at answer:
		SELECT name,
			review_count,
			fans
		FROM user
		ORDER BY fans DESC
		LIMIT 10;
	
	-- Copy and Paste the Result Below:
	/*
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
	*/
		
  -----------------------------------
--| Part 2: Inferences and Analysis |--
  -----------------------------------

/* 1. Pick one city and category of your choice and group the businesses in that city or category by their overall star rating.Compare the businesses with 2-3 stars to the businesses with 4-5 stars and answer the following questions. */

-- My choices for further analysis
		City: Scottsdale
		Category: Soul Food
		2-3 stars = 'Top Rated'
		4-5 stars = 'Top Rated'

--- Summary Data ---
Average Soul Food Rating: 3.75 ('Top Rated' Category)
Average Scottsdale Rating: 3.95 ('Average Rating' Category)

	-- Scottsdale
		-- Spread of ratings in the city of Scottsdale
			+-------------+-----------------+
			| Star_Rating | Count_Of_Rating |
			+-------------+-----------------+
			|         5.0 |             137 |
			|         4.5 |              86 |
			|         4.0 |             102 |
			|         3.5 |              63 |
			|         3.0 |              41 |
			|         2.5 |              34 |
			|         2.0 |              19 |
			|         1.5 |              10 |
			|         1.0 |               5 |
			+-------------+-----------------+
			((Total amount of ratings: 395))

			-- Top 
			Scottsdale 'Top Rated' businesses: 188
			Scottsdale 5 star businesses: 137 (2,094 total reviews)
			Scottsdale 4 star businesses: 102 (10,594 total reviews)
			-- Average
			Scottsdale 'Average Rating' businesses: 157 
			Scottsdale 3 star businesses: 41 (6,730 total reviews)
			Scottsdale 2 star businesses: 19 (1,050 total reviews)
			--	Low
			Scottsdale 'Low Rating' businesses: 15
			Scottsdale 1 star businesses: 5 (146 total reviews)
		Total number of Yelp reviews in Scottsdale: 20,614

	-- Hours of opperation summary
		Minimum: 
		Maximum: 
		Average:

-- Soul Food
	-- There are only two 'Soul Food' businesses
		+---------------+-------------------------------+-----------+-------+----------------+
		| city          | name                          | category  | stars | StarCategory   |
		+---------------+-------------------------------+-----------+-------+----------------+
		| Phoenix       | Charlie D's Catfish & Chicken | Soul Food |   4.5 | Top Rated      |
		| North Randall | Oinky's Pork Chop Heaven      | Soul Food |   3.0 | Average Rating |
		+---------------+-------------------------------+-----------+-------+----------------+

	-- Hours of opperation summary
		Minimum: 
		Maximum: 
		Average:

/* i. Do the two groups you chose to analyze have a different distribution of hours? */
	RESPONSE!

/* ii. Do the two groups you chose to analyze have a different number of reviews? */
    RESPONSE!
         
/* iii. Are you able to infer anything from the location data provided between these two groups? Explain. */
	RESPONSE!

/* SQL code used for analysis: */
-- Creating a categorization system for star numbers for ease of analysis
	SELECT city,
		name,
		stars,
		--c.category
		CASE
			WHEN stars >= 4 AND stars <= 5 THEN 'Top Rated'
			WHEN stars >= 3 AND stars < 4 THEN 'Average Rating'
			WHEN stars >= 2 AND stars < 3 THEN 'Average Rating'
			WHEN stars >= 1 AND stars < 2 THEN 'Low Rating'
			ELSE 'Not Relevant'
		END AS 'StarCategory'
		FROM business
		--FROM category c INNER JOIN business b ON c.business_id = b.id
		WHERE city = 'Scottsdale'
		ORDER BY stars;

-- Finding spread of star ratings in Scotssdale
	-- Identifying the inputs in 'stars'
		SELECT DISTINCT stars
		FROM business
		WHERE city = 'Scottsdale'
		ORDER BY stars desc

	-- Count organized around a single star value
		SELECT stars,
			COUNT(stars)
		FROM business
		WHERE city = 'Scottsdale' AND stars = 5
		GROUP BY stars

	-- Summary view of star input counts
		SELECT stars,
			COUNT(stars)
		FROM business
		WHERE city = 'Scottsdale'
		GROUP BY stars
		ORDER BY stars desc
	
-- Sample of joining the hours with buisness (Question i.) 

	SELECT *
	FROM business b INNER JOIN hours h ON b.id = h.business_id
	WHERE b.city = 'Scottsdale'
-- (Question i.) Analyzing hours for Scottsdale
	SELECT b.city,
		b.name,
		h.hours
	FROM business b 
		JOIN hours h ON b.id = h.business_id
	WHERE city = 'Scottsdale';
		+------------+--------------------------+-----------------------+
		| city       | name                     | hours                 |
		+------------+--------------------------+-----------------------+
		| Scottsdale | Taliesin West            | Monday|8:30-14:30     |
		| Scottsdale | Taliesin West            | Tuesday|8:30-17:00    |
		| Scottsdale | Taliesin West            | Friday|8:30-20:00     |
		| Scottsdale | Taliesin West            | Wednesday|8:30-17:00  |
		| Scottsdale | Taliesin West            | Thursday|8:30-14:30   |
		| Scottsdale | Taliesin West            | Sunday|8:30-15:00     |
		| Scottsdale | Taliesin West            | Saturday|8:30-15:00   |

		| Scottsdale | The Cider Mill           | Monday|10:00-18:00    |
		| Scottsdale | The Cider Mill           | Tuesday|10:00-18:00   |
		| Scottsdale | The Cider Mill           | Friday|10:00-20:00    |
		| Scottsdale | The Cider Mill           | Wednesday|10:00-18:00 |
		| Scottsdale | The Cider Mill           | Thursday|10:00-20:00  |
		| Scottsdale | The Cider Mill           | Sunday|11:00-16:00    |
		| Scottsdale | The Cider Mill           | Saturday|10:00-20:00  |

		| Scottsdale | Scent From Above Company | Friday|6:00-16:00     |
		| Scottsdale | Scent From Above Company | Tuesday|6:00-16:00    |
		| Scottsdale | Scent From Above Company | Thursday|6:00-16:00   |
		| Scottsdale | Scent From Above Company | Wednesday|6:00-16:00  |
		| Scottsdale | Scent From Above Company | Monday|6:00-16:00     |
		+------------+--------------------------+-----------------------+

-- (Question i.) Analyzing hours for Soul Food
	SELECT b.city,
		b.name,
		h.hours,
		--TO_CHAR(TRIM('#' FROM '%%') AS Day - 1, 'd')
		--TRIM('#' FROM '%') AS Day
		c.category
	FROM business b 
		INNER JOIN category c ON c.business_id = b.id 
		INNER JOIN hours h ON b.id = h.business_id
	WHERE c.category = 'Soul Food'
	ORDER BY hours;
		+---------------+-------------------------------+-----------------------+-----------+
		| city          | name                          | hours                 | category  |
		+---------------+-------------------------------+-----------------------+-----------+
		| North Randall | Oinky's Pork Chop Heaven      | Monday|6:00-23:00     | Soul Food |
		| North Randall | Oinky's Pork Chop Heaven      | Tuesday|6:00-23:00    | Soul Food |
		| North Randall | Oinky's Pork Chop Heaven      | Friday|6:00-23:00     | Soul Food |
		| North Randall | Oinky's Pork Chop Heaven      | Wednesday|6:00-23:00  | Soul Food |
		| North Randall | Oinky's Pork Chop Heaven      | Thursday|6:00-23:00   | Soul Food |
		| North Randall | Oinky's Pork Chop Heaven      | Sunday|6:00-23:00     | Soul Food |
		| North Randall | Oinky's Pork Chop Heaven      | Saturday|6:00-23:00   | Soul Food |

		| Phoenix       | Charlie D's Catfish & Chicken | Monday|11:00-18:00    | Soul Food |
		| Phoenix       | Charlie D's Catfish & Chicken | Tuesday|11:00-18:00   | Soul Food |
		| Phoenix       | Charlie D's Catfish & Chicken | Friday|11:00-18:00    | Soul Food |
		| Phoenix       | Charlie D's Catfish & Chicken | Wednesday|11:00-18:00 | Soul Food |
		| Phoenix       | Charlie D's Catfish & Chicken | Thursday|11:00-18:00  | Soul Food |
		| Phoenix       | Charlie D's Catfish & Chicken | Sunday|13:00-16:00    | Soul Food |
		| Phoenix       | Charlie D's Catfish & Chicken | Saturday|11:00-18:00  | Soul Food |
		+---------------+-------------------------------+-----------------------+-----------+

-- Taking a look at the highest rated businesses in Scottsdale

	SELECT name,
		review_count,
		stars,
		city,
		state
	FROM business
	WHERE city = 'Scottsdale'
	ORDER BY stars DESC, review_count DESC;

-- A count of Scottsdale businesses rated '5'

	SELECT COUNT(*)
	FROM business
	WHERE city = 'Scottsdale' AND stars = '5';

-- Calculating review numbers across different star ratings

	SELECT SUM(review_count) AS Number_of_Reviews
	FROM business
	WHERE city = 'Scottsdale' AND stars >= 3 AND stars < 4

	SELECT SUM(review_count) AS Number_of_Reviews
	FROM business
	WHERE city = 'Scottsdale' AND stars >= 2 AND stars < 3

-- Calculating the total number of reviews in Scottsdale

	SELECT SUM(review_count) AS Total_Scottsdale_Reviews
	FROM business
	WHERE city = 'Scottsdale'


-- Calculating the total volume of businesses with reviews in the city of Scottsdale, AZ

	SELECT COUNT(review_count) AS Total_Scottsdale_Reviews
	FROM business
	WHERE city = 'Scottsdale'

-- An average rating for the city of Scottsdale with accompanying star category 

	
	SELECT city,
		AVG(stars) AS AverageRating,
	CASE
		WHEN stars < 4 AND stars >= 2 THEN 'Average Rating'
		WHEN stars >= 4 THEN 'Top Rated'
		ELSE 'Low Rating'
	END AS 'StarCategories'
	FROM business
	WHERE city = 'Scottsdale'
	GROUP BY city;

---------
-- Joining in order to analyze the 'Soul' food category with our previous categorization system for star numbers
	SELECT b.city,
		b.name,
		c.category,
		b.stars,
	CASE
		WHEN stars >= 4 AND stars <= 5 THEN 'Top Rated'
		WHEN stars >= 3 AND stars < 4 THEN 'Average Rating'
		WHEN stars >= 2 AND stars < 3 THEN 'Average Rating'
		WHEN stars >= 1 AND stars < 2 THEN 'Low Rating'
		ELSE 'Not Relevant'
		END AS 'StarCategory'
	FROM category c INNER JOIN business b ON c.business_id = b.id
	WHERE c.category = 'Soul Food'
	ORDER BY stars DESC;

---------
-- I would have used both of the queries above to create views for further querying but was limited by using the in-browser querying with Coursera. Here is what it would have looked like:
	CREATE VIEW Scottsdale_Rating
	AS
		SELECT city,
			AVG(stars) AS AverageRating,
		CASE
			WHEN stars < 2 THEN 'Low Rating'
			WHEN stars < 4 AND stars >= 2 THEN 'Average Rating'
			WHEN stars >= 4 THEN 'Top Rated'
		END AS 'StarCategories'
		FROM business
		WHERE city = 'Scottsdale'
		GROUP BY city;

	CREATE VIEW Soul_Food_Rating
	AS
		SELECT
			c.category,
		AVG(b.stars) AS AverageRating,
		CASE
			WHEN stars < 2 THEN 'Low Rating'
			WHEN stars < 4 AND stars >= 2 THEN 'Average Rating'
			WHEN stars >= 4 THEN 'Top Rated'
		END AS 'Star_Category'
		FROM category c INNER JOIN business b ON c.business_id = b.id
		WHERE c.category = 'Soul Food'
		GROUP BY c.category;




---------
---------
---------
/* 2. Group business based on the ones that are open and the ones that are closed. What differences can you find between the ones that are still open and the ones that are closed? List at least two differences and the SQL code you used to arrive at your answer. */
		
/* i. Difference 1: */

         
/* ii. Difference 2: */
         
         
         
--SQL code used for analysis:




	
/* 3. For this last part of your analysis, you are going to choose the type of analysis you want to conduct on the Yelp dataset and are going to prepare the data for analysis. */

/* Ideas for analysis include: Parsing out keywords and business attributes for sentiment analysis, clustering businesses to find commonalities or anomalies between them, predicting the overall star rating for a business, predicting the number of fans a user will have, and so on. These are just a few examples to get you started, so feel free to be creative and come up with your own problem you want to solve. */

-- Provide answers, in-line, to all of the following: 
	
/* i. Indicate the type of analysis you chose to do: */
         
         
/* ii. Write 1-2 brief paragraphs on the type of data you will need for your analysis and why you chose that data: */
                           
                  
/* iii. Output of your finished dataset: */
         
         
/* iv. Provide the SQL code you used to create your final dataset: */







