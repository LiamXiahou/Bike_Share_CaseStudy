# Bike_Share_CaseStudy
This is for capstone of Bike Share in Google Data Analytics Certificate. 
This case study will follow 6 phases of data analysis which are **ASK, PREPARE, PROCESS, ANALYZE, SHARE, ACT. **
## ASK
What is the business goal ? 
- To convert casual riders to membership riders based on understand how casual riders and annual members use Cyclistic bikes differently. (Why? --> Cyclistic’s finance analysts have concluded that annual members are much more profitable than casual riders.)
- why casual riders would buy a membership, and how digital media could affect their marketing tactics ?
Audience : Marketing team, Executive team
## Prepare
- where is data located ? -->[Raw data source](https://divvy-tripdata.s3.amazonaws.com/index.html)
- How is data orgnaized ? --> by month, quarter or year. 

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
    **It took ~30s to stack 23 tables into 8929238 rows. --> **\
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
    ![image](https://user-images.githubusercontent.com/102010540/160196585-dfabe02f-b28a-45e0-893b-3d1cd77b72d4.png)\


- Are there issues with bias or credibility in this data? Does your data ROCCC? --> should not be becasue this is the only data from the case study 😂
- How are you addressing licensing, privacy, security, and accessibility? --> no idea
- How did you verify the data’s integrity? --> will check later. However, the data is huge . 
- How does it help you answer your question? --> TBD
- Are there any problems with the data? --> TBD
