# Analysis of the worst US states for driving

This is an analysis of the worst states in the US for driving. I am creating this repo (and an associated blog post!) as part of the "Data Scientist Nanodegree Program" with Udacity

## Setup
If you want to recreate this analysis, you can follow the instructions below. This was executed on Windows 10 and edited in VS Code, but I see no reason why this won't run on other platforms. You WILL need python on your system.

1. Clone this repo
2. Create a virtual environment inside the repo:
```
python -m venv venv -- assuming you have python isntalled and in your path. 
```
3. Install requirements. Make sure to activate your virtual environment, then run the following:
```
python -m pip install -r requirements.txt
```
4. You will need to acquire the data for this analysis yourself - I have not committed them to this repo. You can find links to the data below. I recommend finding them and creating a directory structure as follows:
```
projectroot
- venv
- .gitignore
- requirements.txt
- data # you will need to make this and the following structure
    - raw
        - US_Accidents_Dec21_updated.csv
        - US_Constructions_Dec21.csv
        - hm60.xls
        - fta-apportionments-formula-and-discretionary-programs-state-fy-1998-2022-Full-Year.xls
    - working
        - FTA_funding_2021_raw.csv
        - hm60_raw.csv
```
- NOTE - the csv files in the working directory above were created from the excel files referenced in "raw". Instructions on creating these excel files without MS Excel are below. The schema which results from this activity can be viewed in "DrivingAnalysisExploration.ipynb"

### Data

I have utilized the following datasets:

- US Accidents (2016 - 2021) - https://www.kaggle.com/datasets/sobhanmoosavi/us-accidents?datasetId=199387&sortBy=voteCount
- US Road Construction and Closures (2016 - 2021) | Kaggle - https://www.kaggle.com/datasets/sobhanmoosavi/us-road-construction-and-closures
- Table HM-60 - Highway Statistics 2020 - Policy | Federal Highway Administration (dot.gov) - https://www.fhwa.dot.gov/policyinformation/statistics/2020/hm60.cfm
- FTA Allocations for Formula and Discretionary Programs by State FY 1998-2022 Full Year | FTA (dot.gov) - https://www.transit.dot.gov/funding/grants/fta-allocations-formula-and-discretionary-programs-state-fy-1998-2022-full-year

In the interest of preserving the rights of the hosts of these data, I will not place them in my repo. Generally speaking, it is bad practice to have data in your repo anyway - there should be a proper pipeline for getting these data into a repo, since comitting large amounts of data to a repo will dramatically slow down push/pull times. 

If this analysis were to be repeated over and over, I would probably use something like [requests](https://docs.python-requests.org/en/latest/) to make an automated script for pulling these data. However, seeing as I am pulling these data one time, it is faster to just pull the data via my web browser.

### Creating raw forms of government data
My two datasets from the govenment are hosted as MS Excel workbooks, not unformatted csv files. As expected, there is a lot of annoying formatting in these workbooks, including:
- Merged cells, which have unpredictable behavior when read into pandas
- Lots of header rows. These can be ignored on a pandas read using the header rows option, but it still requires some observation to figure out how many rows to cut off
- Text descriptions spread across adjacent cells. 
- Summary statistics, some of which are Excel formulas and others which are hard-coded values. I do not want to run the risk of summing a column and accidentally including a "grand total" cell, so it would be useful to remove these.

I could use pandas to create reproducible code to clean up these sheets, but "excel-engineering" in pandas is time-consuming in my experience. In the interest of time, it has just been faster to use spreadsheet software such as [LibreOffice](https://www.libreoffice.org/discover/libreoffice/) to copy the data to a new spreadsheet and execute the following corrections:
- Delete summary rows
- Delete unnecessary header rows
- Concatenate text from multiple description cells to create a single, useful header row
- Un-merge merged cells and make sure these data are properly included in my header row
I have saved these sheets as unformatted csv files. The schema for these can be viewed in my exploration notebook.