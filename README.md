## Analyzing-and-Visualizing-Data-with-Power-BI

### Course: [Analyzing and Visualizing Data with Power BI](https://www.edx.org/course/data-analysis-in-power-bi?index=product_value_experiment_a&queryID=1fe231627ed8db3b2baba8e7f1b4b777&position=2) ###
#### _Meant to teach the fundamentals of Microsoft Power BI_ ####

### Institution: Davidson College ###

### Platform: edX.org ###

_Power BI Desktop Version: 2.84.701.0_

### Week 1: Data Modeling and Building a Report ###

Create a Power BI report that analyzes wine review data.

Techniques you’ll need to use:
* Import data from CSV file
* Create explicit measures with DAX using formulas like AVERAGE, COUNT, MEDIAN, MIN, and MAX.
* Add charts and other visuals

Source: [Kaggle](https://www.kaggle.com/zynicide/wine-reviews)

1.	Basic Data Modeling in Power Query
    * Remove columns
    * Rename columns
    
Note: While uploading the table to Power BI Desktop, Power Query detected an Error in row number 18,883. The error was excluded from the table in Power BI Desktop. 

2.	Building a Report
    * 100% Stacked Bar Chart – Highest, Median, and Lowest Price by Variety (DAX measures MAX, MEDIAN, MIN)
    * Treemap – Total Regions by Country (DAX measure COUNT)
    * Slicer - Points
    * Map – Top 50 Wineries by Country by Average Points (DAX measure Average, Filter Top N)

### Week 2: Sports Analytics and Global Economic Indicators ###

Create a Power BI report analyzing population and GDP for US states since 1997. Clean and filter the data using Power Query, then create relationships between your tables. Use measures in DAX to calculate growth rates and GDP per capita. Finally, visualize and explore the data in Power BI Desktop.

Techniques you’ll need to use:
* Power Query
  * Filtering
  * Replace values
  * Unpivoting
* Power BI Desktop
  * Map visual
  * Slicers and/or filters
  
Data sources:
* Federal Reserve Bank of St. Louis
* U.S. Bureau of Economic Analysis
* https://data.world/markmarkoh/us-state-table

1.	Data Modeling, Pivot and Unpivot, Relationships , Defining DAX Measures, 
* Choose Columns in US State file
* Unpivot GDP and Population files
* Merge Queries with Left Join by Abbreviation/State
* Replace Values in GDP File for DC, Added Indicator columns
* Append Queries Transformation GDP and Population files
* DAX Measure with Indicators for GDP, Population, and GDP per capita

2.	Slicers and Charts, Conditional Formatting, Visuals, Hierarchies, Bookmarks
* Page 1
  * Slicer for States
  * Charts for GDP, Population, and GDP per capita 
* Page 2
  * DAX Growth Rates with variables
Formula: GDP Growth = (Latest Year GDP – Earliest Year GDP) / Earliest Year GDP

DAX: 
```
GDP Growth = 
var FirstYear = CALCULATE(MIN('GDP/Population by State by Year'[Year]), 'GDP/Population by State by Year'[Indicator] = "GDP")
var LatestYear = CALCULATE(MAX('GDP/Population by State by Year'[Year]), 'GDP/Population by State by Year'[Indicator] = "GDP")
VAR FirstYearGDP = CALCULATE( [GDP], 'GDP/Population by State by Year'[Year] = FirstYear)
VAR LatestYearGDP = CALCULATE( [GDP], 'GDP/Population by State by Year'[Year] = LatestYear)
VAR Diff = LatestYearGDP - FirstYearGDP
return
DIVIDE(Diff, FirstYearGDP, 0)
```
* Page 2
  * Bookmark
    
   States – All states except Washington DC
   
   Capitol – Only Washington DC
* Page 3
  * Hierarchies and Matrix
  
### Week 3: Personal Finance ###

Create a Power BI report that visualizes city-wide budgets and actuals. Find out which departments are generating the most revenue or spending the most money, and which departments have gone furthest over their budgets.

Techniques you’ll need to use:
* Duplicate queries and merge queries
  * Normalize the data, cutting down on duplicates and allowing for faster and more efficient filtering of the data within Power BI. Transform each of these attributes into its own table using the Duplicate Queries functionality in Power Query. You would then use the Merge Queries functionality in your original table to merge each attribute's ID column back into the main table.
  
Note: The table is normalized according to the proposed model!

Data sources: [Budget amounts and actuals for the city of Houston, TX](https://courses.edx.org/assets/courseware/v1/74245fda4966c94008a57b5e9c226217/asset-v1:DavidsonX+DavidsonX.D005+1T2022a+type@asset+block/Week_3_Wrap-Up_File.xlsx)

Proposed data model:
![](https://github.com/silvanazdravevska/Analyzing-and-Visualizing-Data-with-Power-BI/blob/e6c36acfac8c20c44132daa3224616277a02c04a/Week%203.png)

Metrics: 
```
Actual Expenditures = CALCULATE(SUM('Budget vs Actuals'[Actuals]), 'Budget vs Actuals'[Revenue or Expenditure] = "Expenditures")
Actual Revenue = CALCULATE(SUM('Budget vs Actuals'[Actuals]), 'Budget vs Actuals'[Revenue or Expenditure] = "Revenues")
Budgeted Expenditures = CALCULATE(SUM('Budget vs Actuals'[Current Budget]), 'Budget vs Actuals'[Revenue or Expenditure] = "Expenditures")
Budgeted Revenue = CALCULATE(SUM('Budget vs Actuals'[Current Budget]), 'Budget vs Actuals'[Revenue or Expenditure] = "Revenues")
Expenses as % of Revenue = DIVIDE([Actual Expenditures], ABS([Actual Revenue]), 0)
Variance - Expenditures = [Budgeted Expenditures] - [Actual Expenditures]
Variance - Revenue = [Actual Revenue] - [Budgeted Revenue]
```

