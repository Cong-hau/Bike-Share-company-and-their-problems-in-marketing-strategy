# Bike Share company and their problems in marketing strategy
I am eager to introduce and appreciate when you travel on my case study. In this case study, I am a junior data analyst work for a bike-share company, Cyclistic
================
Hau Lee
2022-05-20
## Content summary
+ Scenario
+ Ask
+ Prepare
+ Process
+ Analyze
+ Share
# Scenario
About the company: Cyclistic launched a successful bike-share offering, the bikes can be unlocked from one station and returned to any other station in the system anytime. Cyclistic supply 5,824 bicycles, a network of 692 stations across Chicago and 3 options of sharing: single-ride passes, full-day passes and annual memberships. Customers who purchase single-ride or full-data passes are referred to as casual riders and who purchase annual memberships are Cyclistic members.  
+ Characters and teams: Cyclistic: a bike-share program. Lily Moreno: the director of marketing and my manager. Cyclistic marketing analytics team: a team of data analysts who are responsible for collecting, analyzing, and reporting data. Cysclistic executive team: the detail-oriented executive team will decide whether to approve the recommended marketing program.  
+ The problem: Cyclistic’s marketing strategy relied on building general awareness and appealing to broad consumer segments, however Cyclistic’s finance analysts have concluded that annual members are much more profitable than casual riders. So the marketing strategy is less effective and do not have significant impact in growth rates, revenue.  
+ To solve the problem, I relied on 6 stages of Google Data Analysis Process: Ask, Prepare, Process, Analyze, Share, Act.  
## Ask
Moreno has set a clear goal: Design marketing strategies aimed at converting casual riders into annual members. To get the goal, synonym answering 3 sub questions:
+	How do annual members and casual riders use Cyclistic bikes differently? 
+	Why would casual riders buy Cyclistic annual memberships?
+ How can Cyclistic use digital media to influence casual riders to become members?
Moreno has assigned first question to me. Identify Moreno’s expectations: 
1.	A clear statement of the business task
2.	A description of all data sources used
3.	Documentation of any cleaning or manipulation of data
4.	A summary of your analysis
5.	Supporting visualizations and key findings
6.	Your top three recommendations based on your analysis.

Clarify effects of my insights (form 1st question) drive business decisions: to figure out characteristics of 2 type of members, make sense of differences, based on that to find key factors affect to the converting casual into annual member or main causes, critical obstacle of non-upgrade decision, such as: low demand, don not have stations which they want, etc. Based on these insights will support to answer the rest of questions and make suitable marketing strategy.

**Statement of the business task:** Design marketing strategies aimed at converting casual riders into annual members.
## Prepare
To figure out the variance of behaviors between casual rides and annual members, I used Cyclistic’s historical trip data within previous 12 months. The data is internal data, archive here, each files correspond each monthly period. The data is organized by spreadsheet, a row represent for a bike trip and columns represent for objects of trip, include:	
| Objects | Description | Data type |
|--------|----------| ----------| 
| ride_id | Ride’s ID | nvarchar |  
| rideable_type | Type of bike |nvarchar |   
| started_at | Started time of trip | datetime |
| ended_at | Ended time of trip |datetime | 
| start_station_name | Started station’s name | nvarchar|
| start_station_id | Started station’s ID | nvarchar| 
| end_station_name | Ended station’s name | nvarchar| 
| end_station_id | Ended station’s ID | nvarchar| 
|start_lat|Latitude of started station|Float |
|start_lng|Longitude of started station|Float |
|end_lat|Latitude of ended station|Float |
|end_lng|Longitude of ended station|Float |
|member_casual|Annual member or casual rider|nvarchar |

Because of using the whole of data to analyze, I mean I did not create sample from population, so I can ensure that it do not make sampling bias. Additionally, the data only include records about all trip, do not have any interpretations, assessments or felling of collector so it does not create observer bias, interpretation bias, confirmation bias. Conclude that the data is credibility, non-tendency to skews insights in a certain direction.
+ To assess the data sources is good or not, I used ROCCC standard, ROCCC is acronym of reliable, original, comprehensive, current, and cited. 

| Standard | Description | Good or Bad |
|--------|----------| ----------| 
| R – Reliable | Accurate, complete, unbiased | Good |
| O – Original | Can locate the original resource | Good |
| C – Comprehensive | Contain critical information to answer business question | Good |
| C – Current | Don’t out of day, relevant | Good |
| C – Cited |  | Good |

+ The data has been made available by Motivate International Inc. under this [license](https://ride.divvybikes.com/data-license-agreement). The data is privacy and prohibit from any using rider’s personally identifiable information.

To summarize, all data sources used: The data sources is internal, privacy and made by Motivate International Inc. The data is organized by spreadsheet, a row represent for a bike trip and columns represent for objects of trip, include: ride id, type of bike, type of service, when and where of starting, ending.
## Process
After I downloaded the 12 previous data files, from September 2021 to October 2022, I made a quickly reviewing and realize that the size of dataset is large to spreadsheet tools, so I used Microsoft SQL Server on localhost server to clean and analyze data for high effectiveness. I did not use BigQuery because my BigQuery account is free, so it is limited a lot of ability, such as: creating, updating function and also I want to improve MS SQL Server skills.
+ Import data into SQL Server Management Studio.
![image](https://user-images.githubusercontent.com/30770825/207770921-0f5ff36c-6fdf-42c4-8744-eb8b852b3a6d.png)
+ Or use Azure Data Studio which is focused on customer experience, user-centric to simply my query process.
![image](https://user-images.githubusercontent.com/30770825/207771025-71f5d486-d22a-4449-8ce6-e10f3d13f212.png)

Because of downloading, importing data into SSMS, there might have errors, so these activities created variance between original data and downloaded data or missing a part of information or data type is not consistent with original data. So before I dig into analyze data to get insights, I have to conduct 2 pre-cleaning activities: review data integrity and check insufficient data to ensure data is accurate, consistent, completeness, trustworthiness and sufficient to answer the business questions.

**Review data integrity:** I reviewed data integrity of each file from September 2021 to October 2022, excepted objects: start_station_name, start_station_id, end_station_name, end_station_id because these are not rules are applied themselves and are not key to answer the question: “How do annual members and casual riders use Cyclistic bikes differently?”. 
| Objects | Rules | Data Integrity |
| -------- | ------------- | ------------- |
| ride_id | Unique, not NULL | Good |
| rideable_type | 3 Specific values: electric-bike, classic-bike, docked-bike | Good |
| started_at, ended_at | Not NULL. started_at is happened before ended_at | Bad |
| start_lat, end_lat | Positive float | Good |
| start_lng, eng_lng| Negative float | Good |
| member_casual | Only contain 2 values: member or casual | Good |

**SQL Script:**

``` sql
/****** Script for Rewview Integrity from SSMS  ******/
--- Check ride_id unique
Select count(ride_id) - count(distinct(ride_id)) AS duplicate
From [202111-divvy-tripdata]

---Check ride_id not NULL
Select *
FROM [202111-divvy-tripdata]
WHERE ride_id is NULL

---Check values
Select distinct(rideable_type)
FROM [202111-divvy-tripdata]

---Check started_at, ended_at not NULL
Select *
FROM [202111-divvy-tripdata]
WHERE started_at is Null or ended_at is NULL

---Check started_at is happened before ended_at or not
Select *
From [202111-divvy-tripdata]
WHERE started_at > ended_at

---Check longitude and latitude
SELECT *
FROM [202111-divvy-tripdata]
WHERE start_lng > 0 or end_lng > 0 or start_lat < 0 or end_lat < 0

---Check value of memeber_casual
SELECT distinct(member_casual)
FROM [bike_share].[dbo].[202111-divvy-tripdata]
```

Most of objects are good data integrity, but there are rows which started time is happened after ended time, it is no logical, the causes might be from human error or malware. However, number of error row is tiny than total trip, so I decided to skip all error records by deleting from databases.

 | File | Inaccuracy Row | Total row |
 | ---- | ---- | ---- |
 | 202111 | 53 | 359978 |
 | 202203 | 2 | 284042 |
 | 202205 | 1 | 634858 |
 | 202206 | 12 | 769204 |
 | 202207 | 16 | 823488 |
 | 202208 | 15 | 785932 |
 | 202209 | 9 | 701339 |
 | 202210 | 4 | 558685 |
 
To ready to analyze, I revised data again to ensure that data is clean or dirty. I created a checklist for that and documented cleaning log if necessary.
+ Check date formats
+ Check meaningful of name column
+ Check spelling
+ Check string formats
+ Correct data
+ Fill Incomplete important
+ Remove duplicate

**Documentation of change log:**
|Date Create| User Create | Summary of change | Approve |
|---| --- | --- |--- |
|13/12/2022| Hau | Remove rows have started time is happened after ended time | Moreno |

## Analyze
I added a new column called “day_of_week”, it contains the day of week that each ride started. Format as an integer number, nothing that 1 = Sunday and 7 = Saturday.
```sql
ALTER TABLE [202202-divvy-tripdata] ADD day_of_week int
UPDATE [202202-divvy-tripdata] SET day_of_week = DATEPART(WEEKDAY, started_at)
```
To figure out behaviors of membership and casual rider, I created another column named “ride_length” to calculate the length of each ride, measured by minute. Format as an decimal number instead of time type, because in Microsoft SQL Server, time type is limited to 23:59:59, so make inaccurate data when some rides are over more than 1 day, the other reason is I would like to calculate mean of time.
```sql
ALTER TABLE [202202-divvy-tripdata] ADD ride_length decimal(18,0)
UPDATE [202202-divvy-tripdata] 
SET ride_length = DATEDIFF(MINUTE, started_at, ended_at)
```
After dig into analyzing, I reviewed dataset and wonder that there are abnormal points or not? 
```sql
SELECT TOP 1000 *, ride_length/(60*24) as day
FROM [202201-divvy-tripdata]
ORDER BY DAY DESC
```
Here is a snipshot of abnormal points. The result talks about top 1000 data of ride_length columns in 202201-divvy-tripdata files and for easy comparison, I converted ride_length from minute to day. Look at the data, there are a tiny group of length of each ride which abnormally over longer than rest of all (most of ride_length is smaller than 2 days) and the abnormal data is recorded in all months of databases. The causes of abnormal data might from bike management system, the hypothesis is theft or user want not to go to the station for returning but let the bike in wherever they want because all abnormal data made by casual riders.

![image](https://user-images.githubusercontent.com/30770825/207774392-6920b353-fd45-4579-a55f-f7936d514313.png)

I sat back and compare with the total trip made by casual riders, and I realized the abnormal data might affect to insights in a wrong view and were not represented common behaviors of riders. Therefore, I skipped all abnormal by filtering with clause `sql WHERE ride_length < 2*24*60`

Let’s conduct descriptive analysis
```sql
---Calculate mean, max, sum, trip
SELECT AVG(ride_length) as mean
	, MAX(ride_length) as max
	, SUM(ride_length) as sum
	, count(ride_id) as trip
	, member_casual
FROM [202111-divvy-tripdata]
WHERE ride_length < 2*24*60
GROUP BY member_casual
---Calculate most occuring value in dataset
SELECT count(day_of_week) as mode
	, day_of_week
FROM [202111-divvy-tripdata]
GROUP BY day_of_week
ORDER BY MODE DESC
---Calculate total of trip
SELECT count(*) as total_trip
FROM [202111-divvy-tripdata]
```
I combined all data into one table and make an overall view and made sense of average of ride length, max of ride length, number of rides by each month, by day of week and by membership or casual rider.
```sql
---Combine all tables into one
SELECT *  INTO [2111-2210-divvy-tripdata]
FROM [202111-divvy-tripdata]
UNION ALL
Select * From [202112-divvy-tripdata]
UNION ALL
Select * From [202201-divvy-tripdata]
UNION ALL
Select * From [202202-divvy-tripdata]
UNION ALL
Select * From [202203-divvy-tripdata]
UNION ALL
Select * From [202204-divvy-tripdata]
UNION ALL
Select * From [202205-divvy-tripdata]
UNION ALL
Select * From [202206-divvy-tripdata]
UNION ALL
Select * From [202207-divvy-tripdata]
UNION ALL
Select * From [202208-divvy-tripdata]
UNION ALL
Select * From [202209-divvy-tripdata]
UNION ALL
Select * From [202210-divvy-tripdata]
---Desciptive analysis
SELECT month(started_at) as month
	, day_of_week
	, member_casual
	, AVG(ride_length) as mean_ride_length
	, MAX(ride_length) as max_ride_length
	, SUM(ride_length) as sum_ride_length
	, count(ride_id) as number_rides
FROM [2111-2210-divvy-tripdata]
GROUP BY month(started_at), day_of_week, member_casual
ORDER BY month(started_at), number_rides DESC
```
**Note:** to simply the syntax, I did not show year in the querying results. Values of the month-column contain: 1,2,3,4,5,6,7,8,9,10 is represented January 2022, February 2022,… so on and 11, 12 is noted November 2021, December 2021.

To make the trends, relationships, or insights, I downloaded all findings and used Pivot Excel to visualize the trends.

![image](https://user-images.githubusercontent.com/30770825/207775312-9a36f565-be11-4796-a909-5da8a1112678.png)
Here is the average of ride length for members and casual riders by monthly. The chart talk about average rented length of casual riders is always greater, appropriate 2 times than members. 

![image](https://user-images.githubusercontent.com/30770825/207775419-4cbec50b-5215-4a41-a5af-7fa1cd1dde29.png)
The number of rides by length for members and casual riders by monthly, members conduct more rides than casual riders. It leads to members are much more profitability than casual riders according to financial analysis, although average of ride length of members is less than casual riders. The other finding is number of riders increase towards on autumn and very low on winter when it is so cold to ride a bike for leisure or travel to work.

![image](https://user-images.githubusercontent.com/30770825/207775517-e3b8a62e-88fa-4bc5-8f28-0c8f0697400d.png)
Next, the chart is average of ride length by day of week, is summarized by 12 previous month data. The change of ride length is equal both members and casual riders. It is higher on weekends than workday from Monday to Friday. 

![image](https://user-images.githubusercontent.com/30770825/207775583-dcfade52-7bf5-4674-9aed-3dd42d15bb3b.png)
The final chart is number of rides by day of week, is summarized by 12 previous month data. The chart contains 2 important insights, firstly on workday from Monday to Friday, number of rides for members always higher than casual riders, this is quite familiar because of the trend from the rides by member, casual, time chart. The first finding led to a hypothesis that: annual members have planned to use the bike service for fixed demands, such as commute to work. 

Secondly, on the weekend, it is abnormal as number of rides of casual rides is equal or even larger than members, this led to casual riders often use the bike service for random activities which in their mind suddenly and are not any intend before, for instance: leisure, entertainment, enjoy the fresh air.

**All above insights lead me to a summary of analysis:**
1.	Average of ride length for casual rides is higher than members. 
2.	Number of rides for members is higher than casual riders
3.	People tend to use more bike service on comfort seasons.
4.	Average of ride length for both is higher on weekend than workday.
5.	Annual members make more rides on workday than weekend.
6.	Casual riders make more rides on weekend than workday

## Share
Every good presentation always talks about compelling story of data, so it includes 3 parts
1.	Context: The marketing strategy of Cyclistic is less effective and do not have significant impact in growth rates, revenue. So, marketing manager want to design marketing strategies aimed at improving effectiveness by converting casual riders into annual members. Firstly, the manager would like to figure out: How do annual members and casual riders use Cyclistic bikes differently? 
2.	Key Insights: Average of ride length for casual rides is higher than members. Number of rides for members is higher than casual riders. Annual members make more rides on workday than weekend. Casual riders make more rides on weekend than workday.
3.	Conclusions

Based on 3 parts of data’s story, I used Power Point to prepare presentation to show the analysis results to my manager and stakeholders. After, I putted myself into audience’s shoes to recheck, evaluate my slide, here the checklist to do this task:

+ Include a good title and subtitle that describe what you’re about to present?
+ Include the date of your presentation or the date when your slideshow was last updated?
+ Use a font size that lets the audience easily read your slides?
+ Showcase what business metrics you used?
+ Include effective visuals (like charts and graphs)?

## Conclusion
I appreciate to say thanks when you travel over long time to here. I relied on 6 stages of Google Data Analysis Process: Ask, Prepare, Process, Analyze, Share, Act to solve the problem from bike share company meet low-effecitve marketing strategy. I hope that the case study is useful documentation for you. 

See ya!
