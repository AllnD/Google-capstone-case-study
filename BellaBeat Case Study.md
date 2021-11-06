# Google-capstone-case-study
## Background ##

This is a case study for the Google analytics certificate which focuses on a health app tech manufacturer named Bellabeat https://bellabeat.com/. The purpose of this case study is to allow the analyst to analyse the provided datasets and utilize Google’s 6 step data analysis process to identify possible growth opportunities for the company to expand.

## 1.	ASK ##
This first phase involves defining the problem by asking the right question and thinking in a structured way 
with the identified main stakeholders in mind. 
#### <p> 1.1 Stakeholders <p> #### 
<ol>
<li>First is the founder and CEO Urska Srsen </li>
<li>The other party is the BellaBeat marketing analytics team.
</li>
</ol>

#### <p>1.2	Business Task<p> ####
To analyze smart device usage by consumers and use the insights gained to identify growth opportunities for the company. Based on the insights gained, recommendations are to be made to the marketing team to formulate a marketing strategy based upon the company’s products listed below:
  
  ![image](https://user-images.githubusercontent.com/88995922/133566224-fe95e7f2-390e-48a6-b8d3-ae41a2b7961a.png)

## 2. PREPARE ##
#### <p> 2.1 Data Source<p> ####
The datasets provided are available as public domain license listed as Kaggle: Fitbit Fitness Data stored as 18csv files. The information is made available via a survey by Amazon Mechanical Turk between 3rd to 5 December 2016. 30 Fitbit users consented to the submission and use of personal tracker data which includes detailed activity such as heart rate, daily activity, steps taken and sleep monitoring.

#### <p> 2.2.	Limitations <p> ####
This data is outdated as it was done in 2016. Also it only involves a small sampling size of 30 people which hardly represents the fitness community. In terms of ROCCC, it is fairly low. There is also a lack of information on water intact which might reduce the effectiveness of the recommendations on one of Bellabeat’s product which is spring. It was also evident that some data were missing such as weight log which only list 8 users, as not all users log their data. It is highly likely that the outcome of this analysis will be skewed as the data sets are of poor quality. 

#### <p> 2.3.	Key Tasks<p> ####
Preparing the data by downloading and cleaning it. Some deduction made of the data source:
<br> •	The data sources focused on a few themes that Fitbit users are all familiar with which is tracking sleep, physical activity such as steps and calories burned. <br> 
•	Some files are repeated information and would not be of use such as dailySteps_merged , dailyCalories_merged is a summary of the same info from dailyActivty.
I only used files which have major representation of sleep, BMI, calorie burnt and activity levels.

## 3. PROCESS ##
I will be using both excel and SQL to clean data by separating string values in a cell and filtering. This would also allow altering the schema in the CSV files to ensure BigQuery can import the table. Using BiqQuery as the RDMS, I have cleaned the data from duplicates, null values and converting string functions.
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
<li>Converting data types -Using excel to separate data string of date and time in cells so that it can be imported into Big Query as a table. This step is crucial to also ensure that tables can be linked in tableau and SQL with standardised primary keys</li>
<li>Identifying illogical Data- A few columns indicated zero activities such as steps. This is more somewhat illogical and could rather indicate that respondents were not wearing their Fitbit.</li>
<li> Agregating data 
 </li>
  
  </ul>
  
## 4. ANALYSE ##

After cleaning the data and preparing it ready for analysis, this phase involves analyzing the data with the goal of identifying patterns and conclusions. As mentioned in section 2, the hypothesis to test are factors that determine a person’s health that open up opportunities for BellaBeat to offer new services. Hence relationships between sleep vs activities vs calories burned in a seven day period is examined. A good BMI is between 18.5—24.9 (https://www.nhlbi.nih.gov/health/educational/lose_wt/BMI/bmicalc.htm)
<ul>
<li> There is positive correlation between activity level,steps taken and calories burnt </li>
<li> Of 24 distinct Ids on available participants, they do seem to have a healthy BMI range with a median of 24.9 and no obese levels, suggesting that survey participants have good weight management. However, 37.5 % is overweight.

  ![image](https://user-images.githubusercontent.com/88995922/133640291-e5b82bb8-faef-47d1-bac9-c686bddad52d.png)

</li>	
<li> The highest calories burnt are friday, saturday and sundays, which suggest participants are most active on non work days.This shows that they are also trying to be more active during off days. However, at the same time, they are sleeping and staying in bed more.</li>
<li> There is weak correlation on sleep and calories burned. Also, amount of sleep is not related to activity levels. The patterns reveals that work days are main factors impacting sleep, activity levels and hence calories burnt. This suggests participants sleep less and move less due to work commitments with an exception on wednesday.
 </li>
</ul>

## 5. SHARE ##
After analysing the data and deducing the conclusion based on the relationships and correlation of the various factors, the data will be presented in a visualised form to share it with stakeholders to communicate the findings. I have used both R and Tableau for this.

#### <p> 5.1 Loading used packages into R to create relationships based on graph. <p> ####

<li> Identifying relationships with scatter graph</li>
  
```{r ggplot for daily activities data}
> install.packages("tidyverse")
Installing package into ‘/cloud/lib/x86_64-pc-linux-gnu-library/4.1’
(as ‘lib’ is unspecified)
also installing the dependencies ‘colorspace’, ‘sys’, ‘bit’, ‘ps’, ‘base64enc’, ‘fastmap’, ‘rappdirs’, ‘rematch’, ‘farver’, ‘labeling’, ‘munsell’, ‘RColorBrewer’, ‘viridisLite’, ‘askpass’, ‘bit64’, ‘prettyunits’, ‘processx’, ‘evaluate’, ‘highr’, ‘yaml’, ‘xfun’, ‘htmltools’, ‘tinytex’, ‘jquerylib’, ‘backports’, ‘ellipsis’, ‘generics’, ‘glue’, ‘assertthat’, ‘blob’, ‘DBI’, ‘lifecycle’, ‘R6’, ‘tidyselect’, ‘vctrs’, ‘withr’, ‘data.table’, ‘gargle’, ‘uuid’, ‘cellranger’, ‘curl’, ‘ids’, ‘rematch2’, ‘digest’, ‘gtable’, ‘isoband’, ‘scales’, ‘cpp11’, ‘pkgconfig’, ‘mime’, ‘openssl’, ‘fansi’, ‘utf8’, ‘clipr’, ‘vroom’, ‘tzdb’, ‘Rcpp’, ‘progress’, ‘callr’, ‘fs’, ‘knitr’, ‘rmarkdown’, ‘selectr’, ‘stringi’, ‘broom’, ‘cli’, ‘crayon’, ‘dbplyr’, ‘dplyr’, ‘dtplyr’, ‘forcats’, ‘googledrive’, ‘googlesheets4’, ‘ggplot2’, ‘haven’, ‘hms’, ‘httr’, ‘jsonlite’, ‘lubridate’, ‘magrittr’, ‘modelr’, ‘pillar’, ‘purrr’, ‘readr’, ‘readxl’, ‘reprex’, ‘rlang’, ‘rstudioapi’, ‘rvest’, ‘stringr’, ‘tibble’, ‘tidyr’, ‘xml2’
library(ggplot2)

> library(readr)
> daily <- read_csv("daily/daily.csv")
```

```{r ggplot for various relationships data visualzation}
> ggplot(data=daily)+geom_point(mapping = aes(x=FairlyActiveMinutes,y=Calories,color=TotalSteps))

```
![L0](https://user-images.githubusercontent.com/88995922/140601295-b61fb5f7-2759-4a2d-b746-96fac752389f.png)
![L1](https://user-images.githubusercontent.com/88995922/140601296-752ce494-fa3d-442a-92de-ccf9de3b6ac6.png)
![L2](https://user-images.githubusercontent.com/88995922/140601300-554abdc2-477b-4da5-add8-96e5ee6a786e.png)
![L3](https://user-images.githubusercontent.com/88995922/140601302-b4ef3c9c-f98a-40f1-8779-de362b4f5d72.png)


#### <p>5.2 Tableau dashboard <p> ####
<ul>
<li> There is positive correlation between activity levels and calories burnt. The higher the activity level, the more density and increase in calories burnt. Not surprising, there is negative correlation on sedentary lifestyles with calories burnt.
  
![calories](https://user-images.githubusercontent.com/88995922/140600840-04637a39-779e-4c6c-aa13-f782e9533024.png)

  
</li>
  
<li> Analysis on the correlation of activities , calories and sleep in a 7 day period.
  
![image](https://user-images.githubusercontent.com/88995922/133636951-3b55ebdc-29e5-4f3a-a981-9ee9faefd3a4.png)
  
 </li> </ul>


  
## 6.	ACT ##
Based on the conclusions drawn from our analysis , the proposed recommendations should focus on the gaps that can help improve the tracking of variables related to user health. With the stakeholders interest in consideration which involves cost saving and optimisation of resource, the following areas are proposed for Bella marketing to further explore.
#### <p> 6.1 Recommendations <p> ####
<ol>
<li> Enhance the exisiting products to include BMI/weight tracking since it is an obvious gap. It seems that participants have trouble or finds it inconvenient logging in this data.</li>
<li> Create a calory and activity plan to better make use of off days. The activity levels and calories burnt can be much higher. Then create a tracker or reminder to hit some form of target activity level based on that plan especially on low activity days on monday, tuesday and thursday.</li>

</ol>

### =======================THE END===========================





 





