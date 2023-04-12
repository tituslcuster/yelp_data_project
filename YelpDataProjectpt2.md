# Part 2 -- Inferences and Analysis
This SQL project was a final assignment for the Couersera course "[SQL for Data Science](https://www.coursera.org/account/accomplishments/verify/K93FFFBPQ9AC)". 

This document covers part 2 of the assignment and contains some questions for deeper explorartory data analysis that interested me. Unlike part one, the scope of this document is much more focused and practical.

## Index
i. Summary Data for the questions
1. Question #1 -- Is there a difference in distribution of hours for businesses in Scottsdale, AZ compared to 'Soul Food' businesses?
2. Question #2 -- Is there a difference in number of reviews for businesses in Scottsdale, AZ compared to 'Soul Food' businesses?
3. Question #3 -- Can anything be inferred from the location data provided between businesses in Scottsdale, AZ and 'Soul Food' businesses?
4. Question #4 -- What differences are there between open and closed businesses?
5. Question #5 -- What are the commonalities and anomalies between businesses clustered into 'Star Categories'?

## i. -- Summary Data

### Scottsdale
Spread of ratings in the city of Scottsdale
+ Scottsdale **'Premium Rated'** businesses: 137 *(2,094 total reviews)*
+ Scorrsdale **'High Rated'** businesses: 104 *(10,594 total reviews)*
+ Scottsdale **'Top Rated'** businesses: 188 *(6,730 total reviews)*
+ Scottsdale **'Average Rating'** businesses: 157 *(1,050 total reviews)*
+ Scottsdale **'Low Rating'** businesses: 15 *(146 total reviews)*

*(Total number of rated businesses in Scottsdale: 497)*
*(Total number of Yelp reviews in Scottsdale: 20,614)*

+ Average Scottsdale Rating: 3.95 ('Average Rating' Category)

These summary data points came from simple SUM and COUNT functioning.
```sql
-- Counting 'Top Rated' businesses in Scottsdale
	SELECT stars,
        COUNT(stars) AS Num_Top
    FROM business
    WHERE city = 'Scottsdale' 
        AND stars >= 4 
        AND stars < 5
```
```sql
-- Totaling review counts based on star category
    SELECT SUM(review_count)
    FROM business
    WHERE city = 'Scottsdale'
        AND stars = 5
```

Viewing these summary data points gives a high level view of how many businesses in Scottsdale fall into each individual star_category and how those rankings are affected by the number of reviews at a 1,000 foot view

### Soul Food

Unfortunately the 'Soul Food' Yelp category only returned two rated businesses, a far cry from the 395 rated businesses from the city of Scottsdale. The aggregate star rating for each of these businesses were included under the 'stars' column.
```sql
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
```
```		
    +---------------+-------------------------------+-----------+-------+----------------+
    | city          | name                          | category  | stars | StarCategory   |
    +---------------+-------------------------------+-----------+-------+----------------+
    | Phoenix       | Charlie D's Catfish & Chicken | Soul Food |   4.5 | Top Rated      |
    | North Randall | Oinky's Pork Chop Heaven      | Soul Food |   3.0 | Average Rating |
    +---------------+-------------------------------+-----------+-------+----------------+
```
(Total number of 'Soul Food' Reviews: 10)

+ Average Soul Food Rating: 3.75 ('Top Rated' Category)

### Closed and Open Businesses
+ Closed businesses: 1,520 (35,261 reviews)
+ Open businesses: 8,480 (269,300 reviews)
(Total businesses: 10,000)

```sql
	SELECT DISTINCT is_open
	FROM business

	SELECT COUNT(name)
	FROM business
	-- WHERE is_open = 0
	-- WHERE is_open = 1
```

## 1. Is there a difference in distribution of hours for businesses in Scottsdale, AZ compared to 'Soul Food' businesses?
The businesses in Scotssdale with listed hours show much more variablitiy in their hours while the 'Soul Food' businesses have much more consistant hours. Opening times remain largely the same across all businesses, with the only exceptions coming on weekends.  In fact the only changes in opening time occured on Sundays.'The Cider Mill' in Scottsdale and the other 'Charlie Ds Catfish & Chicken' a Soul Food business were the only two businesses with fluctuation in opening time.

```
    +---------------+--------------------------+-----------------------+
    | city          | name                     | hours                 |
    +---------------+--------------------------+-----------------------+
    | Scottsdale    | The Cider Mill           | Sunday|11:00-16:00    |
    | North Randall | Oinky's Pork Chop Heaven | Sunday|6:00-23:00     |
    +---------------+--------------------------+-----------------------+


```

Closing times displayed the highest range of variablitiy across the entire dataset, but largely for businesses in Scottsdale. Soul Food businesses generally did not change their closing times with only one exception on a single day. 'Charlie Ds Catfish & Chicken' had the only closing and opening time fluctuation for a Soul Food business occuring all on Sunday. 

Two businesses did not have any weekly changes to their hours: 'Scent From Above Company' in Scottsdale and the Soul Food business 'Oinkys Pork Chop Heaven'
### (Question 1) Analyzing hours for each group 
Scottsdale
```sql
	SELECT b.city,
		b.name,
		h.hours,
		SUBSTR(hours, 1, 3) dayopen
		--SUBSTR(hours, CHARINDEX('|',hours)+1, LEN(hours)) timeopen
		--SUBSTR(hours, POSITION('|',hours)+1, LEN(hours)) timeopen
		--SUBSTR(hours, LEGNTH(hours) - ) timeopen
	FROM business b 
		INNER JOIN category c ON c.business_id = b.id 
		INNER JOIN hours h ON b.id = h.business_id
	WHERE city = 'Scottsdale';
```
```
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
```
Soul Food
```sql
		SELECT b.city,
			b.name,
			h.hours,
			SUBSTR(hours, 1, 3) dayopen
		FROM business b 
			INNER JOIN category c ON c.business_id = b.id 
			INNER JOIN hours h ON b.id = h.business_id
		WHERE c.category = 'Soul Food';
```
```
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
```

## 2. Is there a difference in number of reviews for businesses in Scottsdale, AZ compared to 'Soul Food' businesses?
Both groups show very different sizes of reviews due to the wide range in sample sizes. It is important to remember that there are 497 businesses in the city of Scottsdale that are being comparing to only 2 businesses classified as 'Soul Food' businesses. 

```sql
	-- Scottsdale
	SELECT SUM(review_count) AS Number_of_Reviews
	FROM business
	WHERE city = 'Scottsdale'
```
```sql	
	-- Soul Food
	SELECT SUM(review_count) AS Number_of_Reviews
	FROM category c INNER JOIN business b ON c.business_id = b.id
	WHERE category = 'Soul Food';
```
There were 20,614 reviews in the city of Scottsdale, and there were only 10 total reviews for the 'Soul Food' business category. The difference in review count between both constraint was vast.
         
## 3. Can anything be inferred from the location data provided between businesses in Scottsdale, AZ and 'Soul Food' businesses?
There is nothing to infer based off of location since both of the two 'Soul Food' businesses are located in seperate cities (North Randall, and Phoenix). To discover this I began by identifying the specific latitude and longitude of the two 'Soul Food' businesses since there were only two factors to query against for Scottsdale.
```sql
		SELECT DISTINCT b.name,
			b.latitude,
			b.longitude
		FROM business b 
			INNER JOIN category c ON c.business_id = b.id 
		WHERE c.category = 'Soul Food';
```
```
		+-------------------------------+----------+-----------+
		| name                          | latitude | longitude |
		+-------------------------------+----------+-----------+
		| Oinky's Pork Chop Heaven      |  41.4352 |  -81.5214 |
		| Charlie D's Catfish & Chicken |  33.4468 |  -112.057 |
		+-------------------------------+----------+-----------+
```
With the coordinates identified for the smaller list of businesses, now it was time to attempt to match the two sets of variables against the 994 potential matches in latitude or longitude with the businesses in Scottsdale.

```sql
	-- Querying against the known Soul Food lat. and long.
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
```
Attempting to match the constraints of the two 'Soul Food' businesses to the larger sample of Scottsdale businesses produced no crossover of latitude or longitude. The query above returned **no** results, meaning that no business in Scottsdale shared any latitudinal or longitudinal variables with the two 'Soul Food' businesses in the data set.

## 4. What differences are there between open and closed businesses?

#### Difference #1:
The disparity in number of reviews as it relates to closed vs. open businesses is quite vast. Here are two reasons that I would infer:
1. There are more open businesses than closed businesses
2. Open businesses generate new review counts and closed businesses counts are frozen

```sql
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
```
```
		+-------------+--------------+
		| Open/Closed | Review_Count |
		+-------------+--------------+
		| Open        |      269,300 |
		| Closed      |       35,261 |
		+-------------+--------------+
```

There is no limit to the ammount of reviews that any one business could potentially generate compared to another, but when a business is closed that progress is frozen in time creating a widening disparity between open and closed.

A great measure for further analysis would be the volume of reviews written during final three to four months of a business before it closes. Are there more reviews written (likely due to a potential flaw or vulnerabilty of their business), or would reviews plumet as a result of wanning interest by patrons?

Fortunatly the next difference will give some insight as it relates to this dataset!
         
#### Difference #2:
*NOTE: The following numbers are derived from a count of businesses that have a finalized star rating independant of the review count. This means that the total count of these queries should amount back to the total amount of businesses in the database (10,000).*


Unsuprisingly, open businesses had more 'top' ratings than closed businesses did, but what did suprise was the low volume of 'low' rating businesses that were closed. The largest grouping of ratings for closed businesses came from the 'average' category. Most likely this happened as a result of 'average' containing a wider spread of ratings (spanning two ratings!) skewing it slightly, but a mere 38 'low' rated businesses really displays just how far off my assumptions about review score were as they relates to businesses success. Based soley off of these numbers it looks like there is no discernable correlation between being rated 'low' and your business closing as it pertains to this dataset alone.

	Low Rated Open: 324
	Average Rated Open: 3,801
	Top Rated Open: 4,355
	-----
	Low Rated Closed: 38
	Average Rated Closed: 829
	Top Rated Closed: 653

*Querying for each of these findings was as simple as constraining and aliasing with the proper label*
```sql
	SELECT COUNT(name) AS 'Top Rated & Open Businesses'
	FROM business
	WHERE is_open = 1
		AND stars >= 4
```
```
		+-----------------------------+
		| Top Rated & Open Businesses |
		+-----------------------------+
		|                        4355 |
		+-----------------------------+
```      

## 5. What are the commonalities and anomalies between businesses clustered into 'Star Categories'?	
The Star Categories are as follows:s
```
5.0 stars = 'Premium Rated'
4.0 - 4.9 stars = 'Top Rated'
3.0 - 3.9 stars = 'High Rating'
2.0 - 2.9 stars = 'Average Rating'
1.0 - 1.9 stars = 'Low Rating'
```
**NOTE REGARDING 'StarCategories': Values are derived from a count of businesses that have a finalized star rating independant of the review count. This means that the total count of these queries should amount back to the total amount of businesses in the database (10,000).**
		    
         
### Decription of the data needed and used for this analysis
For the purposes of this analysis I focused only the 'premium rated' businesses against the number of reviews that they contained. Comparisons were ran based on city, state, and open/closed status while querying for outliers and summary data to get a good grasp on the larger points of this dataset. Sampleing the top ten of different variations of the dataset proved effective in examining the larger differences between the greatest volumes of star category and review count. 

None of the businesses within the top 10 most reviewed on Yelp had anything above a 4.5. The same group didnt dip past 2.5 as the lowest rating. I would infer that this range of rating could be the result of the prime motivator behind a consumer reviewing a business on Yelp. The main reasons why a consumer would take time to review a business on Yelp is because they had an experience that they wanted to share with other people. While average ratings for businesses might display more volume in the 'average' and 'low' categories, an area for further analysis would be if individual reviews tend more towards wide swings in star category compared to the average rating of the business.

```sql
-- Top 10 businesses with the most reviews
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
```    
```
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
```
Regardless, among 'premium' businesses review count held a strong correlation with large volume of 'premium' counts across city and state metrics. The city with the most 'premium' businesses (Las Vegas) also held a strong lead ahead of all other cities in review count (diffrences of 48,351 for review count and 560 'premium' businesses). Of course Las Vegas as notable worldwide center for adult entertainment might influence its large lead ahead of other cities and states. The top ten cities displayed slight variability in position of the top ten most premium businesses compared to the ten most reviewed cities, but largely the same cities remained on the list with the exception of Chandler, AZ and Mesa, AZ.


```sql
-- Top ten states with the most premium businesses
SELECT state,
    COUNT(stars = 5) AS Premium_Business_Count,
    SUM(review_count) AS TotalNumberOfReviews
FROM  business
GROUP BY state
ORDER BY Premium_Business_Count DESC
LIMIT 10;
```
```
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
```

Similar to what the top ten cities with the highest review count and 'premium' business count revealed, the top ten groupings by state revealed little variability. The three states at the top of the list held the same position across both aggregations. In fact the top nine states were all the same states across review count and 'premium' business count with the only position change occuring at the fourth and fifth spots across lists alternating between North Carolina and Ohio. The tenth spot changed dramatically across lists. This most likely occured due to the same trend affecting both tables: 'premium' businesses do not numerically have a larger decrease in count moving down the list, but their decrease wildly displaces the amount of reviews. As a result of this the number of reviews becomes much less stable as 'premium' volume decreases. This is because the 'TotalNumberOfReviews' column is unaffected by number of stars meaning that the less that the 'premium' can impact the number of reviews, the less likely there will be a correlation between the two constraints.

Average ratings across businesses in states produced an entirely set of results that portrayed
```sql
SELECT name,
		name 
		,review_count
		SUBSTR(AVG(stars),1,4) AS AverageStarRating
		,CASE
			WHEN stars = 5 THEN 'Premium Rated'
			WHEN stars >= 4 AND stars < 5 THEN 'Top Rated'
			WHEN stars >= 3 AND stars < 4 THEN 'High Rating'
			WHEN stars >= 2 AND stars < 3 THEN 'Average Rating'
			WHEN stars >= 1 AND stars < 2 THEN 'Low Rating'
			ELSE 'This is weird.. check on this one'
		END AS 'StarCategory'
		state
	FROM business
	GROUP BY state
	ORDER BY AverageStarRating DESC
	LIMIT 10;
```
```
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
	+-------+-------------------+
```
```sql
	SELECT name,
			review_count,
			SUBSTR(AVG(stars),1,4) AS AverageStarRating,
			state,
			CASE
				WHEN stars = 5 THEN 'Premium Rated'
				WHEN stars >= 4 AND stars < 5 THEN 'Top Rated'
				WHEN stars >= 3 AND stars < 4 THEN 'High Rating'
				WHEN stars >= 2 AND stars < 3 THEN 'Average Rating'
				WHEN stars >= 1 AND stars < 2 THEN 'Low Rating'
				ELSE 'This is weird.. check on this one'
			END AS 'StarCategory'
		FROM business
		GROUP BY name
		ORDER BY review_count DESC
		LIMIT 10;
```
```
	+----------------------+--------------+-------------------+-------+----------------+
	| name                 | review_count | AverageStarRating | state | StarCategory   |
	+----------------------+--------------+-------------------+-------+----------------+
	| The Buffet           |         3873 | 3.5               | NV    | High Rating    |
	| Schwartz's           |         1757 | 4.0               | QC    | Top Rated      |
	| Joe's Farm Grill     |         1549 | 4.0               | AZ    | Top Rated      |
	| Carson Kitchen       |         1410 | 4.5               | NV    | Top Rated      |
	| Delmonico Steakhouse |         1389 | 4.0               | NV    | Top Rated      |
	| Le Thai              |         1252 | 4.0               | NV    | Top Rated      |
	| Scarpetta            |         1116 | 4.0               | NV    | Top Rated      |
	| Diablo's Cantina     |         1084 | 3.0               | NV    | High Rating    |
	| MGM Grand Buffet     |          961 | 2.5               | NV    | Average Rating |
	| Joyride Taco House   |          902 | 4.0               | AZ    | Top Rated      |
	+----------------------+--------------+-------------------+-------+----------------+
```
The highest average star rating highlighted a really important relationship between the sample size of review count as it relates to the average star rating. From my own life experience I have found it to be a common ancedote for reviewing the ratings of a product or service by checking the number of reviews associated with the rating. Common business practice would be to have friends and family rate a business highly regardless of their actual opinion of the business.
```sql
	SELECT name,
			review_count,
			SUBSTR(AVG(stars),1,4) AS AverageStarRating,
			state,
			CASE
				WHEN stars = 5 THEN 'Premium Rated'
				WHEN stars >= 4 AND stars < 5 THEN 'Top Rated'
				WHEN stars >= 3 AND stars < 4 THEN 'High Rating'
				WHEN stars >= 2 AND stars < 3 THEN 'Average Rating'
				WHEN stars >= 1 AND stars < 2 THEN 'Low Rating'
				ELSE 'This is weird.. check on this one'
			END AS 'StarCategory'
		FROM business
		--WHERE review_count > 1000
		GROUP BY name
		ORDER BY AverageStarRating DESC
		LIMIT 20;
```
```
	+----------------------------------------+--------------+-------------------+-------+---------------+
	| name                                   | review_count | AverageStarRating | state | StarCategory  |
	+----------------------------------------+--------------+-------------------+-------+---------------+
	| 10 Factory Fitness Center              |            4 | 5.0               | AZ    | Premium Rated |
	| 12th House Interiors                   |            4 | 5.0               | OH    | Premium Rated |
	| 24/7 Carpet & Floor Care               |           47 | 5.0               | NV    | Premium Rated |
	| 24/7 Vegas VIP                         |            7 | 5.0               | NV    | Premium Rated |
	| 305 Kustoms                            |           28 | 5.0               | NV    | Premium Rated |
	| 3D MicroworksLV                        |            4 | 5.0               | NV    | Premium Rated |
	| 3rd Thursday Adventure Run Tempe       |            9 | 5.0               | AZ    | Premium Rated |
	| 4E Kennels                             |           24 | 5.0               | NV    | Premium Rated |
	| 50/50 Realty                           |            4 | 5.0               | AZ    | Premium Rated |
	| 808 Car Audio Evolution                |            4 | 5.0               | NV    | Premium Rated |
	| A Brighter Avenue                      |            5 | 5.0               | AZ    | Premium Rated |
	| A Caring Dental Group                  |            3 | 5.0               | OH    | Premium Rated |
	| A Desert Custom Cycles                 |            4 | 5.0               | AZ    | Premium Rated |
	| A Kleener Image                        |            5 | 5.0               | OH    | Premium Rated |
	| A Little Off                           |            6 | 5.0               | NV    | Premium Rated |
	| A Magical Day, Princess Parties & More |            4 | 5.0               | NV    | Premium Rated |
	| A Safe Pool of Arizona                 |           15 | 5.0               | AZ    | Premium Rated |
	| A Signature Gifts Antiques & Furniture |            3 | 5.0               | OH    | Premium Rated |
	| A Way Home Moving                      |           21 | 5.0               | WI    | Premium Rated |
	| A&B Scissor Hands                      |           12 | 5.0               | IL    | Premium Rated |
	+----------------------------------------+--------------+-------------------+-------+---------------+
```
In fact, of the 10,000 businesses in this data set, **only 8 of them had reviews over 1,000**
```sql
SELECT COUNT(*) AS Number_Of_Reviews_Over_1000
	FROM business
	WHERE review_count >= 1000 
```
```
+-----------------------------+
| Number_Of_Reviews_Over_1000 |
+-----------------------------+
|                           8 |
+-----------------------------+
```

## Extra Queries 
```sql
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
```

Closed premium rated businesses  
```sql
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
```
```
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
```

Open premium businesses (same as above but changing the WHERE is_open from '0' to '1')
```
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
```

Top ten states with the most premium businesses
```sql
SELECT state,
    COUNT(stars = 5) AS Premium_Business_Count,
    SUM(review_count) AS TotalNumberOfReviews
FROM  business
GROUP BY state
ORDER BY Premium_Business_Count DESC
LIMIT 10;
```
```
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
```

Top ten states with the most reviews
```sql
SELECT state,
    COUNT(stars = 5) AS Premium_Business_Count,
    SUM(review_count) AS TotalNumberOfReviews
FROM  business
GROUP BY state
ORDER BY TotalNumberOfReviews DESC
LIMIT 10;
```
```
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
```

Top ten cities with the most premium businesses
```sql
SELECT state,
    city,
    COUNT(stars = 5) AS Premium_Business_Count,
    SUM(review_count) AS TotalNumberOfReviews
FROM  business
GROUP BY city
ORDER BY Premium_Business_Count DESC
LIMIT 10;
```
```
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
```
Top ten cities with the most reviews
	```sql
    SELECT state,
		city,
		COUNT(stars = 5) AS Premium_Business_Count,
		SUM(review_count) AS TotalNumberOfReviews
	FROM  business
	GROUP BY city
	ORDER BY TotalNumberOfReviews DESC
	LIMIT 10;
	```
```
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
```