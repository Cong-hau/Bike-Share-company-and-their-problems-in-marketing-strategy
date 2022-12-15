# Bike Share company and their problems in marketing strategy
I am eager to introduce and appreciate when you travel on my case study. In this case study, I am a junior data analyst work for a bike-share company, Cyclistic
## Content summary
+ Scenario
+ Ask
+ Prepare
+ Process
+ Analyze
+ Share
## Scenario
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
