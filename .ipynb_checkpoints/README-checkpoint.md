# Project 1: Query Project

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

---

## Parts 1, 2, 3

We have broken down this project into 3 parts, about 1 week's work each to help you stay on track.

**You will only turn in the project once  at the end of part 3!**

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

```
SELECT COUNT(*)
  FROM `bigquery-public-data.san_francisco.bikeshare_trips`
```

We use the bikeshare_trips to find the total number of trips in the dataset, which is 983648.
 
- What is the earliest start date and time and latest end date and time for a trip?

```
SELECT MIN(start_date) earliest_start_date, MAX(end_date) latest_end_date
  FROM `bigquery-public-data.san_francisco.bikeshare_trips` 
```

The earliest start date and time for a trip is is 2013-08-29 09:08:00 PST, and the latest end date and time for a trip is 2016-08-31 23:48:00 PST. Note that based on the 'bikeshare_trips' scheme description, the time is already in PST, even though the timestamp is marked as UTC. 

- How many bikes are there?

```
SELECT COUNT(DISTINCT bike_number)
  FROM `bigquery-public-data.san_francisco.bikeshare_trips` 
```

The best guess for the number of bikes is based on the bike number, where we assume each bike is assigned a different ID. We don't account for retired bikes since we don't have this information, and assume that newly added bike take on a new ID. There are 700 distinct IDs, so we think there are 700 bikes during the time period of this dataset stated above.


### Questions of your own
- Make up 3 questions and answer them using the Bay Area Bike Share Trips Data.  These questions MUST be different than any of the questions and queries you ran above.

- Question 1: Based on station id and name from the bikeshare_stations table, which start and end stations in the bikeshare_trips table have different names when matched by station id? Are the mismatches between start and end stations different from each other? How many occurrences are there for each distinct difference, and is the mismatch potentially something of concern about the dataset?

  * Answer: There are 12 distinct entries where the start station name from the trips table is different than the station name in the station table when joined based on station id. For the end station name, there were also 12 distinct entries, with names and number of occurrences (trips) listed below:

Start station differences:
| station\_id | start\_station\_name                  | name                                 | num\_trips |
|-------------|---------------------------------------|--------------------------------------|------------|
| 47          | Post at Kearny                        | Post at Kearney                      | 11308      |
| 46          | Washington at Kearny                  | Washington at Kearney                | 7136       |
| 30          | Evelyn Park and Ride                  | Middlefield Light Rail Station       | 1738       |
| 33          | Rengstorff Avenue / California Street | Charleston Park/ North Bayshore Area | 1171       |
| 26          | Kaiser Hospital                       | Redwood City Medical Center          | 147        |
| 83          | Mezes                                 | Mezes Park                           | 119        |
| 89          | S\. Market St at Park Ave             | S\. Market st at Park Ave            | 84         |
| 25          | Broadway at Main                      | Stanford in Redwood City             | 67         |
| 80          | San Jose Government Center            | Santa Clara County Civic Center      | 23         |
| 88          | 5th S at E\. San Salvador St          | 5th S\. at E\. San Salvador St       | 19         |
| 21          | Sequoia Hospital                      | Franklin at Maple                    | 15         |
| 88          | 5th St at E\. San Salvador St         | 5th S\. at E\. San Salvador St       | 1          |


End station differences:
| station\_id | end\_station\_name                    | name                                 | num\_trips |
|-------------|---------------------------------------|--------------------------------------|------------|
| 47          | Post at Kearny                        | Post at Kearney                      | 11201      |
| 46          | Washington at Kearny                  | Washington at Kearney                | 8654       |
| 30          | Evelyn Park and Ride                  | Middlefield Light Rail Station       | 1446       |
| 33          | Rengstorff Avenue / California Street | Charleston Park/ North Bayshore Area | 1123       |
| 83          | Mezes                                 | Mezes Park                           | 114        |
| 26          | Kaiser Hospital                       | Redwood City Medical Center          | 108        |
| 89          | S\. Market St at Park Ave             | S\. Market st at Park Ave            | 103        |
| 25          | Broadway at Main                      | Stanford in Redwood City             | 81         |
| 88          | 5th S at E\. San Salvador St          | 5th S\. at E\. San Salvador St       | 24         |
| 80          | San Jose Government Center            | Santa Clara County Civic Center      | 23         |
| 21          | Sequoia Hospital                      | Franklin at Maple                    | 14         |
| 88          | 5th St at E\. San Salvador St         | 5th S\. at E\. San Salvador St       | 1          |

First of all, the start station/end station names differ from the stations table in the exact same station ids. Based on the differences above, many of them appear to be misspellings, such as "Kearny" in the trips table vs "Kearney" in the stations table, or the trips table calling "Mezes Part" simply as "Mezes". Since these likely do not represent different locations, these are of no concern for the integrity of the data for analysis, although better normalization of the dataset would be sensible. Some pairs have different names that represent the same location based on Google Map searches. These could be two landmark points in the same area, or different names for the same complex. This includes "Evelyn Park and Ride" vs "Middlefield Light Rail Station", "Kaiser Hospital" vs "Redwood City Medical Center", "San Jose Government Center" and "Santa Clara County Civic Center". The only suspicious pair is "Rengstorff Avenue / California Street" and "Charleston Park", which according to Google Maps is a 12 minute bike ride from each other. However, likely a single bikeshare station (id:33) serves both areas. The conclusion is that as long as work with the station id table, the data seems to have no issues in terms of start/end locations.

  * SQL query:

Stations where start station name from trips table differs from stations table
```
SELECT stations.station_id, trips.start_station_name, stations.name, COUNT(*) num_trips
FROM `bigquery-public-data.san_francisco.bikeshare_trips` trips
    JOIN `bigquery-public-data.san_francisco.bikeshare_stations` stations
    ON trips.start_station_id = stations.station_id
WHERE stations.name != trips.start_station_name
GROUP BY stations.station_id, trips.start_station_name, stations.name
ORDER BY num_trips DESC;
```

Stations where end station name from trips table differs from stations table
```
SELECT stations.station_id, trips.end_station_name, stations.name, COUNT(*) num_trips
FROM `bigquery-public-data.san_francisco.bikeshare_trips` trips
    JOIN `bigquery-public-data.san_francisco.bikeshare_stations` stations
    ON trips.end_station_id = stations.station_id
WHERE stations.name != trips.end_station_name
GROUP BY stations.station_id, trips.end_station_name, stations.name
ORDER BY num_trips DESC
```

- Question 2: How many weekday bikeshare trips begins between 6am and 8:59am, or starts between 5pm and 7:59pm, PST, where the start and end stations are different? Is this a large number of trips? These could signify the majority of weekday commuters.
  * Answer: First, we note that the times are already local PST based on the table schema (despite the table itself saying UTC). There are 442842 trips, or about 45% of total trips, that are weekday trips and begin during these timeframes. This is a significantly smaller subsection of the data. However, since we are only covering a 6 hour window each day for 5 days (30 hours total versus 168 hours per week), proportionally speaking a large fraction of trips are taken within this window.
  
  * SQL query:

```
SELECT COUNT(*)
  FROM `bigquery-public-data.san_francisco.bikeshare_trips` 
WHERE EXTRACT(DAYOFWEEK FROM start_date) BETWEEN 2 AND 6
AND (EXTRACT(HOUR FROM start_date) BETWEEN 6 AND 8
    OR EXTRACT(HOUR FROM start_date) BETWEEN 17 AND 19)
AND start_station_id != end_station_id
```

- Question 3: For the two different subscriber types, what percent of trips for each type are intercity (start station and end station are in different cities)? Should we specifically consider these trips as potentially not commuter trips during our analysis?

  * Answer: For "Customers", 0.522% of trips are intercity, whereas only 0.087% trips are intercity for "Subscribers", which means Customers take intercity trips 6 times more often than Subscribers. Commuter trips are less likely to be intercity, so this combined with trip duration could help us narrow down which trips are commuter. However, the number of intercity trips represents such a small fraction of total trips that special treatment of such trips may not be impactful on the analysis.
  
  * SQL query:

```
SELECT ROUND(num_trips / total_trips * 100, 3) intercity_trips_pct, t1.subscriber_type
FROM
  (SELECT COUNT(*) num_trips, subscriber_type
   FROM `bigquery-public-data.san_francisco.bikeshare_trips` trips
      JOIN `bigquery-public-data.san_francisco.bikeshare_stations` stations_1
      ON trips.start_station_id = stations_1.station_id
      JOIN `bigquery-public-data.san_francisco.bikeshare_stations` stations_2
      ON trips.end_station_id = stations_2.station_id
    WHERE stations_1.landmark != stations_2.landmark
    GROUP BY subscriber_type) t1
JOIN
    (SELECT subscriber_type, COUNT(*) total_trips
     FROM `bigquery-public-data.san_francisco.bikeshare_trips`
     GROUP BY subscriber_type) t2
ON t1.subscriber_type = t2.subscriber_type

```
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

```
SELECT station_pair, region, station_pair_num_trips
FROM
  (SELECT *, ROW_NUMBER() OVER (PARTITION BY region ORDER BY station_pair_num_trips DESC) r, 
  FROM
    (SELECT LEAST(trips.start_station_name) || ', ' || GREATEST(trips.end_station_name) station_pair, regions_1.region_id region, COUNT(*) station_pair_num_trips
      FROM `bigquery-public-data.san_francisco.bikeshare_trips` trips
      JOIN `round-ring-276215.bikeshare_views.region_station` regions_1
      ON trips.start_station_id = regions_1.station_id
      JOIN `round-ring-276215.bikeshare_views.region_station` regions_2
      ON trips.end_station_id  = regions_2.station_id
    WHERE regions_1.region_id = regions_2.region_id
    GROUP BY 1, 2
    ORDER BY 3 DESC) t1) t2
  WHERE r <= 5
```

| station\_pair                                                     | region | station\_pair\_num\_trips |
|-------------------------------------------------------------------|--------|---------------------------|
| Harry Bridges Plaza \(Ferry Building\), Embarcadero at Sansome    | 3      | 9150                      |
| 2nd at Townsend, Harry Bridges Plaza \(Ferry Building\)           | 3      | 7620                      |
| Harry Bridges Plaza \(Ferry Building\), 2nd at Townsend           | 3      | 6888                      |
| Embarcadero at Sansome, Steuart at Market                         | 3      | 6874                      |
| Embarcadero at Folsom, San Francisco Caltrain \(Townsend at 4th\) | 3      | 6351                      |
| University and Emerson, University and Emerson                    | 5      | 1184                      |
| Washington at Kearny, Washington at Kearny                        | 12     | 314                       |
| Paseo de San Antonio, Paseo de San Antonio                        | 12     | 204                       |
| Washington at Kearney, Washington at Kearney                      | 12     | 89                        |


- Top 3 most popular regions(stations belong within 1 region)

```
SELECT region, COUNT(*) num_trips
FROM
(SELECT regions_1.region_id region
  FROM `bigquery-public-data.san_francisco.bikeshare_trips` trips
  JOIN `round-ring-276215.bikeshare_views.region_station` regions_1
  ON trips.start_station_id = regions_1.station_id
UNION ALL
SELECT regions_2.region_id region
  FROM `bigquery-public-data.san_francisco.bikeshare_trips` trips
  JOIN `round-ring-276215.bikeshare_views.region_station` regions_2
  ON trips.end_station_id = regions_2.station_id)
GROUP BY region
ORDER BY 2 DESC
```

| region | num\_trips |
|--------|------------|
| 3      | 1492263    |
| 12     | 25534      |
| 5      | 4353       |


- Total trips for each short station name in each region

```
SELECT station_name, COUNT(*) num_trips
FROM
(SELECT trips.start_station_name station_name
  FROM `bigquery-public-data.san_francisco.bikeshare_trips` trips
UNION ALL
SELECT trips.end_station_name station_name 
  FROM `bigquery-public-data.san_francisco.bikeshare_trips` trips)
GROUP BY station_name
ORDER BY num_trips DESC
```

- What are the top 10 used bikes in each of the top 3 region. these bikes could be in need of more frequent maintenance.

```
SELECT bike_number, region, num_uses
FROM
  (SELECT *, ROW_NUMBER() OVER (PARTITION BY region ORDER BY num_uses DESC) r
    FROM
    (SELECT bike_number, region, COUNT(*) num_uses 
    FROM
    (SELECT trips.bike_number, regions_1.region_id region
      FROM `bigquery-public-data.san_francisco.bikeshare_trips` trips
      JOIN `round-ring-276215.bikeshare_views.region_station` regions_1
      ON trips.start_station_id = regions_1.station_id
      WHERE region_id IN (3, 5, 12)
    UNION ALL
    SELECT trips.bike_number, regions_2.region_id region
      FROM `bigquery-public-data.san_francisco.bikeshare_trips` trips
      JOIN `round-ring-276215.bikeshare_views.region_station` regions_2
      ON trips.end_station_id = regions_2.station_id
      WHERE region_id IN (3, 5, 12))
  GROUP BY bike_number, region))
WHERE r <= 10
```

| bike\_number | region | num\_uses |
|--------------|--------|-----------|
| 389          | 3      | 4418      |
| 524          | 3      | 4372      |
| 631          | 3      | 4350      |
| 392          | 3      | 4323      |
| 328          | 3      | 4307      |
| 585          | 3      | 4303      |
| 558          | 3      | 4301      |
| 287          | 3      | 4291      |
| 503          | 3      | 4266      |
| 592          | 3      | 4264      |
| 165          | 5      | 60        |
| 638          | 5      | 55        |
| 120          | 5      | 54        |
| 71           | 5      | 51        |
| 307          | 5      | 50        |
| 20           | 5      | 48        |
| 83           | 5      | 48        |
| 9            | 5      | 47        |
| 201          | 5      | 47        |
| 160          | 5      | 45        |
| 328          | 12     | 87        |
| 275          | 12     | 85        |
| 353          | 12     | 83        |
| 480          | 12     | 80        |
| 388          | 12     | 79        |
| 340          | 12     | 79        |
| 284          | 12     | 78        |
| 99           | 12     | 77        |
| 489          | 12     | 77        |
| 487          | 12     | 76        |


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
  bq query --use_legacy_sql=false 'SELECT COUNT(*) dataset_size FROM `bigquery-public-data.san_francisco.bikeshare_trips`'
  ```

| dataset\_size |
|---------------|
| 983648        |          
  

  * What is the earliest start time and latest end time for a trip?

  ```
  bq query --use_legacy_sql=false 'SELECT MIN(start_date) earliest_start_date, MAX(end_date) latest_end_date
    FROM `bigquery-public-data.san_francisco.bikeshare_trips`'
  ```

| earliest\_start\_date | latest\_end\_date   |
|-----------------------|---------------------|
| 2013-08-29 09:08:00   | 2016-08-31 23:48:00 |
  
  * How many bikes are there?

  ```
  bq query --use_legacy_sql=false 'SELECT COUNT(DISTINCT bike_number) num_bikes
     FROM `bigquery-public-data.san_francisco.bikeshare_trips`' 
  ```

| num\_bikes |
|------------|
| 700        |

2. New Query (Run using bq and paste your SQL query and answer the question in a sentence, using properly formatted markdown):

  * How many trips are in the morning vs in the afternoon?

In this problem, I will define morning hours as between 6am and 8:59am, and afternoon hours as between 5pm and 7:59pm. In order to define morning and afternoon trips, we will consider a trip a morning trip if it begins in the morning and ends the same day, and an afternoon trip if it begins in the afternoon and ends on the same day. The same day is to exclude multi-day bike checkouts because we do not know for sure when the bike was actually used during that period. Note that the timestamps are already in PST as defined by the schema. We also disregard whether the trip was a roundtrip or if the start/end stations differed.

```
bq query --use_legacy_sql=false 'SELECT COUNT(*) num_trips,
  CASE WHEN EXTRACT(HOUR FROM start_date) BETWEEN 6 AND 8 THEN "morning"
       WHEN EXTRACT(HOUR FROM start_date) BETWEEN 17 AND 19 THEN "afternoon"
       ELSE "neither"
  END morning_afternoon
FROM
(SELECT * FROM `bigquery-public-data.san_francisco.bikeshare_trips` 
WHERE EXTRACT(DATE FROM start_date) = EXTRACT(DATE FROM end_date))
GROUP BY 2
ORDER BY morning_afternoon'
```

| num\_trips | morning\_afternoon |
|------------|--------------------|
| 251468     | afternoon          |
| 220472     | morning            |
| 509125     | neither            |

For trips that began and ended on the same day, there were 251468 that began during afternoon hours, and 220472 that began during morning hours. 509125 trips began during other hours of the day. Therefore, there are slightly more afternoon trips than morning trips, although the ratio is pretty close to 50:50. For a 6-hour morning afternoon window, almost half of the trips were taken during this time, so that accounts for a disproportionally large number of trips compared to the other hours.

### Project Questions
Identify the main questions you'll need to answer to make recommendations (list
below, add as many questions as you need).

=======
- Question 0: What are the 5 most popular trips that you would call "commuter trips"? 

- Question 1: What are the 5 most popular end stations from "commuter trips" in the morning commuter hours or most popular start stations from "commuter trips" in the afternoon hours? 

- Question 2: For the most popular commuter trips, which have the highest number of Customers compared to Subscribers?

- Question 3: Which stations have the most free bikes and during which hours?

- Question 4: Which stations have the least amount of free bikes and during which hours? 

- Question 5: Which hours/days of the week has the most and least usage per station?

- Question 6: Which hours/days of the week has the most and least usage overall?

- Question 7: How many trips are under 5 minutes, and what might these trips be?

- Question 8: Which bikes see the least usage?

- Question 9: What is the average duration and standard deviation of the duration of commuter trips? 

- Question 10: How many trips conform to intended behavior of under 30mins? As discussed in class, there is a penalty for trips taking more than 30 minutes.
  
- Question 11: What are the top 5 pairs of stations for which mean trip duration is the longest? 

### Answers

Answer at least 4 of the questions you identified above You can use either
BigQuery or the bq command line tool.  Paste your questions, queries and
answers below.

All questions are answered with BigQuery.

- Question 0: What are the 5 most popular trips that you would call "commuter trips"? 

  * Answer:  In addition to this being a question we need to answer, the end stations of these trips could correspond to corporate business centers, so it would make sense to tap into this marker further by offering corporate discount at these locations. To narrow down a commuter trip, we use the following criteria:
  
  1. Trips should be taken on weekdays between 6am - 9:59am (prime time for leaving for work) or between 4pm - 7:59pm (prime time for leaving from work). The range should cover most of regular commuters, and provides a large enough window to account for individuals with different work hours (for example, younger people tend to work later hours and also arrive later).
  2. Trips should last longer than 5 minutes, but not more than 30 minutes. Since there is an extra charge for longer durations, this is not sustainable, and customers would likely find another way to commute if they had to ride for more than 30mins each way. Trips shorter than 5 minutes means the individual wants to visit a very close location, and it might make more sense to walk rather than routinely pay for a bike service that is rarely used. Evidence suggests that 20 minutes is the [average time](https://bikeleague.org/content/new-census-data-bike-commuting)
  3. Trips should not be counted for public U.S. holidays
  4. Trips should start and end at different locations
  5. A station pair order matters. For example, a trip at 8am from station A to station B does NOT count as the same station pair as a trip at 6pm from station B to station A. We count commuter trips starting and ending at different locations as distinct.
  
  In order to identify which days are holidays, I've created a 'holidays.csv' table, and imported it into BigQuery as a table called holidays:
  
  ```
  SELECT * FROM `round-ring-276215.bikeshare_views.holidays` 
  ORDER BY time
  ```
  The output table is the following:
  
| time         | day                           |
|--------------|-------------------------------|
| 2013\-01\-01 | New Year's Day                |
| 2013\-01\-21 | Martin Luther King Jr\. Day   |
| 2013\-02\-18 | Washington's Birthday         |
| 2013\-05\-27 | Memorial Day                  |
| 2013\-07\-04 | Independence Day              |
| 2013\-09\-02 | Labor Day                     |
| 2013\-10\-14 | Columbus Day                  |
| 2013\-11\-11 | Veterans Day                  |
| 2013\-11\-28 | Thanksgiving                  |
| 2013\-12\-25 | Christmas Day                 |
| 2014\-01\-01 | New Year's Day                |
| 2014\-01\-20 | Martin Luther King Jr\. Day   |
| 2014\-02\-17 | Washington's Birthday         |
| 2014\-05\-26 | Memorial Day                  |
| 2014\-07\-04 | Independence Day              |
| 2014\-09\-01 | Labor Day                     |
| 2014\-10\-13 | Columbus Day                  |
| 2014\-11\-11 | Veterans Day                  |
| 2014\-11\-27 | Thanksgiving                  |
| 2014\-12\-25 | Christmas Day                 |
| 2015\-01\-01 | New Year's Day                |
| 2015\-01\-19 | Martin Luther King Jr\. Day   |
| 2015\-02\-16 | Washington's Birthday         |
| 2015\-05\-25 | Memorial Day                  |
| 2015\-07\-03 | Independence Day \(Observed\) |
| 2015\-07\-04 | Independence Day              |
| 2015\-09\-07 | Labor Day                     |
| 2015\-10\-12 | Columbus Day                  |
| 2015\-11\-11 | Veterans Day                  |
| 2015\-11\-26 | Thanksgiving                  |
| 2015\-12\-25 | Christmas Day                 |
| 2016\-01\-01 | New Year's Day                |
| 2016\-01\-18 | Martin Luther King Jr\. Day   |
| 2016\-02\-15 | Washington's Birthday         |
| 2016\-05\-30 | Memorial Day                  |
| 2016\-07\-04 | Independence Day              |
| 2016\-09\-05 | Labor Day                     |
| 2016\-10\-10 | Columbus Day                  |
| 2016\-11\-11 | Veterans Day                  |
| 2016\-11\-24 | Thanksgiving                  |
| 2016\-12\-25 | Christmas Day                 |
| 2016\-12\-26 | Christmas Day \(Observed\)    |

The top 5 most popular commuter trips are below:

| start\_name                                | end\_name                                  | num\_trips |
|--------------------------------------------|--------------------------------------------|------------|
| Harry Bridges Plaza \(Ferry Building\)     | 2nd at Townsend                            | 5458       |
| 2nd at Townsend                            | Harry Bridges Plaza \(Ferry Building\)     | 5260       |
| San Francisco Caltrain \(Townsend at 4th\) | Harry Bridges Plaza \(Ferry Building\)     | 5019       |
| Embarcadero at Folsom                      | San Francisco Caltrain \(Townsend at 4th\) | 4850       |
| Steuart at Market                          | 2nd at Townsend                            | 4697       |


  * SQL query: 
  
  ```
  WITH commuter_trips AS 
    (SELECT * FROM
       #LEFT JOIN with the holidays table to capture all entries in the bikeshare_trips table
       #Keep only places the holidays table IS NULL (non-holidays)
      (SELECT trips.* FROM `bigquery-public-data.san_francisco.bikeshare_trips` trips
        LEFT JOIN `round-ring-276215.bikeshare_views.holidays` holidays
        ON holidays.time =  CAST(trips.start_date AS DATE)
        WHERE holidays.time IS NULL) trips_non_holiday
      WHERE EXTRACT(DAYOFWEEK FROM start_date) BETWEEN 2 AND 6
        AND (EXTRACT(HOUR FROM start_date) BETWEEN 6 AND 9
          OR EXTRACT(HOUR FROM start_date) BETWEEN 16 AND 19)
        AND duration_sec BETWEEN (60*5) AND (60*30)
        AND start_station_id != end_station_id)        
    SELECT stations_1.name start_name, stations_2.name end_name, COUNT(*) num_trips 
      FROM commuter_trips 
      JOIN `bigquery-public-data.san_francisco.bikeshare_stations` stations_1
      ON commuter_trips.start_station_id = stations_1.station_id 
      JOIN `bigquery-public-data.san_francisco.bikeshare_stations` stations_2
      ON commuter_trips.end_station_id = stations_2.station_id 
    GROUP BY start_name, end_name
    ORDER BY num_trips DESC
    LIMIT 5
  ```

In order to save query time in the future, we also create a new view for commuter trips as defined above:

```
CREATE VIEW `round-ring-276215.bikeshare_views.commuter_trips` AS
(SELECT trips_non_holiday.*, stations_1.name official_start_station_name, stations_2.name official_end_station_name FROM

   #LEFT JOIN with the holidays table to capture all entries in the bikeshare_trips table
   #Keep only places the holidays table IS NULL (non-holidays)

  (SELECT trips.* FROM `bigquery-public-data.san_francisco.bikeshare_trips` trips
    LEFT JOIN `round-ring-276215.bikeshare_views.holidays` holidays
    ON holidays.time =  CAST(trips.start_date AS DATE)
    WHERE holidays.time IS NULL) trips_non_holiday
    
  #JOIN to the stations table twice in order to get the official start and end station names
  #We discovered some name discrepancies in part 1
  
  JOIN `bigquery-public-data.san_francisco.bikeshare_stations` stations_1
    ON trips_non_holiday.start_station_id = stations_1.station_id 
  JOIN `bigquery-public-data.san_francisco.bikeshare_stations` stations_2
    ON trips_non_holiday.end_station_id = stations_2.station_id   
  WHERE EXTRACT(DAYOFWEEK FROM start_date) BETWEEN 2 AND 6
    AND (EXTRACT(HOUR FROM start_date) BETWEEN 6 AND 8
      OR EXTRACT(HOUR FROM start_date) BETWEEN 17 AND 19)
    AND duration_sec BETWEEN (60*5) AND (60*30)
    AND start_station_id != end_station_id)
```

- Question 1: What are the 5 most popular end stations from "commuter trips" in the morning commuter hours or most popular start stations from "commuter trips" in the afternoon hours?
  * Answer: In order to consider where we advertise for a corporate discount, we should narrow the query from the first question down further to consider only areas with high concentration of corporate offices. We expect that the highly used end stations of commuter trips in the morning, and highly used beginning stations in the afternoon, are also areas of high concentration for corporate offices. As a result, we also find the top 5 stations that fit this criteria by considering top used start end stations in the morning commuter trips, and top used start stations in the evening commuter trips:
  
| wanted\_station                                 | counts |
|-------------------------------------------------|--------|
| San Francisco Caltrain \(Townsend at 4th\)      | 86085  |
| San Francisco Caltrain 2 \(330 Townsend\)       | 54621  |
| Harry Bridges Plaza \(Ferry Building\)          | 36419  |
| Temporary Transbay Terminal \(Howard at Beale\) | 35985  |
| 2nd at Townsend                                 | 33107  |

As a result, it appears that the San Francisco Caltrain stations are great locations to advertise corporate offers. It may be that individuals drop off their bikes at this location in order to take the train, or pick up bikes from these locations after taking the train. 
  
  * SQL query:
  
  ```
  SELECT wanted_station, COUNT(*) counts FROM
    (SELECT official_start_station_name wanted_station FROM `round-ring-276215.bikeshare_views.commuter_trips` 
     UNION ALL
     SELECT official_end_station_name wanted_station FROM `round-ring-276215.bikeshare_views.commuter_trips`)
  GROUP BY wanted_station
  ORDER BY counts DESC
  LIMIT 5
  ```

- Question 2: For the most popular commuter trips, which have the highest number of Customers compared to Subscribers? 
  * Answer: For Customers (non-subscribers) who we consider to be commuting, it would be worthwhile to offer these individuals to join as Subscribers. This is because it is possible that these customers simply tried out the service once, and got too busy to consider the service again. We can prompt them with an advertise to join with an discounted annual membership for the first year/month. Ideally, we would track these individuals and provide offers only to these individuals. This question helps us determine on which routes we should look for such individuals. The results are below:
  
  
| start\_name                            | end\_name                              | num\_trips |
|----------------------------------------|----------------------------------------|------------|
| Harry Bridges Plaza \(Ferry Building\) | Embarcadero at Sansome                 | 316        |
| Embarcadero at Sansome                 | Harry Bridges Plaza \(Ferry Building\) | 176        |
| Embarcadero at Sansome                 | 2nd at Townsend                        | 128        |
| Embarcadero at Sansome                 | Steuart at Market                      | 127        |
| Harry Bridges Plaza \(Ferry Building\) | 2nd at Townsend                        | 113        |

It looks like Embarcadero at Sansome is a station where commuters tend to ride only once. It would be worthwhile to target these individual for a discounted annual membership.

  * SQL query:
  
  ```
  SELECT stations_1.name start_name, stations_2.name end_name, COUNT(*) num_trips 
  FROM `round-ring-276215.bikeshare_views.commuter_trips` commuter_trips
  JOIN `bigquery-public-data.san_francisco.bikeshare_stations` stations_1
    ON commuter_trips.start_station_id = stations_1.station_id 
  JOIN `bigquery-public-data.san_francisco.bikeshare_stations` stations_2
    ON commuter_trips.end_station_id = stations_2.station_id 
  WHERE subscriber_type = 'Customer'
  GROUP BY start_name, end_name
  ORDER BY num_trips DESC
  LIMIT 5
  ```

- Question 3: Which stations have the most free bikes and during which hours? (Taking bikes from here gets a discount)
  * Answer: For stations where there are lots of bikes available during certain hours, it would be worthwhile to offer discounts for individual single rides at these locations during these times. This would allow bikes to clear up from these stations, and also potentially land somewhere where there is a bike deficit. We also want to avoid full bike racks (cannot dock a bike), so this would help alleviate that. We will take the average number of free bikes available at each station averaged based on the hour.
  
| station\_name                             | hour | average\_num\_bikes\_available |
|-------------------------------------------|------|--------------------------------|
| 5th St at Folsom St                       | 19   | 20\.62                         |
| 5th St at Folsom St                       | 0    | 20\.1                          |
| 5th St at Folsom St                       | 18   | 20\.07                         |
| 5th St at Folsom St                       | 2    | 20\.04                         |
| 5th St at Folsom St                       | 5    | 20\.0                          |
| 5th St at Folsom St                       | 1    | 20\.0                          |
| 5th St at Folsom St                       | 4    | 19\.96                         |
| 5th St at Folsom St                       | 3    | 19\.93                         |
| 5th St at Folsom St                       | 23   | 19\.58                         |
| 5th St at Folsom St                       | 22   | 19\.43                         |
| 5th St at Folsom St                       | 21   | 19\.36                         |
| 5th St at Folsom St                       | 20   | 19\.31                         |
| 5th St at Folsom St                       | 17   | 18\.54                         |
| 5th St at Folsom St                       | 6    | 18\.04                         |
| Harry Bridges Plaza \(Ferry Building\)    | 18   | 16\.22                         |
| Harry Bridges Plaza \(Ferry Building\)    | 19   | 16\.17                         |
| Harry Bridges Plaza \(Ferry Building\)    | 20   | 16\.02                         |
| Harry Bridges Plaza \(Ferry Building\)    | 21   | 15\.97                         |
| San Francisco Caltrain 2 \(330 Townsend\) | 6    | 15\.88                         |
| Harry Bridges Plaza \(Ferry Building\)    | 22   | 15\.81                         |
| Harry Bridges Plaza \(Ferry Building\)    | 0    | 15\.73                         |
| Harry Bridges Plaza \(Ferry Building\)    | 1    | 15\.73                         |
| Harry Bridges Plaza \(Ferry Building\)    | 23   | 15\.73                         |
| Harry Bridges Plaza \(Ferry Building\)    | 2    | 15\.72                         |
| Harry Bridges Plaza \(Ferry Building\)    | 5    | 15\.71                         |

It would appear that 2 stations, 5th St at Folsom St and Harry Bridges Plaza, dominate in terms of the number of bikes available. Surprisingly, for 5th St at Folsom St, bikes are available at even non-odd hours, such as 6pm and 7pm, are both in the top 3 in terms of available bikes. It would be worthwhile to offer discounts to individuals who can checkout bikes during these hours from these stations in order to keep them moving.
  
  * SQL query:
  
  ```
  SELECT stations.name station_name, EXTRACT(HOUR FROM status.time) hour, AVG(status.bikes_available) average_num_bikes_available
  FROM `bigquery-public-data.san_francisco.bikeshare_status` status
  JOIN `bigquery-public-data.san_francisco.bikeshare_stations` stations
    ON status.station_id = stations.station_id 
  GROUP BY station_name, hour
  ORDER BY average_num_bikes_available DESC
  LIMIT 25
  ```
  
- Question 4: Which stations have the least amount of free bikes and during which hours? 
  * Answer: Similar to question 3, we want to find the most empty bike racks, and offer a discount to those who return bikes to these locations during the hours where the racks are empty. We want to avoid empty bike racks (cannot checkout a bike), so this would help alleviate that.

| station\_name                    | hour | average\_num\_bikes\_available |
|----------------------------------|------|--------------------------------|
| Cyril Magnin St at Ellis St      | 11   | 3\.59                          |
| Cyril Magnin St at Ellis St      | 10   | 3\.66                          |
| Cyril Magnin St at Ellis St      | 12   | 3\.79                          |
| Cyril Magnin St at Ellis St      | 9    | 3\.85                          |
| Cyril Magnin St at Ellis St      | 18   | 3\.89                          |
| Cyril Magnin St at Ellis St      | 19   | 3\.92                          |
| Castro Street and El Camino Real | 18   | 3\.93                          |
| Broadway St at Battery St        | 18   | 4\.01                          |
| Castro Street and El Camino Real | 19   | 4\.01                          |
| Cyril Magnin St at Ellis St      | 17   | 4\.06                          |
| Commercial at Montgomery         | 18   | 4\.1                           |
| Castro Street and El Camino Real | 20   | 4\.19                          |
| Commercial at Montgomery         | 2    | 4\.19                          |
| Commercial at Montgomery         | 3    | 4\.19                          |
| Commercial at Montgomery         | 0    | 4\.2                           |
| Commercial at Montgomery         | 1    | 4\.2                           |
| Cyril Magnin St at Ellis St      | 8    | 4\.21                          |
| Commercial at Montgomery         | 19   | 4\.21                          |
| Cyril Magnin St at Ellis St      | 14   | 4\.21                          |
| Castro Street and El Camino Real | 8    | 4\.24                          |
| Commercial at Montgomery         | 23   | 4\.25                          |
| Commercial at Montgomery         | 4    | 4\.25                          |
| Cyril Magnin St at Ellis St      | 16   | 4\.27                          |
| Castro Street and El Camino Real | 21   | 4\.27                          |
| Castro Street and El Camino Real | 7    | 4\.27                          |

Based on this data, Cyril Magnin St at Ellis St lacks bikes between 9am - 12an, and at 6pm - 7pm. As a result, offering a discount to dock bikes from another location here would be beneficial.
  
  * SQL query:
  
  ```
  SELECT stations.name station_name, EXTRACT(HOUR FROM status.time) hour, AVG(status.bikes_available) average_num_bikes_available
  FROM `bigquery-public-data.san_francisco.bikeshare_status` status
  JOIN `bigquery-public-data.san_francisco.bikeshare_stations` stations
    ON status.station_id = stations.station_id 
  GROUP BY station_name, hour
  ORDER BY average_num_bikes_available
  LIMIT 25
  ```
  
- Question 5: Which hours/days of the week has the most and least usage per station?
  * Answer: Looking at the dataset in a different way, we can also offer discounts on particular days in order to try to increase ridership on days where ridership is low (in other words, many bikes are docked rather than being used). This would be dependent on the station itself, and could be a general marketing scheme to everyone. For example, we might want to offer individuals who have never tried to service before to get a free ride during a particular day within a particular window just to test out the service. We would like to know which windows are least busy. More specifically, let's use a rolling window of 3 hours for each day, and find what the total ridership is during these hours of each day, and find the lowest. This way, we can offer ads such as: For first time riders, get a free ride on Mondays between 3pm and 6pm. Note that in order to get the best turnout, we likely want to choose reasonable hours (6am and 8pm) rather than odd hours (such as 2am - 5am). 
  
| station\_name       | day\_week | date\_hour | rolling\_avg\_3 |
|---------------------|-----------|------------|-----------------|
| 5th St at Folsom St | 6         | 19         | 28\.29          |
| 5th St at Folsom St | 6         | 20         | 27\.69          |
| 5th St at Folsom St | 7         | 6          | 25\.69          |
| 5th St at Folsom St | 6         | 18         | 25\.16          |
| 5th St at Folsom St | 7         | 7          | 23\.53          |
| 5th St at Folsom St | 6         | 17         | 22\.62          |
| 5th St at Folsom St | 7         | 8          | 22\.27          |
| 5th St at Folsom St | 5         | 17         | 22\.04          |
| 5th St at Folsom St | 7         | 9          | 22\.02          |
| 5th St at Folsom St | 5         | 18         | 21\.63          |
| 5th St at Folsom St | 7         | 10         | 21\.51          |
| 5th St at Folsom St | 1         | 16         | 21\.0           |
| 5th St at Folsom St | 1         | 18         | 21\.0           |
| 5th St at Folsom St | 1         | 15         | 21\.0           |
| 5th St at Folsom St | 1         | 17         | 21\.0           |
| 5th St at Folsom St | 1         | 14         | 21\.0           |
| 5th St at Folsom St | 1         | 19         | 20\.93          |
| 5th St at Folsom St | 7         | 11         | 20\.75          |
| 5th St at Folsom St | 5         | 19         | 20\.73          |
| 5th St at Folsom St | 1         | 13         | 20\.69          |

The results can be interpreted as the following: On Fridays (day_week = 6) between 7pm and 10pm, there are on average 28.29 bikes parked at 5th St at Folsom St. For similar 3-hour windows on Saturday, such as 8pm - 11pm, and 6pm - 9pm, 5th St at Folsom St is heavily packed with bikes. Similarly, on Saturday 6am-9am (along with other 3-hour windows beginning at 7, 8, and 9 am, there are lots of bikes at 5th St at Folsom St. A great campaign could be that customers who checkout a bike at 5th St at Folsom St for the first time during 6pm-9pm Fridays or 8am-11am on Saturdays get their first ride free.

  * SQL query:

  ```
    SELECT station_name, day_week, date_hour, ROUND(AVG(avg_avail_bikes) OVER (ORDER BY station_name, day_week, date_hour ROWS BETWEEN CURRENT ROW AND 2 FOLLOWING), 2) rolling_avg_3
    FROM (
        SELECT stations.name station_name, EXTRACT(DAYOFWEEK FROM time) day_week, EXTRACT(HOUR FROM time) date_hour, AVG(bikes_available) avg_avail_bikes
        FROM `bigquery-public-data.san_francisco.bikeshare_status` status
        JOIN `bigquery-public-data.san_francisco.bikeshare_stations` stations
        ON status.station_id = stations.station_id 
        GROUP BY 1, 2, 3
        ORDER BY 1, 2, 3)
    WHERE date_hour BETWEEN 6 AND 20
    ORDER BY rolling_avg_3 DESC
    LIMIT 20
  ```

- Question 6: Which hours/days of the week has the most and least usage overall?

  * Answer: The answer is the same above, except we now do not group based on the station ID.
  
| day\_week | date\_hour | rolling\_avg\_3 |
|-----------|------------|-----------------|
| 7         | 6          | 8\.41           |
| 6         | 20         | 8\.4            |
| 7         | 7          | 8\.38           |
| 1         | 6          | 8\.38           |
| 1         | 7          | 8\.35           |
| 7         | 20         | 8\.34           |
| 7         | 8          | 8\.34           |
| 6         | 19         | 8\.33           |
| 7         | 19         | 8\.31           |
| 1         | 8          | 8\.31           |
| 7         | 9          | 8\.29           |
| 1         | 19         | 8\.29           |
| 7         | 18         | 8\.28           |
| 5         | 20         | 8\.28           |
| 5         | 19         | 8\.28           |
| 1         | 18         | 8\.26           |
| 1         | 9          | 8\.26           |
| 7         | 10         | 8\.23           |
| 4         | 19         | 8\.23           |
| 7         | 17         | 8\.22           |

  Based on the results above, if we wanted to offer a discout for everyone and restricting only to day and hours, it would appear that Saturdays from 6am - 9am has on average the most number of free bikes. Closely following that is Friday from 8pm-10pm, and Saturday 7am - 10am.
  
  * SQL query:
  
  ```
  SELECT day_week, date_hour, ROUND(AVG(avg_avail_bikes) OVER (ORDER BY day_week, date_hour ROWS BETWEEN CURRENT ROW AND 2 FOLLOWING), 2) rolling_avg_3
    FROM (
        SELECT EXTRACT(DAYOFWEEK FROM time) day_week, EXTRACT(HOUR FROM time) date_hour, AVG(bikes_available) avg_avail_bikes
        FROM `bigquery-public-data.san_francisco.bikeshare_status` status
        JOIN `bigquery-public-data.san_francisco.bikeshare_stations` stations
        ON status.station_id = stations.station_id 
        GROUP BY 1, 2
        ORDER BY 1, 2)
    WHERE date_hour BETWEEN 6 AND 20
    ORDER BY rolling_avg_3 DESC
    LIMIT 20
  ```
- Question 7: How many trips are under 5 minutes, and what might these trips be?  

  * Answer: There are 178692 trips below 5 minutes. Below is a table of the top 10 start locations of these trips. All of these stations are right next to the Lyft HQ. Based on the discussion in class, these trips are most likely tests run by the Lyft staff, such as testing software of testing bikes. We do not want to consider these since they do not represent actual customer trips.
  
| start\_station\_name                            | num\_trips |
|-------------------------------------------------|------------|
| Townsend at 7th                                 | 11300      |
| San Francisco Caltrain 2 \(330 Townsend\)       | 9534       |
| 2nd at Folsom                                   | 9015       |
| Market at Sansome                               | 8135       |
| Temporary Transbay Terminal \(Howard at Beale\) | 6623       |
| 2nd at South Park                               | 6548       |
| Steuart at Market                               | 6297       |
| Beale at Market                                 | 5891       |
| San Francisco Caltrain \(Townsend at 4th\)      | 5674       |
| 2nd at Townsend                                 | 5175       |

  * SQL query:
  
  ```
  SELECT COUNT(*) FROM `bigquery-public-data.san_francisco.bikeshare_trips` 
  WHERE duration_sec < (5 * 60);
  
  SELECT start_station_name, COUNT(*) num_trips FROM `bigquery-public-data.san_francisco.bikeshare_trips` 
    WHERE duration_sec < (5 * 60)
    GROUP BY 1
    ORDER BY num_trips DESC
    LIMIT 10
  ```

- Question 8: Which bikes see the least usage?

  * Answer: Bikes that are used too often require more maintainence, so we want to try to keep underused bikes moving. This might be the case when for example when a bike in the middle of a rack is never touched because the sides are easier to access, and there are always extra bikes docked. We might considering offering discounts to individuals who take these bikes and use them in order to even out usage. Also, we limit our query to trips that last at least 5 minutes, and no more than 3 hours. To prevent outliers, we look at only trips that lasted at least 5 minutes (300 seconds) but no more than 3 hours (10800 seconds)
  
| bike\_number | uses |
|--------------|------|
| 876          | 4    |
| 697          | 9    |
| 34           | 17   |
| 323          | 18   |
| 565          | 19   |
| 460          | 22   |
| 224          | 22   |
| 476          | 24   |
| 316          | 25   |
| 237          | 34   |

  * SQL query:
  
  ```
  SELECT bike_number, COUNT(*) uses 
  FROM `bigquery-public-data.san_francisco.bikeshare_trips`
  WHERE duration_sec BETWEEN (5 * 60) AND (3 * 60 * 60)
  GROUP BY bike_number
  ORDER BY uses
  LIMIT 10
  ```

- Question 9: What is the average duration, standard deviation, and standard error of the duration of commuter trips? 

  * Answer: Average time: 10.14 minutes, and std is 3.92 minutes. However, since we have so many samples, the standard error is very low. Based on this, we already seem to have very quick turn-around on commuter trips. 
  
| ave\_commuter\_mins | std\_commuter\_mins | std\_error            |
|---------------------|---------------------|-----------------------|
| 10\.14              | 3\.92               | 0\.0066               |

  * SQL query:
  
  ```
  SELECT ROUND(AVG(duration_sec) / 60, 2) ave_commuter_mins, ROUND(STDDEV(duration_sec) /60, 2) std_commuter_mins, ROUND(STDDEV(duration_sec)/60 / SQRT(COUNT(*)), 5) std_error
  FROM `round-ring-276215.bikeshare_views.commuter_trips` 
  ```
  
- Question 10: How many trips conform to intended behavior of under 30mins, and how many above?? 

  * Answer: As discussed in class, there is a penalty for trips taking more than 30 minutes. We might consider offering a discount to individuals that stay within this limit. We could even increase the penalty for going above 30 minutes to be even steeper to make up for lost revenue. In the visualization step, we will look at the distribution of trip times. Furthermore, we look at only trips above 5mins and under 3hr to avoid extreme outliers.
  
| duration\_group                    | counts |
|------------------------------------|--------|
| more\_than\_30min\_less\_than\_3hr | 36763  |
| more\_than\_5min\_less\_than\_3hr  | 756232 |
| null                               | 190653 |

  * SQL query:
  
  ```
  SELECT CASE WHEN duration_sec > (30 * 60) AND duration_sec < (3 * 60 * 60) THEN 'more_than_30min_less_than_3hr'
            WHEN duration_sec <= (30 * 60) AND duration_sec > (5 * 60) THEN 'more_than_5min_less_than_3hr'
            END duration_group,
       COUNT(*) counts
  FROM `bigquery-public-data.san_francisco.bikeshare_trips`
  GROUP BY 1
  ```
  
- Question 11: What are the top 5 pairs of stations for which mean trip duration is the longest? 

  * Answer: To prevent outliers, look at only trips that lasted at least 5 minutes (300 seconds) but no more than 3 hours (10800 seconds). For pairs of stations, we do not want to consider which direction (namely going from A to B is the same pair as going from B to A). Related to the previous question, we might want to penalize users who greatly go above the rouns.

| station\_1                            | station\_2                           | average\_duration |
|---------------------------------------|--------------------------------------|-------------------|
| Rengstorff Avenue / California Street | Japantown                            | 10544\.0          |
| Mezes                                 | Charleston Park/ North Bayshore Area | 9517\.0           |
| San Antonio Caltrain Station          | Franklin at Maple                    | 9493\.0           |
| San Jose Civic Center                 | Mountain View Caltrain Station       | 8939\.25          |
| Paseo de San Antonio                  | Market at 4th                        | 7798\.0           |


  * SQL query:
  
  ```
  SELECT MIN(start_station_name) station_1, MAX(end_station_name) station_2, AVG(duration_sec) average_duration
  FROM `bigquery-public-data.san_francisco.bikeshare_trips`
  WHERE duration_sec BETWEEN 300 AND 10800
  AND start_station_id != end_station_id
  GROUP By LEAST(start_station_id, end_station_id ) || '_' || GREATEST(start_station_id, end_station_id
  ORDER BY average_duration DESC
  LIMIT 5
  ```
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

