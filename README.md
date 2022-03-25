# Bike_Share_CaseStudy
This is for capstone of Bike Share in Google Data Analytics Certificate. 
This case study will follow 6 phases of data analysis which are **ASK, PREPARE, PROCESS, ANALYZE, SHARE, ACT. **
## ASK
### What is the business goal ? 
- To convert casual riders to membership riders based on understand how casual riders and annual members use Cyclistic bikes differently. (Why? --> Cyclistic’s finance analysts have concluded that annual members are much more profitable than casual riders.)
- why casual riders would buy a membership, and how digital media could affect their marketing tactics ?
Audience : Marketing team, Executive team
## Prepare
### where is data located ? -->[Raw data source](https://divvy-tripdata.s3.amazonaws.com/index.html)
### How is data orgnaized ? --> by month, quarter or year. 

**How to scrape all csv files which are inside zip file with script ? -->** *unknown, currently just manually downloaded files and extract them individually*\
**How to upload all tables (csv files) to DB ? --> \
   --> ~~tried MS Access which does not support csv~~\
    DBeaver works well to load all files from 202004 to 202202 which tooks couples of mininutes. 👍\
    ![image](https://user-images.githubusercontent.com/102010540/159379398-216dc8df-cab1-444b-8fbb-553ee5522f7d.png)\
Does need stack all tables ? stacking will be beneficial for data cleaning**\
   --> Used *UNION ALL* to consolidate all files into one file. 👍\
   ![image](https://user-images.githubusercontent.com/102010540/159621614-394d9673-d3de-4631-8862-f6b8b3ad10cb.png)\
    -->However, there is error message indicating data type of tables are not the same which helps on data cleaning. 🏖️ \
    ![image](https://user-images.githubusercontent.com/102010540/159621703-3559b137-1d12-4231-86ed-e7f867181204.png)\
    How to find out which table(s) having wrong data type of field(s) ?\
    Could not figure out how to highlight error line. the position error does not help at all. 🚩\
    Execute selected syntax by *selecting + ctrl+Enter* and find out talbe 202012 onward data type of start_staion_id and end_station_id are var while the others are int4. 👍\
    ![image](https://user-images.githubusercontent.com/102010540/159625507-52ca45fc-cc04-4f20-95a1-8c4676aec008.png)\
    Correct it by changing the 2 fields to var data type in DBeaver:\
    ![image](https://user-images.githubusercontent.com/102010540/159628757-eb103b08-2f71-4f9a-b7eb-b0533aecc614.png)\
    Have to update data type of table one by one .It did not work while trying to update all tables using SQL syntax. 🚩 \   
    ```
    {
    ALTER TABLE "Bike_Share"."202011_divvy_tripdata_csv" ALTER COLUMN start_station_id TYPE varchar(1024) USING start_station_id::varchar;
    ALTER TABLE "Bike_Share"."202011_divvy_tripdata_csv" ALTER COLUMN end_station_id TYPE varchar(1024) USING end_station_id::varchar;
    }
    ```\
    **However, it worked successfully and UNION completed after updating data type of the 2 fields**  👍\
    **It took ~30s to stack 23 tables into 8929238 rows. --> **
    \
    ```
    {
    select * 
into Bike_Share_202004_202202
from
(
select *
from "Bike_Share"."202004_divvy_tripdata_csv"
union all
select *
from "Bike_Share"."202005_divvy_tripdata_csv"
union all
select *
from "Bike_Share"."202006_divvy_tripdata_csv"
union all
select *
from "Bike_Share"."202007_divvy_tripdata_csv"
union all
select *
from "Bike_Share"."202008_divvy_tripdata_csv"
union all
select *
from "Bike_Share"."202009_divvy_tripdata_csv"
union all
select *
from "Bike_Share"."202010_divvy_tripdata_csv"
union all
select *
from "Bike_Share"."202011_divvy_tripdata_csv"
union all
select *
from "Bike_Share"."202012_divvy_tripdata_csv"
union all
select *
from "Bike_Share"."202101_divvy_tripdata_csv"
union all
select *
from "Bike_Share"."202102_divvy_tripdata_csv"
union all
select *
from "Bike_Share"."202103_divvy_tripdata_csv"
union all
select *
from "Bike_Share"."202104_divvy_tripdata_csv"
union all
select *
from "Bike_Share"."202105_divvy_tripdata_csv"
union all
select *
from "Bike_Share"."202106_divvy_tripdata_csv"
union all
select *
from "Bike_Share"."202107_divvy_tripdata_csv"
union all
select *
from "Bike_Share"."202108_divvy_tripdata_csv"
union all
select *
from "Bike_Share"."202109_divvy_tripdata_csv"
union all
select *
from "Bike_Share"."202110_divvy_tripdata_csv"
union all
select *
from "Bike_Share"."202111_divvy_tripdata_csv"
union all
select *
from "Bike_Share"."202112_divvy_tripdata_csv"
union all
select *
from "Bike_Share"."202201_divvy_tripdata_csv"
union all
select *
from "Bike_Share"."202202_divvy_tripdata_csv"
) Bike_Share_All
    }
    ```
    \
    Output:
    ![image](https://user-images.githubusercontent.com/102010540/160196585-dfabe02f-b28a-45e0-893b-3d1cd77b72d4.png)
\
With manually refresh tables in left panel, Unioned table is added to under table --> \
**We will only need to deal with one table instead of 20+ tables **
👍
\
![image](https://user-images.githubusercontent.com/102010540/160200151-0f8a9bb0-f81e-4c5a-987f-a221c4e4ecdb.png)

### Are there issues with bias or credibility in this data? Does your data ROCCC? --> should not be becasue this is the only data from the case study 😂
### How are you addressing licensing, privacy, security, and accessibility? --> just trust data what Coursera provides. 
### How did you verify the data’s integrity?  
☑️ Run some summary report to check DI. 
```
{
select rideable_type, member_casual ,count(*)
from bike_share_all 
group by rideable_type, member_casual  
}
```
Output as belw does not show DI issue on fields of rideable_type, member_casual \
![image](https://user-images.githubusercontent.com/102010540/160202475-aa9edd89-fa35-4d84-925b-66c054faaef2.png)
\
☑️ Check DI of station name
```
{
select distinct start_station_name 
from bike_share_all bsa 
order by
start_station_name 
}
```
\
Output file to excel from which found some errors as some shown as in below highlighted in red: \
1. leading and trailing spaces from a string.
2. questionalbe names with * or temp in the end. 
3. missing ~850 names in each of Start and End station.  
![image](https://user-images.githubusercontent.com/102010540/160207805-0850b8ff-7b1d-41d8-8b14-9d0c84ec18fa.png)
\
**Solution**
1. Keep in mind to use trim() function to pull station name for analysis. 
```
{
select trim(start_station_name),trim(end_station_name)
from bike_share_all bsa 
}
```
\
2. In real case, will have to verfiy if those station with * or temp are the same as ones without such tails. 
3. Failed to pull records of having blank of either start or end of station. DBeaver does not support Isempty nor Isblank. Isnull does not fit into this enquiry. 
```
{
select distinct trim(start_station_name) as trim_start_station_name,trim(end_station_name) as trim_end_station_name,start_station_id ,end_station_id 
from bike_share_all bsa 
where start_station_name isempty  ;
}
```
\
Output error -> 👎 ![image](https://user-images.githubusercontent.com/102010540/160211202-8daa8845-289d-47e4-b8f3-de6cc23552d0.png)
\

### How does it help you answer your question? --> TBD
### Are there any problems with the data? --> TBD
