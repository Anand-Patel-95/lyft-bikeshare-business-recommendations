# Project 1: Query Project

- my new change
- In the Query Project, you will get practice with SQL while learning about
  Google Cloud Platform (GCP) and BiqQuery. You'll answer business-driven
  questions using public datasets housed in GCP. To give you experience with
  different ways to use those datasets, you will use the web UI (BiqQuery) and
  the command-line tools, and work with them in Jupyter Notebooks.

#### Problem Statement

- You're a data scientist at Lyft Bay Wheels (https://www.lyft.com/bikes/bay-wheels), formerly known as Ford GoBike, the
  company running Bay Area Bikeshare. You are trying to increase ridership, and
  you want to offer deals through the mobile app to do so.

- What deals do you offer though? Currently, your company has several options which can change over time.  Please visit the website to see the current offers and other marketing information. Frequent offers include:
  * Single Ride
  * Monthly Membership
  * Annual Membership
  * Bike Share for All
  * Access Pass
  * Corporate Membership
  * etc.

- Through this project, you will answer these questions:

  * What are the 5 most popular trips that you would call "commuter trips"?

  * What are your recommendations for offers (justify based on your findings)?

- Please note that there are no exact answers to the above questions, just like in the proverbial real world.  This is not a simple exercise where each question above will have a simple SQL query. It is an exercise in analytics over inexact and dirty data.

- You won't find a column in a table labeled "commuter trip".  You will find you need to do quite a bit of data exploration using SQL queries to determine your own definition of a communter trip.  In data exploration process, you will find a lot of dirty data, that you will need to either clean or filter out. You will then write SQL queries to find the communter trips.

- Likewise to make your recommendations, you will need to do data exploration, cleaning or filtering dirty data, etc. to come up with the final queries that will give you the supporting data for your recommendations. You can make any recommendations regarding the offers, including, but not limited to:
  * market offers differently to generate more revenue
  * remove offers that are not working
  * modify exising offers to generate more revenue
  * create new offers for hidden business opportunities you have found
  * etc.

#### All Work MUST be done in the Google Cloud Platform (GCP) / The Majority of Work MUST be done using BigQuery SQL / Usage of Temporary Tables, Views, Pandas, Data Visualizations

A couple of the goals of w205 are for students to learn how to work in a cloud environment (such as GCP) and how to use SQL against a big data data platform (such as Google BigQuery).  In keeping with these goals, please do all of your work in GCP, and the majority of your analytics work using BigQuery SQL queries.

You can make intermediate temporary tables or views in your own dataset in BigQuery as you like.  Actually, this is a great way to work!  These make data exploration much easier.  It's much easier when you have made temporary tables or views with only clean data, filtered rows, filtered columns, new columns, summary data, etc.  If you use intermediate temporary tables or views, you should include the SQL used to create these, along with a brief note mentioning that you used the temporary table or view.

In the final Jupyter Notebook, the results of your BigQuery SQL will be read into Pandas, where you will use the skills you learned in the Python class to print formatted Pandas tables, simple data visualizations using Seaborn / Matplotlib, etc.  You can use Pandas for simple transformations, but please remember the bulk of work should be done using Google BigQuery SQL.

#### GitHub Procedures

In your Python class you used GitHub, with a single repo for all assignments, where you committed without doing a pull request.  In this class, we will try to mimic the real world more closely, so our procedures will be enhanced.

Each project, including this one, will have it's own repo.

Important:  In w205, please never merge your assignment branch to the master branch.

Using the git command line: clone down the repo, leave the master branch untouched, create an assignment branch, and move to that branch:
- Open a linux command line to your virtual machine and be sure you are logged in as jupyter.
- Create a ~/w205 directory if it does not already exist `mkdir ~/w205`
- Change directory into the ~/w205 directory `cd ~/w205`
- Clone down your repo `git clone <https url for your repo>`
- Change directory into the repo `cd <repo name>`
- Create an assignment branch `git branch assignment`
- Checkout the assignment branch `git checkout assignment`

The previous steps only need to be done once.  Once you your clone is on the assignment branch it will remain on that branch unless you checkout another branch.

The project workflow follows this pattern, which may be repeated as many times as needed.  In fact it's best to do this frequently as it saves your work into GitHub in case your virtual machine becomes corrupt:
- Make changes to existing files as needed.
- Add new files as needed
- Stage modified files `git add <filename>`
- Commit staged files `git commit -m "<meaningful comment about your changes>"`
- Push the commit on your assignment branch from your clone to GitHub `git push origin assignment`

Once you are done, go to the GitHub web interface and create a pull request comparing the assignment branch to the master branch.  Add your instructor, and only your instructor, as the reviewer.  The date and time stamp of the pull request is considered the submission time for late penalties.

If you decide to make more changes after you have created a pull request, you can simply close the pull request (without merge!), make more changes, stage, commit, push, and create a final pull request when you are done.  Note that the last data and time stamp of the last pull request will be considered the submission time for late penalties.

Make sure you receive the emails related to your repository! Your project feedback will be given as comment on the pull request. When you receive the feedback, you can address problems or simply comment that you have read the feedback.
AFTER receiving and answering the feedback, merge you PR to master. Your project only counts as complete once this is done.

---

## Parts 1, 2, 3

We have broken down this project into 3 parts, about 1 week's work each to help you stay on track.

**You will only turn in the project once at the end of part 3!**

- In Part 1, we will query using the Google BigQuery GUI interface in the cloud.

- In Part 2, we will query using the Linux command line from our virtual machine in the cloud.

- In Part 3, we will query from a Jupyter Notebook in our virtual machine in the cloud, save the results into Pandas, and present a report enhanced by Pandas output tables and simple data visualizations using Seaborn / Matplotlib.

---

## Part 1 - Querying Data with BigQuery

### SQL Tutorial

Please go through this SQL tutorial to help you learn the basics of SQL to help you complete this project.

SQL tutorial: https://www.w3schools.com/sql/default.asp

### Google Cloud Helpful Links

Read: https://cloud.google.com/docs/overview/

BigQuery: https://cloud.google.com/bigquery/

Public Datasets: Bring up your Google BigQuery console, open the menu for the public datasets, and navigate to the the dataset san_francisco.

- The Bay Bike Share has two datasets: a static one and a dynamic one.  The static one covers an historic period of about 3 years.  The dynamic one updates every 10 minutes or so.  THE STATIC ONE IS THE ONE WE WILL USE IN CLASS AND IN THE PROJECT. The reason is that is much easier to learn SQL against a static target instead of a moving target.

- (USE THESE TABLES!) The static tables we will be using in this class are in the dataset **san_francisco** :

  * bikeshare_stations

  * bikeshare_status

  * bikeshare_trips

- The dynamic tables are found in the dataset **san_francisco_bikeshare**

### Some initial queries

Paste your SQL query and answer the question in a sentence.  Be sure you properly format your queries and results using markdown.

- What's the size of this dataset? (i.e., how many trips)
  - Answer: This dataset contains 983,648 trips.
  ```sql
  SELECT count(distinct trip_id) as total_trips
  FROM `bigquery-public-data.san_francisco.bikeshare_trips`
  ```

    Row | total_trips
    --- | ---
    1 | 983648


- What is the earliest start date and time and latest end date and time for a trip?
  - Answer: The earliest start date and time is 8/29/2013 at 9:08 PST. The latest end date and time is 08/31/2016 at 23:48 PST. These times are in PST according to the schema and also confirmed to be through some initial queries that confirmed that AM times were popular during weekday trips.
  ```sql
  SELECT min(start_date) as earliest_start_date, max(end_date) as latest_end_date
  FROM `bigquery-public-data.san_francisco.bikeshare_trips`
  ```

    Row | earliest_start_date | latest_end_date
    --- | --- | ---
    1 | 2013-08-29 09:08:00 UTC | 2016-08-31 23:48:00 UTC



- How many bikes are there?
  - Answer: This dataset contains 700 distinct bikes.
  ```sql
  SELECT count(distinct bike_number) as total_unique_bikes
  FROM `bigquery-public-data.san_francisco.bikeshare_trips`
  ```

    Row | total_unique_bikes
    --- | ---
    1 | 700

### Questions of your own
- Make up 3 questions and answer them using the Bay Area Bike Share Trips Data.  These questions MUST be different than any of the questions and queries you ran above.

- Question 1: What are the top 5 most frequent distinct trips, defined by start station and end station?
  * Answer: Here we have the top 5 most frequent trips made, defined by how many distinct trips were made from the specified starting station to the specified ending station. 1)  Harry Bridges Plaza (Ferry Building) to Embarcadero at Sansome, 2) San Francisco Caltrain 2 (330 Townsend) to Townsend at 7th, 3)	2nd at Townsend to Harry Bridges Plaza (Ferry Building), 4) Harry Bridges Plaza (Ferry Building) to 2nd at Townsend, 5) Embarcadero at Sansome to Steuart at Market.
  * SQL query:
  ```sql
  SELECT start_station_name, end_station_name, count(distinct trip_id) as num_trips
  FROM `bigquery-public-data.san_francisco.bikeshare_trips`
  group by start_station_name, end_station_name order by (num_trips) DESC
  ```

    Row | start_station_name | end_station_name | num_trips
    --- | --- | --- | ---
    1 | Harry Bridges Plaza (Ferry Building) | Embarcadero at Sansome | 9150
    2 | San Francisco Caltrain 2 (330 Townsend) | Townsend at 7th | 8508
    3 | 2nd at Townsend | Harry Bridges Plaza (Ferry Building) | 7620
    4 | Harry Bridges Plaza (Ferry Building) | 2nd at Townsend | 6888
    5 | Embarcadero at Sansome | Steuart at Market | 6874


- Question 2: What is the number of trips made by different subscriber types?
  * Answer: Subscribers made 846,839 trips or 86.09% of all trips. This includes annual or 30-day members. Customers made 136,809 trips or 13.91% of all trips. This includes 24-hour or 3-day members.
  * SQL query:
  ```sql
  SELECT subscriber_type, count(distinct trip_id) as num_trips, ROUND((count(distinct trip_id)/983648)*100, 2) as percentage
  FROM `bigquery-public-data.san_francisco.bikeshare_trips`
  group by subscriber_type order by num_trips DESC
  ```

    Row | subscriber_type | num_trips | percentage
    --- | --- | --- | ---
    1 | Subscriber | 846839 | 86.09
    2 | Customer | 136809 | 13.91


- Question 3: What is the average trip duration?
  * Answer: The average duration of a trip is about 16.5 minutes.

  * SQL query:
  ```sql
  # find outliers: different durations for bins
  WITH subtable_trip AS (
      SELECT 
          CASE 
              WHEN duration_sec / (3600*24) >= 14 THEN "> 14 days"
              WHEN duration_sec / (3600*24) >= 7 THEN "> 7 days"
              WHEN duration_sec / (3600*24) >= 1 THEN "> 1 day"
              WHEN duration_sec / (3600) < 24 AND duration_sec / (3600) >= 10 THEN "10-24 hours"
              WHEN duration_sec / (3600) < 10 AND duration_sec / (3600) >= 5 THEN "5-10 hours"
              WHEN duration_sec / (3600) < 5 AND duration_sec / (3600) >= 4 THEN "4-5 hours"
              WHEN duration_sec / (3600) < 4 AND duration_sec / (3600) >= 3 THEN "3-4 hours"
              WHEN duration_sec / (3600) < 3 AND duration_sec / (3600) >= 2 THEN "2-3 hours"
              WHEN duration_sec / (3600) < 2 AND duration_sec / (3600) >= 1 THEN "1-2 hours"
              WHEN duration_sec / (60) < 60 AND duration_sec / (60) >= 30 THEN "30-60 minutes"
              WHEN duration_sec / (60) < 30 AND duration_sec / (60) >= 20 THEN "20-30 minutes"
              WHEN duration_sec / (60) < 20 AND duration_sec / (60) >= 10 THEN "10-20 minutes"
              WHEN duration_sec / (60) < 10 AND duration_sec / (60) >= 5 THEN "5-10 minutes"
              WHEN duration_sec / (60) < 5  THEN "< 5 minutes"
          END AS duration_category
          FROM 
              `bigquery-public-data.san_francisco.bikeshare_trips`       
  )
  SELECT duration_category, count(duration_category) as num_trips, round(100*count(duration_category)/983648, 2) as percent_trips
  FROM subtable_trip
  group by duration_category 
  order by (percent_trips) DESC
  ```

    | Row | duration_category | num_trips | percent_trips |
    |-----|-------------------|-----------|---------------|
    | 1   | 5-10 minutes      | 415936    | 42.29         |
    | 2   | 10-20 minutes     | 303297    | 30.83         |
    | 3   | < 5 minutes       | 178692    | 18.17         |
    | 4   | 20-30 minutes     | 38355     | 3.9           |
    | 5   | 30-60 minutes     | 19272     | 1.96          |
    | 6   | 1-2 hours         | 11893     | 1.21          |
    | 7   | 2-3 hours         | 5629      | 0.57          |
    | 8   | 5-10 hours        | 3454      | 0.35          |
    | 9   | 3-4 hours         | 3282      | 0.33          |
    | 10  | 4-5 hours         | 2202      | 0.22          |
    | 11  | 10-24 hours       | 1340      | 0.14          |
    | 12  | > 1 day           | 283       | 0.03          |
    | 13  | > 14 days         | 3         | 0.0           |
    | 14  | > 7 days          | 10        | 0.0           |

This query shows that there are 13 trips over 7 days, and they account for less than 0.01% of the total number of trips. However some of these long trips include trips spanning over 2 weeks and even a trip spanning 200 days. These are outliers for trip duration because they do not reflect the use case of our bikeshare service and inflate the average trip duration. We cannot make business decisions through the inclusion of these trips that are outside of our nominal operations. We do not typically allow users to take bikes for over a week.

Therefore the average trip duration should be calculated by including only trips under 1 week in duration.


  * SQL query:
  ```sql
  # filter outliers: 
  # Trips over 7 days account for less than 0.01% of the total trips, and serve as outliers for the starting time
  SELECT avg(duration_sec) as avg_trip_duration, round((avg(duration_sec)/60),3) as avg_trip_duration_min 
  FROM `bigquery-public-data.san_francisco.bikeshare_trips`
  WHERE duration_sec/3600/24 < 7 ---No trips over 7 days
  ```

  | Row | avg_trip_duration | avg_trip_duration_min |
  |-----|-------------------|-----------------------|
  | 1   | 989.983637223134  | 16.5                  |

### Bonus activity queries (optional - not graded - just this section is optional, all other sections are required)

The bike share dynamic dataset offers multiple tables that can be joined to learn more interesting facts about the bike share business across all regions. These advanced queries are designed to challenge you to explore the other tables, using only the available metadata to create views that give you a broader understanding of the overall volumes across the regions(each region has multiple stations)

We can create a temporary table or view against the dynamic dataset to join to our static dataset.

Here is some SQL to pull the region_id and station_id from the dynamic dataset.  You can save the results of this query to a temporary table or view.  You can then join the static tables to this table or view to find the region:
```sql
#standardSQL
select distinct region_id, station_id
from `bigquery-public-data.san_francisco_bikeshare.bikeshare_station_info`
```

- Top 5 popular station pairs in each region

- Top 3 most popular regions(stations belong within 1 region)

- Total trips for each short station name in each region

- What are the top 10 used bikes in each of the top 3 region. these bikes could be in need of more frequent maintenance.

---

## Part 2 - Querying data from the BigQuery CLI

- Use BQ from the Linux command line:

  * General query structure

    ```
    bq query --use_legacy_sql=false '
        SELECT count(*)
        FROM
           `bigquery-public-data.san_francisco.bikeshare_trips`'
    ```

### Queries

1. Rerun the first 3 queries from Part 1 using bq command line tool (Paste your bq
   queries and results here, using properly formatted markdown):

  * What's the size of this dataset? (i.e., how many trips)

  ```
  bq query --use_legacy_sql=false '
        SELECT count(distinct trip_id) as total_trips
        FROM `bigquery-public-data.san_francisco.bikeshare_trips`'
  ```
```
  +-------------+
  | total_trips |
  +-------------+
  |      983648 |
  +-------------+
```


  * What is the earliest start time and latest end time for a trip?

  ```
  bq query --use_legacy_sql=false '
        SELECT min(start_date) as earliest_start_date, max(end_date) as latest_end_date
        FROM `bigquery-public-data.san_francisco.bikeshare_trips`'
  ```
```
+---------------------+---------------------+
| earliest_start_date |   latest_end_date   |
+---------------------+---------------------+
| 2013-08-29 09:08:00 | 2016-08-31 23:48:00 |
+---------------------+---------------------+
```

  * How many bikes are there?

  ```
  bq query --use_legacy_sql=false '
      SELECT count(distinct bike_number) as total_unique_bikes
      FROM `bigquery-public-data.san_francisco.bikeshare_trips`'
  ```
```
+--------------------+
| total_unique_bikes |
+--------------------+
|                700 |
+--------------------+
```


2. New Query (Run using bq and paste your SQL query and answer the question in a sentence, using properly formatted markdown):

  * How many trips are in the morning vs in the afternoon?
  * Answer: Defining trips made between 6AM and 12PM as morning trips and trips made between 12PM and 6PM as afternoon trips, then there are 399,821 trips in the morning vs 391,199 trips in the afternoon.

  ```
  bq query --use_legacy_sql=false '
  SELECT time_of_day, count(distinct trip_id) as num_trips
  FROM (
    SELECT
    *, EXTRACT(HOUR FROM start_date) as starting_hour,
    CASE WHEN EXTRACT(HOUR FROM start_date) >= 6 AND EXTRACT(HOUR FROM start_date) < 12 THEN "morning (6am to 12 pm)"
    WHEN EXTRACT(HOUR FROM start_date) >= 12 AND EXTRACT(HOUR FROM start_date) < 18 THEN "afternoon (12pm to 6 pm)"
    WHEN EXTRACT(HOUR FROM start_date) >= 18 AND EXTRACT(HOUR FROM start_date) < 21 THEN "evening (6pm to 9pm)"
    ELSE "night (9pm to 6am)"
    END AS time_of_day
    FROM `bigquery-public-data.san_francisco.bikeshare_trips`
)
group by time_of_day
order by (num_trips) DESC'
  ```

  ```
  +--------------------------+-----------+
  |       time_of_day        | num_trips |
  +--------------------------+-----------+
  | morning (6am to 12 pm)   |    399821 |
  | afternoon (12pm to 6 pm) |    391199 |
  | evening (6pm to 9pm)     |    148387 |
  | night (9pm to 6am)       |     44241 |
  +--------------------------+-----------+
  ```


### Project Questions
Identify the main questions you'll need to answer to make recommendations (list
below, add as many questions as you need).

- Question 1: What are the most frequent weekday trips made in the morning rush hour times (6am to 9 am)?

- Question 2: What are the most frequent weekday trips made in the evening rush hour times (4pm to 7pm)?

- Question 3: What are the most frequent weekday trips made in the morning or evening rush hour times?

- Question 4: What is the percentage of total trips that are commuter trips? What percentage of commuter trips are taken by subscribers vs customers?

- Question 5: What are the most popular commuter stations? Looking at the top 10 commuter trips.

- Question 6: What is the average trip duration for the most popular commuter trips?

- Question 7: What is the number of trips taken by month? By Day? What is the average trip duration by month, by day?

- Question 8: How many trips would be affected if we reduced the include time for subscriber trips to 15 min? Limiting this cap during weekdays?

- ...

- Question n:

### Answers

Answer at least 4 of the questions you identified above You can use either
BigQuery or the bq command line tool.  Paste your questions, queries and
answers below.

- Question 1: What are the most frequent weekday trips made in the morning rush hour times (6am to 9 am)?
  * Answer:
  * SQL query:

  ```sql
  WITH base_tbl_tripsByWeekday AS (
    SELECT
    *, EXTRACT(DAYOFWEEK FROM start_date) as DOW, EXTRACT(HOUR FROM start_date) as starting_hour, EXTRACT(HOUR FROM end_date) as ending_hour
    FROM `bigquery-public-data.san_francisco.bikeshare_trips` 
    WHERE EXTRACT(DAYOFWEEK FROM start_date) BETWEEN 2 AND 6
  )
  SELECT start_station_name, end_station_name, count(distinct trip_id) as num_trips
  FROM base_tbl_tripsByWeekday
  WHERE starting_hour BETWEEN 6 AND 9
  group by start_station_name, end_station_name 
  order by (num_trips) DESC
  LIMIT 5
  ```

  Row |	start_station_name |	end_station_name |	num_trips
  --- | --- | --- | ---
  1	| Harry Bridges Plaza (Ferry Building) | 2nd at Townsend | 4796
  2	| Steuart at Market | 2nd at Townsend | 3815
  3	| San Francisco Caltrain (Townsend at 4th) | Temporary Transbay Terminal (Howard at Beale) | 3810
  4	| San Francisco Caltrain (Townsend at 4th) | Embarcadero at Folsom | 3615
  5	| San Francisco Caltrain 2 (330 Townsend) | Townsend at 7th | 3592

- Question 2: What are the most frequent weekday trips made in the evening rush hour times (3pm to 7pm)?
  * Answer:
  * SQL query:

  ```sql
  WITH base_tbl_tripsByWeekday AS (
    SELECT
    *, EXTRACT(DAYOFWEEK FROM start_date) as DOW, EXTRACT(HOUR FROM start_date) as starting_hour, EXTRACT(HOUR FROM end_date) as ending_hour
    FROM `bigquery-public-data.san_francisco.bikeshare_trips` 
    WHERE EXTRACT(DAYOFWEEK FROM start_date) BETWEEN 2 AND 6
  )
  SELECT start_station_name, end_station_name, count(distinct trip_id) as num_trips
  FROM base_tbl_tripsByWeekday
  WHERE starting_hour BETWEEN 16 AND 19
  group by start_station_name, end_station_name 
  order by (num_trips) DESC
  LIMIT 5
  ```

  Row |	start_station_name |	end_station_name |	num_trips
  --- | --- | --- | ---
  1	| 2nd at Townsend | Harry Bridges Plaza (Ferry Building) | 4329
  2	| Embarcadero at Folsom | San Francisco Caltrain (Townsend at 4th) | 4134
  3	| Embarcadero at Sansome | Steuart at Market | 4106
  4	| 2nd at South Park |Market at Sansome | 3539
  5	| Steuart at Market | San Francisco Caltrain (Townsend at 4th) | 3516

- Question 3: What are the most frequent weekday trips made in the morning or evening rush hour times?
  * Answer:
  * SQL query:

  ```sql
  WITH base_tbl_tripsByWeekday AS (
      SELECT
      *, EXTRACT(DAYOFWEEK FROM start_date) as DOW, EXTRACT(HOUR FROM start_date) as starting_hour, EXTRACT(HOUR FROM end_date) as ending_hour
      FROM `bigquery-public-data.san_francisco.bikeshare_trips` 
      WHERE EXTRACT(DAYOFWEEK FROM start_date) BETWEEN 2 AND 6
  )
  SELECT start_station_name, end_station_name, count(distinct trip_id) as num_trips, avg(starting_hour) as average_starting_hour
  FROM base_tbl_tripsByWeekday
  WHERE (starting_hour BETWEEN 6 AND 9) OR (starting_hour BETWEEN 16 AND 19)
  group by start_station_name, end_station_name 
  order by (num_trips) DESC
  LIMIT 5
  ```


  Row	| start_station_name |	end_station_name |	num_trips |	average_starting_hour
  --- | --- | --- | --- | ---
  1	| San Francisco Caltrain 2 (330 Townsend) | Townsend at 7th | 5741 | 11.764849329385118
  2	| Harry Bridges Plaza (Ferry Building) | 2nd at Townsend | 5539 | 9.184329301317941
  3	| 2nd at Townsend | Harry Bridges Plaza (Ferry Building) | 5322 | 15.231491920330699
  4	| Embarcadero at Sansome | Steuart at Market | 5164 | 15.190549961270307
  5	| San Francisco Caltrain (Townsend at 4th) | Harry Bridges Plaza (Ferry Building) | 5087 | 11.155887556516609



- Question 4: What is the percentage of total trips that are commuter trips? What percentage of commuter trips are taken by subscribers vs customers?
  * Answer: 62.7% of all trips are commuter trips, they start during morning or evening rush hour times during the weekdays. Of these commuter trips, 94.3% of them are subscribers and 5.7% are customers. This means that the majority of our riders are commuters and the majority of these commuters are already subscribers. The second most frequent trips taken are also during the weekday, outside of rush hour. Even then, subscribers make up 81.7% of non commuter trip riders on weekdays. On weekends, there is almost a 50/50 split between subscribers and customers. Weekend trips account for roughly 11.4% of all trips, while weekdays account for 89.6% of all trips.
  * SQL query:

  ```sql
  SELECT 
    during_week,
    commuter_hours,
    round(100*COUNT(distinct trip_id)/983648, 1) as percent_total_trips,
    COUNT(distinct trip_id) as num_trips,
    round(100*SUM(is_sub)/COUNT(distinct trip_id), 1) as percent_subscribers,
    round(100*SUM(is_cus)/COUNT(distinct trip_id), 1) as percent_customers,
    FROM (
        SELECT 
            *,
            CONCAT(start_station_name, ' ---> ', end_station_name) as start_end_pair,
            EXTRACT (HOUR from start_date) as starting_hour,
            CASE
                WHEN EXTRACT(DAYOFWEEK FROM start_date) BETWEEN 2 AND 6 THEN "weekday"
                ELSE "weekend"
            END as during_week,
            CASE 
                WHEN (EXTRACT(HOUR FROM start_date) BETWEEN 6 AND 9) OR (EXTRACT(HOUR FROM start_date) BETWEEN 16 AND 19) THEN "yes"
                ELSE "no"
            END AS commuter_hours,
            CASE 
                WHEN (subscriber_type = "Subscriber") THEN 1
                ELSE 0
            END AS is_sub,
            CASE 
                WHEN (subscriber_type = "Customer") THEN 1
                ELSE 0
            END AS is_cus,
        FROM `bigquery-public-data.san_francisco.bikeshare_trips`
        WHERE duration_sec/2600/24 < 7
    )
  ---during morning or evening rush hour times 
  group by during_week, commuter_hours
  order by percent_total_trips
  DESC
  ```
  | Row | during_week | commuter_hours | percent_total_trips | num_trips | percent_subscribers | percent_customers |
  |-----|-------------|----------------|---------------------|-----------|---------------------|-------------------|
  | 1   | weekday     | yes            | 62.7                | 617090    | 94.3                | 5.7               |
  | 2   | weekday     | no             | 25.9                | 254891    | 81.7                | 18.3              |
  | 3   | weekend     | no             | 7.3                 | 71413     | 49.0                | 51.0              |
  | 4   | weekend     | yes            | 4.1                 | 40228     | 53.6                | 46.4              |



- Question 5: What are the most popular commuter stations? Looking at the top 10 commuter trips.
  * Answer: Among the top 10 commuter trips, counting both start stations and end stations, the most popular commuter stations were the following 9 stations. These stations encompass all of the top commuter trips in the morning and evening. They are the stations most relied upon for commuter trips. In fact, looking at the top 100 most popular commuter trips reveals that only 36 stations are used. Commuter trips usually stop or end around a select few stations.
  * SQL query:
  ```sql
  WITH commuter_trips_tbl AS (
      WITH base_tbl_tripsByWeekday AS (
      SELECT 
      *, EXTRACT(DAYOFWEEK FROM start_date) as DOW, EXTRACT(HOUR FROM start_date) as starting_hour, EXTRACT(HOUR FROM end_date) as ending_hour
      FROM `bigquery-public-data.san_francisco.bikeshare_trips` 
      WHERE EXTRACT(DAYOFWEEK FROM start_date) BETWEEN 2 AND 6
      )
      SELECT start_station_name, start_station_id, end_station_name, end_station_id, count(distinct trip_id) as num_trips, avg(starting_hour) as average_starting_hour
      FROM base_tbl_tripsByWeekday
      WHERE ((starting_hour BETWEEN 6 AND 9) OR (starting_hour BETWEEN 16 AND 19)) AND (duration_sec/3600/24 < 7)
      group by start_station_name, start_station_id, end_station_name, end_station_id
      order by (num_trips) DESC
      LIMIT 10
  )
  SELECT start_station_name as commuter_station_name, start_station_id as station_id
  FROM commuter_trips_tbl
  UNION DISTINCT
  SELECT end_station_name, end_station_id
  FROM commuter_trips_tbl
  ```

  | Row | commuter_station_name                         | station_id          |
  |-----|-----------------------------------------------|---------------------|
  | 1   | San Francisco Caltrain 2 (330 Townsend)       | 69                  |
  | 2   | Harry Bridges Plaza (Ferry Building)          | 50                  |
  | 3   | 2nd at Townsend                               | 61                  |
  | 4   | Embarcadero at Sansome                        | 60                  |
  | 5   | San Francisco Caltrain (Townsend at 4th)      | 70                  |
  | 6   | Embarcadero at Folsom                         | 51                  |
  | 7   | Townsend at 7th                               | 65                  |
  | 8   | Steuart at Market                             | 74                  |
  | 9   | Temporary Transbay Terminal (Howard at Beale) | 55                  |

- Question 6: What is the average trip duration for the most popular commuter trips?
  * Answer: 
  * SQL query:


- Question 7: What is the number of trips taken by month? By Day? What is the average trip duration by month, by day?
  * Answer:
  * SQL query:


- Question 8: How many trips would be affected if we reduced the include time for subscriber trips to 15 min? Limiting this cap during weekdays?
  * Answer:
  * SQL query:

- ...

- Question n:
  * Answer:
  * SQL query:

---

## Part 3 - Employ notebooks to synthesize query project results

### Get Going

Create a Jupyter Notebook against a Python 3 kernel named Project_1.ipynb in the assignment branch of your repo.

#### Run queries in the notebook

At the end of this document is an example Jupyter Notebook you can take a look at and run.  

You can run queries using the "bang" command to shell out, such as this:

```
! bq query --use_legacy_sql=FALSE '<your-query-here>'
```

- NOTE:
- Queries that return over 16K rows will not run this way,
- Run groupbys etc in the bq web interface and save that as a table in BQ.
- Max rows is defaulted to 100, use the command line parameter `--max_rows=1000000` to make it larger
- Query those tables the same way as in `example.ipynb`

Or you can use the magic commands, such as this:

```sql
%%bigquery my_panda_data_frame

select start_station_name, end_station_name
from `bigquery-public-data.san_francisco.bikeshare_trips`
where start_station_name <> end_station_name
limit 10
```

```python
my_panda_data_frame
```

#### Report in the form of the Jupter Notebook named Project_1.ipynb

- Using markdown cells, MUST definitively state and answer the two project questions:

  * What are the 5 most popular trips that you would call "commuter trips"?

  * What are your recommendations for offers (justify based on your findings)?

- For any temporary tables (or views) that you created, include the SQL in markdown cells

- Use code cells for SQL you ran to load into Pandas, either using the !bq or the magic commands

- Use code cells to create Pandas formatted output tables (at least 3) to present or support your findings

- Use code cells to create simple data visualizations using Seaborn / Matplotlib (at least 2) to present or support your findings

### Resource: see example .ipynb file

[Example Notebook](example.ipynb)
