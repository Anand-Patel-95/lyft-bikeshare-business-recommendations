# Project 1: Query Project

- The datasets are found on Google Cloud Platform (GCP), and detailed in Parts 1 & Parts 2.
  

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

- Likewise to make your recommendations, you will need to do data exploration, cleaning or filtering dirty data, etc. to come up with the final queries that will give you the supporting data for your recommendations. You can make any recommendations regarding the offers, including, but not limited to:
  * market offers differently to generate more revenue
  * remove offers that are not working
  * modify exising offers to generate more revenue
  * create new offers for hidden business opportunities you have found
  * etc.

---

## Part 1 & 2 - Explore the data with BigQuery on GCP


[Part 1 & 2 Analysis](part1-and-part2.md)

---

## Part 3 - Employ notebooks to synthesize query project results

### See my .ipynb file

[My Part 3 Notebook](Project_1.ipynb)

This notebook contains interactive maps generated using the plotly library's scatter_mapbox. These maps will not embed into GitHub's preview. The static map images for analysis have been added to the notebook, for your reference. 

For people interested in playing with the interactive maps for this data, you must have the ability to run the jupyter notebook code and have the plotly library installed. Instructions to install the necessary library are:

From a terminal:

```
pip install plotly
```
Then run the downloaded part 3 jupyter notebook.

---

## Overall Recommendations:

- What are the 5 most popular trips that you would call "commuter trips"?

    The top 5 commuter trips are presented in the following table below. A unique commuter trip is defined as a starting station and ending station. The num_trips gives the numbert of rides taken on this commuter trip, and the avg_duration_min column gives the average duration of this commuter trip.

| start_station_name                      | start_station_id | end_station_name                     | end_station_id | num_trips | avg_duration_min |
|-----------------------------------------|------------------|--------------------------------------|----------------|-----------|------------------|
| San Francisco Caltrain 2 (330 Townsend) | 69               | Townsend at 7th                      | 65             | 6265      | 4.33             |
| 2nd at Townsend                         | 61               | Harry Bridges Plaza (Ferry Building) | 50             | 5435      | 8.43             |
| Harry Bridges Plaza (Ferry Building)    | 50               | 2nd at Townsend                      | 61             | 5358      | 9.79             |
| Embarcadero at Sansome                  | 60               | Steuart at Market                    | 74             | 5198      | 6.86             |
| Steuart at Market                       | 74               | 2nd at Townsend                      | 61             | 5008      | 8.9              |

- What are your recommendations for offers (justify based on your findings)?
    - Holiday special offers: during November, December, and January to increase the number of subscribers taking trips. Customers can also purchase the holiday temporary pass to gain the same benefits. Holiday pass would offer generous riding incentives for subscribers and customers to increase the number of rides at the cost of duration, since we need to get ridership up.
    - Weekend subscription: cheaper than a normal subscription, may convert some customers to subscribers if we offer long trip durations and perks for the price of a small subscription fee. If our current ride limit is 30 minutes for customers, a weekend subscription could bump this up to 60 minutes at the cost of a reoccuring subscription fee. This would turn some of our customers into a more stable revenue generation source through subscriptions.
- Business Suggestion: 
    - Ensure that the customer ride limit is at most 60 minutes, since customer ride time is decreasing over the years.
    - Bike maintenence should be done between 11 am and 3 pm to ensure that these operations do not interfere with peak hours for subscribers. Maintenence can be done on bikes at the most popular commuter trip stations, prior to the commuter trip start times for these stations.
    - Reduce the subscriber weekday free ride time limit to 15 minutes during rush hour. Introduce an exponentially scaling charging model for trip duration over 15 minutes. Customer time limit can be even lower than this during the weekday rush hour times or have a faster charging model to keep subscriptions as the cost-effective option.
    - Ensure bike availability before commuting hours by shifting bikes from smaller stations to the commuter start stations prior to commuter times.
    - Expand commuter trips in SF by establishing more stations north around Embarcadero at Sansome. This station is relatively removed from others in this area, but still serves as a commuter hub. There could be a number of potential subscribers who would use our service to commute to workplaces north of Market Street since we do have many riders going up to the Embarcadero at Sansome station.
    - Expand new small stations and bikes to recreational parts of Eastern SF around Golden Gate State Park, Twin Peaks, or the Presidio to raise weekend ridership. The first two have good potential locations around the BART that can leverage the use case of users riding in from public transportation before using our bikes. This means that there would be infrastructure for weekend bike rides to more areas of interest, and even users from outside of SF could use our bikes after using public transport to get to these locations. Tourists going to these locations would have our bike service close by for use too. We could expect an increase in customer ridership on the weekend, followed by a possible increase in subscriber ridership given the new, interesting options.
