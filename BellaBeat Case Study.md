# Google-capstone-case-study
this is a test

# PROCESS
SELECT LENGTH(Id)
FROM DailyActivity
--Thre are actually 33 disticnt IDs contrary to the 30


--Running SQL script to check for duplicates
Select Id, ActivityDate, TotalSteps, Count(*)
From aboutsql.BellaBeat.Daily_Activity
group by id, ActivityDate, TotalSteps
Having Count(*) > 1

Results-> No duplicates

--Finding duplicate in sleeplog table
>SELECT Id,TotalTimeInBed,TotalMinutesAsleep 
FROM `aboutsql.BellaBeat.Sleep_day`
GROUP BY Id, SleepDay, TotalSleepRecords, TotalTimeInBed, TotalMinutesAsleep
HAVING Count(*) > 1
Found 3 duplicates
Hence ned to recfreate a new table minus the duplicate>

## CREATE OR REPLACE TABLE aboutsql.BellaBeat.Sleep_day2 ##
<p>This is a normal paragraph:</p>

<pre><code>This is a code block.
AS 
SELECT DISTINCT *
FROM `aboutsql.BellaBeat.Sleep_day`
New table returns 410 rows indicating 3 duplicate rows removed.</code></pre>

--Finding duplicate in sleeplog table
SELECT Id,TotalTimeInBed,TotalMinutesAsleep 
FROM `aboutsql.BellaBeat.Sleep_day`
GROUP BY Id, SleepDay, TotalSleepRecords, TotalTimeInBed, TotalMinutesAsleep
HAVING Count(*) > 1
Found 3 duplicates
Hence ned to recfreate a new table minus the duplicate

