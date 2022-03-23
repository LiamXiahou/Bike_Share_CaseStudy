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
Does it need merge all tables ? merging will be beneficial for data cleaning**\
   --> Used *UNION ALL* to consolidate all files into one file. 👍\
   ![image](https://user-images.githubusercontent.com/102010540/159621614-394d9673-d3de-4631-8862-f6b8b3ad10cb.png)\
    -->However, there is error message indicating data type of tables are not the same which helps on data cleaning. 🚩 \
    ![image](https://user-images.githubusercontent.com/102010540/159621703-3559b137-1d12-4231-86ed-e7f867181204.png)\
    How to find out which table(s) having wrong data type of field(s) ?\
    -->
- Are there issues with bias or credibility in this data? Does your data ROCCC? --> should not be becasue this is the only data from the case study 😂
- How are you addressing licensing, privacy, security, and accessibility? --> no idea
- How did you verify the data’s integrity? --> will check later. However, the data is huge . 
- How does it help you answer your question? --> TBD
- Are there any problems with the data? --> TBD
