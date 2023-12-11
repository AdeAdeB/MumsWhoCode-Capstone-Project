Question 1:What is the average nightly price for accomodations in Germany? How does the pricing vary based on the neighborhood or property type?

SQL Queries:

a) SELECT host_location, AVG(CAST(REGEXP_REPLACE(price, '[^\d.]', '', 'g') AS NUMERIC)) AS average_price

FROM listings

WHERE host_location = 'Germany'

GROUP BY host_location


b) SELECT neighbourhood, property_type, AVG(CAST(REGEXP_REPLACE(price, '[^\d.]', '', 'g') AS NUMERIC)) AS average_price

FROM listings

GROUP BY neighbourhood, property_type

ORDER BY neighbourhood, property_type


Answer: The average nightly price for accomodations in Germany is 8080.3

Question 2: Which neighborhood has the highest demand? What property type is the most commonly booked?

SQL Queries:

a) SELECT neighbourhood, COUNT (*) AS total_bookings

FROM listings

GROUP BY neighbourhood

ORDER BY total_bookings DESC

Answer: a) WARD 115 with total bookings of '4459' has the highest demand

b) Entire rental unit with the total bookings of '7932' is the most commonly booked

Question 3: What is the average review score for properties in Hong Kong?

SQL Queries:

SELECT host_location, AVG(review_score_rating) AS average_review_score


FROM listings


WHERE host_location = 'Hong Kong'


GROUP BY host_location


ORDER BY average_review_score DESC

Answer: The average review score for properties in Hong Kong is 3.54

Question 4: Who are the top hosts or properties in terms of bookings or reviews? 

SQL Queries:
--Top hosts based on bookings

SELECT host_id, host_name, COUNT (*) AS total_bookings

FROM listings

GROUP BY host_id, host_name

ORDER BY total_bookings DESC

LIMIT 5

--Top hosts based on reviews

SELECT host_id, host_name, AVG(review_score_rating) AS average_review_score

FROM listings

GROUP BY host_id, host_name

ORDER BY average_review_score DESC

LIMIT 5

Answer: a) Manageair happens to be the topmost host in term of bookings with total bookings of '155'

b) Tushar is the top host in term of reviews
