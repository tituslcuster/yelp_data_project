# Yelp Data Project

This SQL project was a final assignment for the Couersera course "[SQL for Data Science](https://www.coursera.org/account/accomplishments/verify/K93FFFBPQ9AC)". 

The "Yelp" project is made up of two major parts:
+ Part 1 -- [Dataset Profiling and Understanding](https://github.com/tituslcuster/yelp_data_project/blob/main/YelpDataProjectpt1.md)
+ Part 2 -- [Inferences and Analysis](https://github.com/tituslcuster/yelp_data_project/blob/main/YelpDataProjectpt2.md)

This document covers part 2 of the assignment and contains some questions for deeper explorartory data analysis that interested me. Unlike part one, the scope of this document is much more focused and practical. 

The structure of the initial questions through out all of part 1 and aspects of part 2 originate from the course, but for the purposes of compiling my work I have resummarized and edited the questions for clear and consise readability. For a full look at the questions from the original assignment and my responses, check out [YelpDataProject_TitusCuster.sql](https://github.com/tituslcuster/SQL/blob/main/YelpDataProject_TitusCuster.sql).

## Business questions I will be attempting to answer with this dataset:
1. Is there a difference in distribution of hours for businesses in Scottsdale, AZ compared to 'Soul Food' businesses?
2. Is there a difference in number of reviews for businesses in Scottsdale, AZ compared to 'Soul Food' businesses?
3. Can anything be inferred from the location data provided between businesses in Scottsdale, AZ and 'Soul Food' businesses?
4. What differences are there between open and closed businesses?
5. What are the commonalities and anomalies between businesses clustered into 'Star Categories'?

## The Dataset
![The ER Diagram included with this assignment](https://i.imgur.com/2y8nPne.png)
> The ER Diagram included with this assignment

### Here is a short desciption of what each table contains:
+ **business** -- the primary focus of my analysis! the business table contains all of the relvant data except for information regarding hours of operation for businesses.
+ **hours** -- table containing hours of operations for all businsses. hours include the hours dictated by a day of the week seperated by a '|' in each row. Each hour is paired with a cooresponding business_id making joining simple.
+ **category** -- table containing the category of business to give Yelp users a way of search or filtering for business by category. Categories might include entries such as Shopping, Restaurants, Sandwhiches, or Jewelry Repair.

-------
*(tables marked with n/a do not apply to the scope of this analysis)*

+ **attribute** -- (n/a) descriptors for businesses on yelp such as "good for kids" or "Noise Level" coupled with a attribute specific ranking value indicating the business_id's ranking across a variety of attributes.
+ **review** -- (n/a) the long-form keying of the main text of a review. This table also includes timestamps for submission, star ranking, and id's for joins with the 'business' and 'user' tables.
+ **checkin** -- (n/a) hourly timestamps displaying a count of the number of checkins associated with each business. The 'date' field includes a day of the week and hour (in 24hr format) seperated by a '-' in each row . These checkins do NOT indicate user specific checkins but only coorespond to business checkins.
+ **tip** -- (n/a) long-form text describing a 'tip' submitted by users for use as insight for future patrons of the business being reviewed. This table includes id's for joins with the 'business' and 'user' tables and a traditional timestamp format column labled as 'date'.
+ **user** -- (n/a) a full rundown on user profiles including the day the user opened their Yelp reviewer account, number of reviews they have made, and an average rating for their stars. This table also includes counts of times they have interacted with other users or have been interacted with on the platform across various metrics.
+ **friend** -- n/a a table used only for linking that attaches user_id to a unrecognizable friend_id. From what i can find, this is the only instance of friend_id in the database, which leaves me wondering what function this table can serve.
+ **elite_years** -- n/a this table displays a list of years as they relate to any given user_id cooresponding to "elite" status. Here's more about Yelp elite from [Business Insider](https://www.businessinsider.com/guides/tech/how-to-become-yelp-elite?op=1): "Yelp Elite Squad is a selective, annually chosen membership within the Yelp community."




<!--
The following is a test and is currently commented out:

column name|column name|
-----------|-----------|
element    |      value|
element    |      value|
element    |      value|
-->

Throughout the course of this analysis I found it neccessary to create my own categorization of star ratings under a five tiered column entitled 'StarCategories'
<!--
```
5.0 stars = 'Premium Rated'
4.0 - 4.9 stars = 'Top Rated'
3.0 - 3.9 stars = 'High Rating'
2.0 - 2.9 stars = 'Average Rating'
1.0 - 1.9 stars = 'Low Rating'
```
-->
**Star Spread** |**Category Name**    |
----------------|---------------------|
5.0 stars       |      'Premium Rated'|
4.0 - 4.9 stars |          'Top Rated'|
3.0 - 3.9 stars |        'High Rating'|
2.0 - 2.9 stars |     'Average Rating'|
1.0 - 1.9 stars |         'Low Rating'|


I implemented this categorization system in SQL across multiple queries, but I began by applying it to the entire spread of 'business' tables entries.
```sql
SELECT city,
	name,
	stars,
	CASE
        WHEN stars = 5 THEN 'Premium Rated'
        WHEN stars >= 4 AND stars < 5 THEN 'Top Rated'
        WHEN stars >= 3 AND stars < 4 THEN 'High Rating'
        WHEN stars >= 2 AND stars < 3 THEN 'Average Rating'
        WHEN stars >= 1 AND stars < 2 THEN 'Low Rating'
        ELSE 'This is weird.. check on this one'
    END AS 'StarCategory'
FROM business
ORDER BY stars DESC;
```

This was a great starting place for the rest of the project since I would often be returning to this categorization method throughout the entirity of the project!

**NOTE REGARDING 'StarCategories': Values are derived from a count of businesses that have a finalized star rating independant of the review count. This means that the total count of these queries should amount back to the total amount of businesses in the database (10,000).**
