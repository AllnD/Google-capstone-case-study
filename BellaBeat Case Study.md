# Google-capstone-case-study
## Background ##

This is a case study for the Google analytics certificate which focuses on a high-tech manufacturer named Bellabeat. The purpose of this case study is to allow the analyst to analyse the provided datasets and utilize Google’s 6 step data analysis process to identify possible growth opportunities for the company to expand.

## 1.	ASK ##
This first phase involves defining the problem by asking the right question and thinking in a structured way 
with the identified main stakeholders in mind. 
<p> 1.1 Stakeholders <p>
<ol>
<li>First is the founder and CEO Urska Srsen </li>
<li>The other party is the BellaBeat marketing analytics team.
</li>
</ol>
<p>1.2	Business Task<p>
To analyze smart device usage by consumers and use the insights gained to identify growth opportunities for the company. Based on the insights gained, recommendations are to be made to the marketing team to formulate a marketing strategy based upon the company’s products listed below:
  
  ![image](https://user-images.githubusercontent.com/88995922/133566224-fe95e7f2-390e-48a6-b8d3-ae41a2b7961a.png)

## 2. PREPARE ##
<p> 2.1 Data Source<p>
The datasets provided are available as public domain license listed as Kaggle: Fitbit Fitness Data stored as 18csv files. The information is made available via a survey by Amazon Mechanical Turk between 3rd to 5 December 2016. 30 Fitbit users consented to the submission and use of personal tracker data which includes detailed activity such as heart rate, daily activity, steps taken and sleep monitoring.

<p> 2.2.	Limitations <p>
This data is outdated as it was done in 2016. Also it only involves a small sampling size of 30 people which hardly represents the fitness community. In terms of ROCCC, it is fairly low. There is also a lack of information on water intact which might reduce the effectiveness of the recommendations on one of Bellabeat’s product which is spring. It was also evident that some data were missing such as weight log which only list 8 users, as not all users log their data. It is highly likely that the outcome of this analysis will be skewed as the data sets are of poor quality. 

<p> 2.3.	Key Tasks<p>
Preparing the data by downloading and cleaning it. Some deduction made of the data source:
<br> •	The data sources focused on a few themes that Fitbit users are all familiar with which is tracking sleep, physical activity such as steps and calories burned. <br> 
•	Some files are repeated information and would not be of use such as dailySteps_merged , dailyCalories_merged is a summary of the same info from dailyActivty.
I only used files which have major representation of weight loss, calorie burnt and activity. This means inly selecting weightLogInfo_merged.csv,
dailyIntensities_merged.csv and dailyActivity_merged.csv

## 3.PROCESS ##
Using excel to clean data by separating string values in a cell. This would then allow BigQuery to import the table.
Using BiqQuery as the RDMS, I will clean the data from duplicates, null values and cleaning string functions.
Data cleaning process involves the following
<ul>
<li> Removing duplicates-Checking and removing duplicate rows and creating new tables in SQL on cleaned data
<pre><code>
--Running SQL script to check for duplicates--
Select Id, ActivityDate, TotalSteps, Count(*)
From aboutsql.BellaBeat.Daily_Activity
group by id, ActivityDate, TotalSteps
Having Count(*) > 1
-- No duplicates-- </code></pre>

<pre><code>--Finding duplicate in sleepDay table

SELECT Id,TotalTimeInBed,TotalMinutesAsleep 
FROM `aboutsql. BellaBeat.Sleep_day`
GROUP BY Id, SleepDay, TotalSleepRecords, TotalTimeInBed, TotalMinutesAsleep
HAVING Count(*) > 1

--Found 3 duplicates, Hence need to recreate a new table minus the duplicate--

CREATE OR REPLACE TABLE aboutsql.BellaBeat.Sleep_day2
AS 
SELECT DISTINCT *
FROM `aboutsql.BellaBeat.Sleep_day`
--New table returns 410 rows indicating 3 duplicate rows removed--
</code></pre>

 </li>
<li>Checking for consistency-Ensuring the length of the ID were consistent
<pre><code>
 --Double checking that all IDs in DailyActivity have the same number of characters--

SELECT LENGTH(Id)
FROM DailyActivity
--There are actually 33 distinct IDs contrary to the 30--
 </code></pre>
</li>
<li>Converting data types -Using excel to separate data string of date and time in cells so that it can be imported into Big Query as a table.</li>
<li>Identifying illogical Data- A few columns indicated zero activities such as steps. This is more somewhat illogical and could rather indicate that respondents were not wearing their Fitbit.</li>
<li> Agregating data 
 </li>
  
  </ul>
  
## 4. ANALYSE ##

After cleaning the data and preparing it ready for analysis, this phase involves analyzing the data with the goal of identifying patterns and conclusions, and trying to make predictions and conclusions. As mentioned in section 2, the hypothesis to test are factors that determine a person’s health that open up opportunities for BellaBeat to offer new services. Hence relationships between sleep vs stress vs activities vs calories burned. 
https://www.nhlbi.nih.gov/health/educational/lose_wt/BMI/bmicalc.htm defintiion of good BMI
aggregate data here

<ul>
<li> There is positive correlation between steps taken and calories burnt </li>
<li> Activity levels have a positive correlation with calories burned </li>	
<li> Sedentary lifestyles and calories burnt have a negative correlation. Hence,</li>
<li> From the joined tables, the users data shows that the ones with highest activity , steps and sleep shows the best weight and calories burned.
 </li>
</ul>

## 5. SHARE ##
Loading into R to create relationships based on graph. 


## 6.	ACT ##
Based on the conclusions drawn from our analysis , the proposed recommendations should focus on the available variables that constitute health improvements. Users that move more, have better sleep and take more steps have better weight. Hence, the following areas are proposed for Bella marketing.
<p> 6.1 Recommendations <p>
<ol>
<li> Enhance the products to track and sound alarm for sedentary or inactive times.</li>
<li> Participants didn’t burn more calories on days that they worked vs. days they didn’t. Create a tracker and reminder on this e</li>
<li> Create an app in the products that can predict sleep time and stress.</li>
</ol>

### =====================THE END===========================	





 





