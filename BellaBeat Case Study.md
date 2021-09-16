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
•	Some files are repeated information and would not be of use. dailySteps_merged , dailyCalories_merged is a summary of the same info from dailyActivty.
•	I only used files which have major representation of weight loss, calorie burnt and activity. This means inly selecting weightLogInfo_merged.csv,
dailyIntensities_merged.csv
•	dailyActivity_merged.csv

## 3.PROCESS ##
Using excel to clean data by separating string values in a cell. This would then allow BigQuery to import the table.
Using BiqQuery as the RDMS, I will clean the data from duplicates, null values and cleaning string functions.
Data cleaning process involves the following
<ul>
<li>Removing duplicates-Checking and removing duplicate rows and creating new tables in SQL on cleaned data</li>
<li>Checking for consistency-Ensuring the length of the ID were consistent</li>
<li>Converting data types -Using excel to separate data string of date and time in cells so that it can be imported into Big Query as a table.</li>
<li>Illogical Data- A few columns indicated zero activities such as steps. This is more somewhat illogical and could rather indicate that respondents were not wearing their Fitbit.</li>
</ul>
 <pre><code>
 --Double checking that all IDs in DailyActivity have the same number of characters--

SELECT LENGTH(Id)
FROM DailyActivity
--There are actually 33 distinct IDs contrary to the 30--
 </code></pre>



