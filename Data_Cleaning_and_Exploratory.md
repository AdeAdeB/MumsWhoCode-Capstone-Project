
--Having opened the data and trying to get familiar with it, it was observed that the data is very dirty


--Data Quality is an issue as there are lots of null and Not Applicable values


--These values were replaced with default values because dropping them will cause a great loss to the data


--For the integer data types, the default value was 'Zero'


--For the string data types, the default value was 'Unknown'


--For the date type, the default value was the current date


--Some redundant columns were noticed and dropped as they are irrelevant

--The syntax of the query that was used for replacing these values is:


UPDATE listings


SET review_scores_value = 0
WHERE review_scores_value IS NULL


--The syntax for dropping redundant columns is;


ALTER TABLE listings
DROP COLUMN amenities


 #The pattern and overview of the dataset

 --The dataset is structured in a way that defines the names of the different AirBnb in Capetown, their hosts' identity, the dates when they started being hosts, their locations, the number of rooms present, the review ratings, how swift the hosts are in responding, the properties type, the hosts' total listings, the prices of the rooms, the minimum and maximun nights that can be spent, the availability of the rooms in 30, 90, and 365 days respectively.

 
Queries:

```sql
--Query for creating the listings table in the CapetownAirBnB Database
DROP TABLE IF EXISTS listings;
CREATE TABLE listings (
			id bigint,
			name varchar,
			host_id int,
			host_name Varchar,
			host_since date,
			host_location Varchar,
			host_response_time varchar,
			host_response_rate varchar,
			host_acceptance_rate varchar,
			host_is_superhost bool,
			host_listings_count smallint,
			host_total_listings_count smallint,
			host_verifications Varchar,
			host_has_profile_pic bool,
			neighbourhood Varchar,
			property_type Varchar,
			room_type Varchar,
			accomodates smallint,
			bathrooms_text Varchar,
			bedrooms smallint,
			beds smallint,
			amenities Varchar,
			price Varchar,
			minimum_nights int,
			maximum_nights int,
			has_availability bool,
			availability_30 varchar,
			availability_60 varchar,
			availability_90 varchar,
			availability_365 varchar,
			number_of_reviews int,
			first_review date,
			last_review date,
			review_score_rating numeric,
			review_scores_accuracy numeric,
			review_scores_cleanliness numeric,
			review_scores_checkin numeric,
			review_scores_communication numeric,
			review_scores_location numeric,
			review_scores_value numeric,
			instant_bookable bool			
);
--Having created this table, right_click on your table, click on import, fill in the fields and that should succesfully import the CSV file into the listins table
