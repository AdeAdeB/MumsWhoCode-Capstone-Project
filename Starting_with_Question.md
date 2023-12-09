Answer the following questions and provide the SQL queries used to find the answer.

Question 1: How many total listings are there in the Capetown AirBnB data? Would you say there are inactive listings in the data? If yes, What percentage of total listing is active or inactive?

SQL Queries:1) To determine the total listings 
SELECT COUNT(*) AS total_listings
FROM listings
2) Percentage of active and inactive listings using the host_response_time column
SELECT
CASE WHEN host_response_time = 'within an hour' THEN 'Active' ELSE 'Inactive' END AS status,
COUNT(*) AS listing_count,
(COUNT(*) * 100.0 / (SELECT COUNT(*) FROM listings)) AS percentage
FROM listings
GROUP BY status;
Answer: 1) 21120 listings 
        2) Yes, there are inactive listings in the data
        3) 53.5% is active while 46.5% is inactive

Question 2: How many distinct hosts are present in the CapeTown AirBnB listing? How many listings does each of this distinct hosts have? Which host have the most listings

SQL Queries: 1) Getting the total number of distinct hosts in the CapeTown AirBnB listing
SELECT COUNT(DISTINCT host_id) AS distinct_hosts_coumt
FROM listings
SELECT  host_id, MAX(host_total_listings_count) AS max_listings
FROM listings
GROUP BY host_id, host_total_listings_count
ORDER BY host_total_listings_count DESC
2) To determine the total number of listings each distinct host has
SELECT
host_id,
COUNT(*) AS listing_count
FROM listings
GROUP BY host_id;
3) Getting the host with the most listings
SELECT
host_id, host_name,
COUNT(*) AS listing_count
FROM listings
GROUP BY host_id, host_name
ORDER BY listing_count DESC;

Answer: 1) 11303
3) Manageair has the most listings with listing count of '155'

Question 3: On Availability, What is the total number of listings available for more than 90 days a year? How many listings have availability for less than 30 days a year? What percentage of listings are available for instant booking?

SQL Queries: 1) To get the total number of listings available for more than 90 days a year
SELECT COUNT (*) AS total_listings_more_than_90_days
FROM listings
WHERE CAST (availability_365 AS INTEGER) > 90
2) To determine the number of listings available for less than 30 days a year
SELECT COUNT (*) AS total_listings_less_than_30_days
FROM listings
WHERE CAST (availability_365 AS INTEGER) <30 
3) Determining the percentage of listings available for instant booking
SELECT
(COUNT(CASE WHEN instant_bookable = 't' THEN 1 END) * 100) / COUNT(*) AS instant_booking_percentage
FROM listings

Answer: 1) 14998
        2) 3842
        3) 27%

Question 4: What are the different types of properties available and their counts? Which property type has the highest average price? Write a query to show the correlation between review and pricing.

SQL Queries: 1) Getting the different types of properties available
SELECT property_type,
COUNT (*) AS property_count
FROM listings
GROUP BY property_type
ORDER BY property_count DESC
2) To get the property with the highest average price
SELECT 
property_type,
AVG(CAST(REGEXP_REPLACE(listings.price, '[^\d.]', '', 'g') AS NUMERIC)) AS average_price
FROM listings
GROUP BY property_type
ORDER BY average_price DESC
LIMIT 1;
3) Correlation between review and pricing
SELECT 
CORR(number_of_reviews, CAST(REGEXP_REPLACE(listings.price, '[^\d.]', '', 'g') AS NUMERIC)) AS correlation
FROM listings
--The price is in string or character varying type instead of numeric, hence, the need for the CAST function
Answer: 1)there are 81 property types
        2) Entire villa has the highest average price with 13742.4 as the average price
        3) From the result of the query, it shows there is no correlation between review and pricing (Negative correlation)

Question 5: How do individual listings or hosts perform compared to the average or top-performing ones in terms of ratings, pricing, and booking frequency?

SQL Queries: 1) In term of ratings
SELECT
host_id,
AVG(review_score_rating) AS average_rating,
CASE
WHEN AVG(review_score_rating) > (SELECT AVG(review_score_rating) FROM listings) THEN 'Above Average'
WHEN AVG(review_score_rating) = (SELECT AVG(review_score_rating) FROM listings) THEN 'Average'
ELSE 'Below Average'
END AS rating_comparison
FROM listings
GROUP BY host_id
2) In term of pricings
SELECT
host_id,
AVG (CAST(REGEXP_REPLACE(listings.price, '[^\d.]', '', 'g') AS NUMERIC)) AS average_price,
CASE
WHEN AVG (CAST(REGEXP_REPLACE(listings.price, '[^\d.]', '', 'g') AS NUMERIC)) > (SELECT AVG(CAST(REGEXP_REPLACE(listings.price, '[^\d.]', '', 'g') AS NUMERIC)) FROM listings) THEN 'Above Average'
WHEN AVG (CAST(REGEXP_REPLACE(listings.price, '[^\d.]', '', 'g') AS NUMERIC)) = (SELECT AVG(CAST(REGEXP_REPLACE(listings.price, '[^\d.]', '', 'g') AS NUMERIC)) FROM listings) THEN 'Average'
ELSE 'Below Average'
END AS price_comparison
FROM listings
GROUP BY host_id;
3) In term of booking frequency
SELECT host_id,
COUNT(*) AS booking_count,
AVG(CAST(REGEXP_REPLACE(listings.price, '[^\d.]', '', 'g') AS NUMERIC)) AS average_price,
AVG(review_score_rating) AS average_review
FROM listings
GROUP BY host_id
ORDER BY booking_count DESC;
Answer: 1) A lot of hosts performed above average
        2) In term of pricing and using host ids, most of the prices are below average
        3) host id '487012580' has the highest booking frequency of '155' bookings
        
