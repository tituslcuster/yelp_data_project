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
	
	/* Copy and Paste the Result Below: */
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
	Posting more reviews does not directly correlate with more fans. 

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
	
		
  -----------------------------------
--| Part 2: Inferences and Analysis |--
  -----------------------------------

/* 1. Pick one city and category of your choice and group the businesses in that city or category by their overall star rating.Compare the businesses with 2-3 stars to the businesses with 4-5 stars and answer the following questions. */

-- My choices for further analysis
		City: Scottsdale
		Category: Soul Food
		4.0 - 5.0 stars = 'Top Rated'
		2.0 - 3.9 stars = 'Average Rating'
		1.0 - 1.9 stars = 'Low Rating'

** NOTE REGARDING 'StarCategories': Values are derived from a count of businesses that have a finalized star rating independant of the review count. This means that the total count of these queries should amount back to the total amount of businesses in the database (10,000).**

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
			((Total amount of rated businesses: 395))

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

-- Soul Food
	-- There are only two 'Soul Food' businesses
		+---------------+-------------------------------+-----------+-------+----------------+
		| city          | name                          | category  | stars | StarCategory   |
		+---------------+-------------------------------+-----------+-------+----------------+
		| Phoenix       | Charlie D's Catfish & Chicken | Soul Food |   4.5 | Top Rated      |
		| North Randall | Oinky's Pork Chop Heaven      | Soul Food |   3.0 | Average Rating |
		+---------------+-------------------------------+-----------+-------+----------------+

/* i. Do the two groups you chose to analyze have a different distribution of hours? */
	The businesses in Scotssdale with listed hours show much more variablitiy in their hours while the 'Soul Food' businesses have much more consistant hours.
	Opening times remain largely the same across all businesses, with the only exceptions coming on weekends.
	In fact the only changes in opening time occured on Sundays.
	'The Cider Mill' in Scottsdale and the other 'Charlie Ds Catfish & Chicken' a Soul Food business were the only two businesses with fluctuation in opening time.
	Closing times displayed the highest range of variablitiy across the entire dataset, but largely for businesses in Scottsdale.
	Soul Food businesses generally did not change their closing times with only one exception on a single day.
	'Charlie Ds Catfish & Chicken' had the only closing and opening time fluctuation for a Soul Food business occuring all on Sunday.
	Two businesses did not have any weekly changes to their hours: 'Scent From Above Company' in Scottsdale and the Soul Food business 'Oinkys Pork Chop Heaven'

/* ii. Do the two groups you chose to analyze have a different number of reviews? */
    Both groups show very different sizes of reviews due to the wide range in sample sizes. 
	There have been 20,614 reviews in the city of Scottsdale, and there have been 10 reviews for the 'Soul Food' business category.
         
/* iii. Are you able to infer anything from the location data provided between these two groups? Explain. */
	There is nothing to infer based off of location since both of the two 'Soul Food' businesses are located in seperate cities (North Randall, and Phoenix)

	Attempting to match the constraints of the two 'Soul Food' businesses to the larger sample of Scottsdale businesses produced no crossover of latitude or longitude.

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
-- (Question i.) Analyzing hours for each group 
	-- Scottsdale
		SELECT b.city,
			b.name,
			h.hours,
			SUBSTR(hours, 1, 3) dayopen
			--SUBSTR(hours, CHARINDEX('|',hours)+1, LEN(hours)) timeopen
			--SUBSTR(hours, POSITION('|',hours)+1, LEN(hours)) timeopen
			--SUBSTR(hours, LEGNTH(hours) - ) timeopen
			--None of the above options work because we are using SQLite
		FROM business b 
			INNER JOIN category c ON c.business_id = b.id 
			INNER JOIN hours h ON b.id = h.business_id
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

	-- Soul Food
		SELECT b.city,
			b.name,
			h.hours,
			SUBSTR(hours, 1, 3) dayopen
		FROM business b 
			INNER JOIN category c ON c.business_id = b.id 
			INNER JOIN hours h ON b.id = h.business_id
		WHERE c.category = 'Soul Food';
		+---------------+-------------------------------+-----------------------+---------+
		| city          | name                          | hours                 | dayopen |
		+---------------+-------------------------------+-----------------------+---------+
		| North Randall | Oinky's Pork Chop Heaven      | Monday|6:00-23:00     | Mon     |
		| North Randall | Oinky's Pork Chop Heaven      | Tuesday|6:00-23:00    | Tue     |
		| North Randall | Oinky's Pork Chop Heaven      | Friday|6:00-23:00     | Fri     |
		| North Randall | Oinky's Pork Chop Heaven      | Wednesday|6:00-23:00  | Wed     |
		| North Randall | Oinky's Pork Chop Heaven      | Thursday|6:00-23:00   | Thu     |
		| North Randall | Oinky's Pork Chop Heaven      | Sunday|6:00-23:00     | Sun     |
		| North Randall | Oinky's Pork Chop Heaven      | Saturday|6:00-23:00   | Sat     |

		| Phoenix       | Charlie D's Catfish & Chicken | Monday|11:00-18:00    | Mon     |
		| Phoenix       | Charlie D's Catfish & Chicken | Tuesday|11:00-18:00   | Tue     |
		| Phoenix       | Charlie D's Catfish & Chicken | Friday|11:00-18:00    | Fri     |
		| Phoenix       | Charlie D's Catfish & Chicken | Wednesday|11:00-18:00 | Wed     |
		| Phoenix       | Charlie D's Catfish & Chicken | Thursday|11:00-18:00  | Thu     |
		| Phoenix       | Charlie D's Catfish & Chicken | Sunday|13:00-16:00    | Sun     |
		| Phoenix       | Charlie D's Catfish & Chicken | Saturday|11:00-18:00  | Sat     |
		+---------------+-------------------------------+-----------------------+---------+
-- (Question ii.) Analyzing the volume of reviews for each group
	-- Scottsdale
	SELECT SUM(review_count) AS Number_of_Reviews
	FROM business
	WHERE city = 'Scottsdale'
	
	-- Soul Food
	SELECT SUM(review_count) AS Number_of_Reviews
	FROM category c INNER JOIN business b ON c.business_id = b.id
	WHERE category = 'Soul Food';

-- (Question iii.) Analyzing location data for inference
	-- Scottsdale lat. and long. (trying to adjust for Soul Food lat and long)
		SELECT DISTINCT b.name,
			b.address,
			b.latitude,
			b.longitude
		FROM business b 
			--INNER JOIN category c ON c.business_id = b.id 
		WHERE city = 'Scottsdale'
			AND latitude < 42
			AND latitude > 33
			AND longitude < 113
			AND longitude > 81;

		+------+---------+----------+-----------+
		| name | address | latitude | longitude |
		+------+---------+----------+-----------+
		+------+---------+----------+-----------+
		(Zero rows)


	-- Soul Food lat. and long.
		SELECT DISTINCT b.name,
			b.latitude,
			b.longitude
		FROM business b 
			INNER JOIN category c ON c.business_id = b.id 
		WHERE c.category = 'Soul Food';

		+-------------------------------+----------+-----------+
		| name                          | latitude | longitude |
		+-------------------------------+----------+-----------+
		| Oinky's Pork Chop Heaven      |  41.4352 |  -81.5214 |
		| Charlie D's Catfish & Chicken |  33.4468 |  -112.057 |
		+-------------------------------+----------+-----------+

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
-- I would have used both of the queries above to create views for further querying but was limited by using the in-browser querying with Coursera (this also might be because of SQLite). Here is what it would have looked like:
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

-- Summary Data --
	Closed businesses: 1,520 (35,261 reviews)
	Open businesses: 8,480 (269,300 reviews)
		(Total businesses: 10,000)

	Explore the following for analysis:
		1. number of reviews
		2. spread of stars
	
/* i. Difference 1: */
The disparity in number of reviews as it relates to closed vs. open businesses is quite vast. Here are two reasons that I would infer:
	1. There are more open businesses than closed businesses
	2. Open businesses are still generating new reviews and closed businesses are not

There is no limit to the ammount of reviews that any one business could potentially generate compared to another, but when a business is closed that progress is frozen in time creating a widening disparity between open and closed.

A great measure for further analysis would be the volume of reviews written during final three to four months of a business before it closes. Are there more reviews written (likely due to a potential flaw or vulnerabilty of their business), or would reviews plumet as a result of wanning interest by patrons?

Fortunatly the next difference will give some insight as it relates to this dataset!
         
/* ii. Difference 2: */
** NOTE: The following numbers are derived from a count of businesses that have a finalized star rating independant of the review count. This means that the total count of these queries should amount back to the total amount of businesses in the database (10,000).**

Unsuprisingly, open businesses had more 'top' ratings than closed businesses did, but what did suprise was the low volume of 'low' rating businesses that were closed. The largest grouping of ratings for closed businesses came from the 'average' category. Most likely this happened as a result of 'average' containing a wider spread of ratings (spanning two ratings!) skewing it slightly, but a mere 38 'low' rated businesses really displays just how far off my assumptions about review score were as they relates to businesses success. Based soley off of these numbers it looks like there is no discernable correlation between being rated 'low' and your business closing as it pertains to this dataset alone.

	Low Rated Open: 324
	Average Rated Open: 3,801
	Top Rated Open: 4,355
	-----
	Low Rated Closed: 38
	Average Rated Closed: 829
	Top Rated Closed: 653

         
/* SQL code used for analysis: */
	SELECT DISTINCT is_open
	FROM business

	SELECT COUNT(name)
	FROM business
	-- WHERE is_open = 0
	-- WHERE is_open = 1

-- Examining number of reviews 
	SELECT 
		CASE 
			WHEN is_open = 1 THEN 'Open'
			WHEN is_open = 0 THEN 'Closed'
			ELSE 'Unknown Status'
		END AS 'Open/Closed',
		SUM(review_count) AS Review_Count
	FROM business
	GROUP BY is_open
	ORDER BY Review_Count DESC;

		+-------------+--------------+
		| Open/Closed | Review_Count |
		+-------------+--------------+
		| Open        |      269,300 |
		| Closed      |       35,261 |
		+-------------+--------------+

-- Examining spread of stars
	SELECT COUNT(name) AS 'Low Rated & Open Businesses'
	FROM business
	WHERE is_open = 1
		AND stars < 2
		+-----------------------------+
		| Low Rated & Open Businesses |
		+-----------------------------+
		|                         324 |
		+-----------------------------+
	
	SELECT COUNT(name) AS 'Average Rated & Open Businesses'
	FROM business
	WHERE is_open = 1
		AND stars < 4 
		AND stars >= 2
		+---------------------------------+
		| Average Rated & Open Businesses |
		+---------------------------------+
		|                            3801 |
		+---------------------------------+

	SELECT COUNT(name) AS 'Top Rated & Open Businesses'
	FROM business
	WHERE is_open = 1
		AND stars >= 4
		+-----------------------------+
		| Top Rated & Open Businesses |
		+-----------------------------+
		|                        4355 |
		+-----------------------------+

	SELECT COUNT(name) AS 'Low Rated & Closed Businesses'
	FROM business
	WHERE is_open = 0
		AND stars < 2
		+-------------------------------+
		| Low Rated & Closed Businesses |
		+-------------------------------+
		|                            38 |
		+-------------------------------+

	SELECT COUNT(name) AS 'Average Rated & Closed Businesses'
	FROM business
	WHERE is_open = 0
		AND stars < 4 
		AND stars >= 2
		+-----------------------------------+
		| Average Rated & Closed Businesses |
		+-----------------------------------+
		|                               829 |
		+-----------------------------------+

	SELECT COUNT(name) AS 'Top Rated & Closed Businesses'
	FROM business
	WHERE is_open = 0
		AND stars >= 4
		+-------------------------------+
		| Top Rated & Closed Businesses |
		+-------------------------------+
		|                           653 |
		+-------------------------------+


---------
---------
---------
	
/* 3. For this last part of your analysis, you are going to choose the type of analysis you want to conduct on the Yelp dataset and are going to prepare the data for analysis. */

/* Ideas for analysis include: Parsing out keywords and business attributes for sentiment analysis, clustering businesses to find commonalities or anomalies between them, predicting the overall star rating for a business, predicting the number of fans a user will have, and so on. These are just a few examples to get you started, so feel free to be creative and come up with your own problem you want to solve. */
	
/* i. Indicate the type of analysis you chose to do: */
         I will be clustering businesses around categories of stars in order to find commonalities and anomalies between them.

		 The star categories are as following:
				  5.0 stars = 'Premium Rated'
			4.0 - 4.9 stars = 'Top Rated'
			3.0 - 3.9 stars = 'High Rating'
			2.0 - 2.9 stars = 'Average Rating'
			1.0 - 1.9 stars = 'Low Rating'
         
/* ii. Write 1-2 brief paragraphs on the type of data you will need for your analysis and why you chose that data: */
	 For the purposes of this assignment I will be analyzing only the 'premium rated' businesses against the number of reviews that they contain. Comparisons for this analysis project will be ran based on city, state, and open/closed status along with querying for outliers and summary data to get a good grasp on the larger points of this dataset. Sampleing the top ten of different variations of the dataset should be effective in examining the differences that take place at the largest volumes of star category and review count. 
	 
 -- General inferences and hypotheticals
	 City and state provide different geographical contexts for business success which I am sure will have some degree of variance for comparision with such a large volume of businesses to draw from at the city or state level. Open and closed status of the business will create even more intriguing insight into the lifecycle of businesses within this dataset. My previous assumptions about open/closed status was challenged in the last question, so I anticipate even more suprises moving forward. My assumptions are that there will be only slight variance at the state level, while cities will see much higher variance in star category. Since I am now adding a split between 'average' and 'high' I would bet that most closed businesses would be in the 'average' category despite the data displaying their absence from the 'low' category.
	
/* iii. Output of your finished dataset: */
	None of the businesses within the top 10 most reviewed on Yelp had anything above a 4.5. The same group didnt dip past 2.5 as the lowest rating. I would infer that this range of rating could be the result of the prime motivator behind a consumer reviewing a business on Yelp. The main reasons why a consumer would take time to review a business on Yelp is because they had an experience that they wanted to share with other people. While average ratings for businesses might display more volume in the 'average' and 'low' categories, an area for further analysis would be if individual reviews tend more towards wide swings in star category compared to the average rating of the business.

	Regardless, among 'premium' businesses review count held a strong correlation with large volume of 'premium' counts across city and state metrics. The city with the most 'premium' businesses (Las Vegas) also held a strong lead ahead of all other cities in review count (diffrences of 48,351 for review count and 560 'premium' businesses). Of course Las Vegas as notable worldwide center for adult entertainment might influence its large lead ahead of other cities and states. The top ten cities displayed slight variability in position of the top ten most premium businesses compared to the ten most reviewed cities, but largely the same cities remained on the list with the exception of Chandler, AZ and Mesa, AZ.

	Similar to what the top ten cities with the highest review count and 'premium' business count revealed, the top ten groupings by state revealed little variability. The three states at the top of the list held the same position across both aggregations. In fact the top nine states were all the same states across review count and 'premium' business count with the only position change occuring at the fourth and fifth spots across lists alternating between North Carolina and Ohio. The tenth spot changed dramatically across lists. This most likely occured due to the same trend affecting both tables: 'premium' businesses do not numerically have a larger decrease in count moving down the list, but their decrease wildly displaces the amount of reviews. As a result of this the number of reviews becomes much less stable as 'premium' volume decreases. This is because the 'TotalNumberOfReviews' column is unaffected by number of stars meaning that the less that the 'premium' can impact the number of reviews, the less likely there will be a correlation between the two constraints.
/* iv. Provide the SQL code you used to create your final dataset: */
	SELECT city
		,name
		,stars
		,is_open
			,CASE
			WHEN stars = 5 THEN 'Premium Rated'
			WHEN stars >= 4 AND stars < 5 THEN 'Top Rated'
			WHEN stars >= 3 AND stars < 4 THEN 'High Rating'
			WHEN stars >= 2 AND stars < 3 THEN 'Average Rating'
			WHEN stars >= 1 AND stars < 2 THEN 'Low Rating'
			ELSE 'This is weird.. check on this one'
		END AS 'StarCategory'
	FROM business
	ORDER BY Star_Category DESC, is_open DESC;


-- Closed premium rated businesses  
	SELECT name
		,stars
		,review_count
		,CASE
			WHEN stars = 5 THEN 'Premium Rated'
			WHEN stars >= 4 AND stars < 5 THEN 'Top Rated'
			WHEN stars >= 3 AND stars < 4 THEN 'High Rating'
			WHEN stars >= 2 AND stars < 3 THEN 'Average Rating'
			WHEN stars >= 1 AND stars < 2 THEN 'Low Rating'
			ELSE 'This is weird.. check on this one'
		END AS 'StarCategory'
	FROM business
	WHERE is_open = 0 
		AND StarCategory = 'Premium Rated'
	ORDER BY stars DESC, review_count DESC;

	+----------------------------------+-------+--------------+---------------+
	| name                             | stars | review_count | StarCategory  |
	+----------------------------------+-------+--------------+---------------+
	| Dethrone Basecamp Fitness Studio |   5.0 |           47 | Premium Rated |
	| Eleve Training Studios           |   5.0 |           37 | Premium Rated |
	| 305 Kustoms                      |   5.0 |           28 | Premium Rated |
	| Imbibe Tours                     |   5.0 |           18 | Premium Rated |
	| Pet Club Northern                |   5.0 |           17 | Premium Rated |
	| Mother's Deli & Bakery           |   5.0 |           16 | Premium Rated |
	| Arizona Painting and Decorating  |   5.0 |           15 | Premium Rated |
	| The Muffin Girl                  |   5.0 |           15 | Premium Rated |
	| Great Skin Rules                 |   5.0 |           14 | Premium Rated |
	| Clothes Minded                   |   5.0 |           13 | Premium Rated |
	| Oliver & Annie                   |   5.0 |           13 | Premium Rated |
	| Nectar Natural Infusions         |   5.0 |           11 | Premium Rated |
	| Your Feel Good Soap Company      |   5.0 |           11 | Premium Rated |
	| Pick N Puff                      |   5.0 |           10 | Premium Rated |
	| NextRelic                        |   5.0 |            9 | Premium Rated |
	| Salon Bespoke                    |   5.0 |            9 | Premium Rated |
	| AccentU                          |   5.0 |            9 | Premium Rated |
	| Port of Siam                     |   5.0 |            9 | Premium Rated |
	| Chic Ink Boutique                |   5.0 |            9 | Premium Rated |
	| Binz                             |   5.0 |            9 | Premium Rated |
	| Patrick's Stop                   |   5.0 |            9 | Premium Rated |
	| ESports Gaming Arena             |   5.0 |            7 | Premium Rated |
	| J and C Glass Studio             |   5.0 |            7 | Premium Rated |
	| Las Vegas Hair & Nails           |   5.0 |            7 | Premium Rated |
	| Chancery Lane Salon              |   5.0 |            7 | Premium Rated |
	+----------------------------------+-------+--------------+---------------+
	(Output limit exceeded, 25 of 138 total rows shown)

-- Open premium businesses (same as above but changing the WHERE is_open from '0' to '1')
	+--------------------------------------+-------+--------------+---------------+
	| name                                 | stars | review_count | StarCategory  |
	+--------------------------------------+-------+--------------+---------------+
	| Green Corner Restaurant              |   5.0 |          267 | Premium Rated |
	| La Maison de Maggie                  |   5.0 |          260 | Premium Rated |
	| UpTown Barbershop                    |   5.0 |          176 | Premium Rated |
	| Master Lock & Security               |   5.0 |          153 | Premium Rated |
	| Tidy Casa                            |   5.0 |          149 | Premium Rated |
	| SPEEDVEGAS                           |   5.0 |          125 | Premium Rated |
	| LasikPlus Vision Center              |   5.0 |          115 | Premium Rated |
	| Loris Grooming                       |   5.0 |          114 | Premium Rated |
	| LunchboxWax Scottsdale               |   5.0 |          112 | Premium Rated |
	| Chula Seafood                        |   5.0 |          106 | Premium Rated |
	| Las Vegas Collision Center           |   5.0 |          103 | Premium Rated |
	| Arbor Care Tree Service              |   5.0 |           91 | Premium Rated |
	| Dog Supplies Outlet                  |   5.0 |           85 | Premium Rated |
	| Sheffield Spice & Tea Co             |   5.0 |           84 | Premium Rated |
	| West Coast Tattoo Parlor             |   5.0 |           84 | Premium Rated |
	| Barefoot Pools Pool Service & Repair |   5.0 |           83 | Premium Rated |
	| Desert Customs Window Tinting        |   5.0 |           81 | Premium Rated |
	| Lora Moon Styling                    |   5.0 |           79 | Premium Rated |
	| Pearl. Dentistry Reimagined          |   5.0 |           79 | Premium Rated |
	| Just Smiles                          |   5.0 |           79 | Premium Rated |
	| Camp Bow Wow Avondale                |   5.0 |           79 | Premium Rated |
	| Doggy Daze                           |   5.0 |           77 | Premium Rated |
	| Simply Natural Nails                 |   5.0 |           74 | Premium Rated |
	| Plumbing Kings                       |   5.0 |           73 | Premium Rated |
	| Phoenix Mountain Animal Hospital     |   5.0 |           72 | Premium Rated |
	+--------------------------------------+-------+--------------+---------------+
	(Output limit exceeded, 25 of 1427 total rows shown)

-- Top ten states with the most premium businesses
	SELECT state,
		COUNT(stars = 5) AS Premium_Business_Count,
		SUM(review_count) AS TotalNumberOfReviews
	FROM  business
	GROUP BY state
	ORDER BY Premium_Business_Count DESC
	LIMIT 10;

	+-------+------------------------+----------------------+
	| state | Premium_Business_Count | TotalNumberOfReviews |
	+-------+------------------------+----------------------+
	| AZ    |                   3042 |               100548 |
	| NV    |                   1921 |                96494 |
	| ON    |                   1664 |                36373 |
	| OH    |                    747 |                14814 |
	| NC    |                    722 |                17140 |
	| PA    |                    553 |                13211 |
	| QC    |                    465 |                10738 |
	| WI    |                    253 |                 6410 |
	| EDH   |                    237 |                 2742 |
	| BW    |                    202 |                 2412 |
	+-------+------------------------+----------------------+

-- Top ten states with the most reviews
	SELECT state,
		COUNT(stars = 5) AS Premium_Business_Count,
		SUM(review_count) AS TotalNumberOfReviews
	FROM  business
	GROUP BY state
	ORDER BY TotalNumberOfReviews DESC
	LIMIT 10;
	+-------+------------------------+----------------------+
	| state | Premium_Business_Count | TotalNumberOfReviews |
	+-------+------------------------+----------------------+
	| AZ    |                   3042 |               100548 |
	| NV    |                   1921 |                96494 |
	| ON    |                   1664 |                36373 |
	| NC    |                    722 |                17140 |
	| OH    |                    747 |                14814 |
	| PA    |                    553 |                13211 |
	| QC    |                    465 |                10738 |
	| WI    |                    253 |                 6410 |
	| EDH   |                    237 |                 2742 |
	| IL    |                    108 |                 2571 |
	+-------+------------------------+----------------------+


-- Top ten cities with the most premium businesses
	SELECT state,
		city,
		COUNT(stars = 5) AS Premium_Business_Count,
		SUM(review_count) AS TotalNumberOfReviews
	FROM  business
	GROUP BY city
	ORDER BY Premium_Business_Count DESC
	LIMIT 10;

	+-------+------------+------------------------+----------------------+
	| state | city       | Premium_Business_Count | TotalNumberOfReviews |
	+-------+------------+------------------------+----------------------+
	| NV    | Las Vegas  |                   1561 |                82854 |
	| AZ    | Phoenix    |                   1001 |                34503 |
	| ON    | Toronto    |                    985 |                24113 |
	| AZ    | Scottsdale |                    497 |                20614 |
	| NC    | Charlotte  |                    468 |                12523 |
	| PA    | Pittsburgh |                    353 |                 9798 |
	| QC    | Montréal   |                    337 |                 9448 |
	| AZ    | Mesa       |                    304 |                 6875 |
	| NV    | Henderson  |                    274 |                10871 |
	| AZ    | Tempe      |                    261 |                10504 |
	+-------+------------+------------------------+----------------------+

-- Top ten cities with the most reviews
	SELECT state,
		city,
		COUNT(stars = 5) AS Premium_Business_Count,
		SUM(review_count) AS TotalNumberOfReviews
	FROM  business
	GROUP BY city
	ORDER BY TotalNumberOfReviews DESC
	LIMIT 10;
	
	+-------+------------+------------------------+----------------------+
	| state | city       | Premium_Business_Count | TotalNumberOfReviews |
	+-------+------------+------------------------+----------------------+
	| NV    | Las Vegas  |                   1561 |                82854 |
	| AZ    | Phoenix    |                   1001 |                34503 |
	| ON    | Toronto    |                    985 |                24113 |
	| AZ    | Scottsdale |                    497 |                20614 |
	| NC    | Charlotte  |                    468 |                12523 |
	| NV    | Henderson  |                    274 |                10871 |
	| AZ    | Tempe      |                    261 |                10504 |
	| PA    | Pittsburgh |                    353 |                 9798 |
	| QC    | Montréal   |                    337 |                 9448 |
	| AZ    | Chandler   |                    232 |                 8112 |
	+-------+------------+------------------------+----------------------+
-- Finding a set of businesses with the highest and lowest number of ratings for a better picture of review volume
	SELECT name, review_count
	FROM business
	ORDER BY review_count DESC
	LIMIT 10;

	+----------------------+--------------+
	| name                 | review_count |
	+----------------------+--------------+
	| The Buffet           |         3873 |
	| Schwartz's           |         1757 |
	| Joe's Farm Grill     |         1549 |
	| Carson Kitchen       |         1410 |
	| Delmonico Steakhouse |         1389 |
	| Le Thai              |         1252 |
	| Scarpetta            |         1116 |
	| Diablos Cantina     |         1084 |
	| MGM Grand Buffet     |          961 |
	| Joyride Taco House   |          902 |
	+----------------------+--------------+

-- Coupling the last query with star information for categorical insight
	SELECT name 
		,review_count
		,stars
		,CASE
			WHEN stars = 5 THEN 'Premium Rated'
			WHEN stars >= 4 AND stars < 5 THEN 'Top Rated'
			WHEN stars >= 3 AND stars < 4 THEN 'High Rating'
			WHEN stars >= 2 AND stars < 3 THEN 'Average Rating'
			WHEN stars >= 1 AND stars < 2 THEN 'Low Rating'
			ELSE 'This is weird.. check on this one'
		END AS 'StarCategory'
	FROM business
	ORDER BY review_count DESC
	LIMIT 10;

	+----------------------+--------------+-------+----------------+
	| name                 | review_count | stars | StarCategory   |
	+----------------------+--------------+-------+----------------+
	| The Buffet           |         3873 |   3.5 | High Rating    |
	| Schwartz's           |         1757 |   4.0 | Top Rated      |
	| Joe's Farm Grill     |         1549 |   4.0 | Top Rated      |
	| Carson Kitchen       |         1410 |   4.5 | Top Rated      |
	| Delmonico Steakhouse |         1389 |   4.0 | Top Rated      |
	| Le Thai              |         1252 |   4.0 | Top Rated      |
	| Scarpetta            |         1116 |   4.0 | Top Rated      |
	| Diablos Cantina      |         1084 |   3.0 | High Rating    |
	| MGM Grand Buffet     |          961 |   2.5 | Average Rating |
	| Joyride Taco House   |          902 |   4.0 | Top Rated      |
	+----------------------+--------------+-------+----------------+

-- Adding city and state information for further insight
		SELECT name 
		,review_count
		,stars
		,CASE
			WHEN stars = 5 THEN 'Premium Rated'
			WHEN stars >= 4 AND stars < 5 THEN 'Top Rated'
			WHEN stars >= 3 AND stars < 4 THEN 'High Rating'
			WHEN stars >= 2 AND stars < 3 THEN 'Average Rating'
			WHEN stars >= 1 AND stars < 2 THEN 'Low Rating'
			ELSE 'This is weird.. check on this one'
		END AS 'StarCategory'
		,state
		,city
	FROM business
	ORDER BY review_count DESC
	LIMIT 10;

	+----------------------+--------------+-------+----------------+-------+-----------+
	| name                 | review_count | stars | StarCategory   | state | city      |
	+----------------------+--------------+-------+----------------+-------+-----------+
	| The Buffet           |         3873 |   3.5 | High Rating    | NV    | Las Vegas |
	| Schwartz's           |         1757 |   4.0 | Top Rated      | QC    | Montréal  |
	| Joe's Farm Grill     |         1549 |   4.0 | Top Rated      | AZ    | Gilbert   |
	| Carson Kitchen       |         1410 |   4.5 | Top Rated      | NV    | Las Vegas |
	| Delmonico Steakhouse |         1389 |   4.0 | Top Rated      | NV    | Las Vegas |
	| Le Thai              |         1252 |   4.0 | Top Rated      | NV    | Las Vegas |
	| Scarpetta            |         1116 |   4.0 | Top Rated      | NV    | Las Vegas |
	| Diablos Cantina      |         1084 |   3.0 | High Rating    | NV    | Las Vegas |
	| MGM Grand Buffet     |          961 |   2.5 | Average Rating | NV    | Las Vegas |
	| Joyride Taco House   |          902 |   4.0 | Top Rated      | AZ    | Gilbert   |
	+----------------------+--------------+-------+----------------+-------+-----------+

-- States with the highest stars
	SELECT state,
		SUBSTR(AVG(stars),1,4) AS AverageStarRating
	FROM business
	GROUP BY state
	ORDER BY AverageStarRating DESC;

	+-------+-------------------+
	| state | AverageStarRating |
	+-------+-------------------+
	| ST    | 5.0               |
	| ELN   | 4.16              |
	| MLN   | 3.87              |
	| EDH   | 3.78              |
	| BW    | 3.75              |
	| PA    | 3.74              |
	| AZ    | 3.72              |
	| NV    | 3.72              |
	| WI    | 3.70              |
	| FIF   | 3.7               |
	| HLD   | 3.66              |
	| NYK   | 3.66              |
	| QC    | 3.63              |
	| OH    | 3.58              |
	| NC    | 3.56              |
	| SC    | 3.52              |
	| IL    | 3.50              |
	| ESX   | 3.5               |
	| NY    | 3.5               |
	| WLN   | 3.5               |
	| ON    | 3.46              |
	| C     | 3.33              |
	| NI    | 3.0               |
	+-------+-------------------+
----
---- If I could use views I would have created one like I did below and then use StarCategories against it
	CREATE VIEW AverageStateStars
	AS	
		SELECT state,
			SUBSTR(AVG(stars),1,4) AS AverageStarRating
		FROM business
		GROUP BY state
		ORDER BY AverageStarRating DESC;
----
--