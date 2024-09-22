# New York-Transportation-Analysis-Dashboard

### https://app.powerbi.com/groups/me/reports/9231a540-5782-4e9a-8c5c-ff0d4caa7453/d1a55a886c19b9d67bd9?experience=power-bi

## Problem Statement

The rise in pedestrian fatalities in New York City has highlighted the need for a data-driven approach to improve safety at high-risk intersections. The NYC Department of Transportation (DOT) aims to address this issue by analyzing collision data to identify intersections with a high frequency of pedestrian collisions. The analysis will focus on factors such as speeding, distracted driving, and inadequate infrastructure. 

The goal is to develop recommendations for targeted interventions, including traffic calming measures and infrastructure improvements, to reduce pedestrian injuries and fatalities.

### Steps followed 

- Step 1 : Loaded the NYC_Collision dataset into Power BI Desktop, ensuring the data source and file formats were correct.

- Step 2 : Open power query editor & in view tab under Data preview section, check "column distribution", "column quality" & "column profile" options.
- Step 3 : Also since by default, profile will be opened only for 1000 rows so you need to select "column profiling based on entire dataset".
- Step 4 : Cleaned the data (errors, duplicates, data types).

  - Removed Errors: Got rid of rows with errors to keep the data clean.
   - Removed Duplicates: Deleted duplicate rows so all entries are unique.
   - Changed Data Types: Updated column types, like CRASH DATE to Date and CRASH TIME to Time Only, to make sure the formats are correct.

- Step 5: Handled missing values, removed unnecessary columns, and standardized values.

     - Filtered Out Missing or Out-of-Range Coordinates: Removed rows with missing latitude and longitude, and filtered out rows outside the range of New York City using a new column called "within-NYC."
   - Removed Unnecessary Columns: Deleted columns that weren’t needed for the analysis to keep the data focused.
    - Replaced Values: Standardized values by correcting similar entries like "AMBULANCE" to "Ambulance" and "trailer" to "Trailer," ensuring consistent data.
- Step 6: Added useful new columns, filtered the data, and reordered the columns for clarity.

   - Inserted New Columns: Created new columns like "Day of Week", "Start of Hour", and "Day Name" based on existing date and time data to enhance the dataset with more useful insights.
  - Filtered Rows: Applied final row filters to focus on specific data, such as relevant dates or boroughs.
   - Reordered Columns: Reorganized columns to ensure the dataset was well-structured and easy to interpret for further analysis.
- Step 7 : Identified the key metrics and the appropriate level of detail required to directly address the (Department of Transportation)DOT`s questions.
  - Borough Analysis
   - Severity of Collisions
   - Collision Pattern over Time
 
- Step 8 : In the report view, under the view tab, theme was selected.
 - Step 9: We conducted EDA on our dataset
    - Line chart displaying the distribution of collisions across the years, we also enabled drill down features so that the user can be able to track by month or week. 
   - A bar chart displaying collision disrtibution by Borough.

- Step 10 : New measures were created.

Following DAX expression was written for the same;

    Total Collisions: TotalCollisions = COUNTROWS('NYC_Collisions')
    Pedestrians Injured = SUM(NYC_Collisions[NUMBER OF PEDESTRIANS INJURED])
    Pedestrians Killed = SUM(NYC_Collisions[NUMBER OF PEDESTRIANS KILLED])
    

- Step 11: After creating the measures, three card visuals were added to the canvas, each displaying:
![Screenshot (105)](https://github.com/user-attachments/assets/9843eb26-08d5-4ea3-8139-19e480e85333)

- Step 12: We created an overall risk score by combining several key factors, including collision frequency, pedestrian involvement, severity ratio, and contributing risk factors. Each factor was measured and weighted to reflect its importance in determining the overall risk at each intersection. This comprehensive risk score allowed us to systematically evaluate and rank intersections, helping to identify the most high-risk areas. 



      Collision Frequencies = CALCULATE(COUNTROWS(NYC_Collisions),FILTER(    NYC_Collisions,NYC_Collisions[ON STREET NAME] 
      = MAX(NYC_Collisions[ON STREET NAME]) &&NYC_Collisions[CROSS STREET NAME] = MAX(NYC_Collisions[CROSS STREET NAME])))

      Pedestrian Involvement Normalized = DIVIDE([Pedestrians Injured] + [Pedestrians Killed],[Collision Frequencies])

      Severity Ratios = DIVIDE(SUM(NYC_Collisions[Number of Persons Injured]) + 5 * 
      SUM(NYC_Collisions[Number of Persons Killed]),[Collision Frequencies],0)
    
      Overall Risk ScoreS =(0.4 * [Collision Frequencies]) + (0.3 * [Severity Ratios]) +(0.2 * 
      [Pedestrian Involvement Normalized]) + (0.1 * [Contributing Risk Measure])

- Step 13: We created a new table from the existing NYC_Collisions after having identifiedthe high risk intersections.    

      HIghRiskIntersections = TOPN(20, NYC_Collisions, [Overall Risk ScoreS], DESC)

- Step 14 : Create visualizations to analyze the data:
  - Table Visual for Top 10-20 High-Risk Intersections - Added a table to display the top 10-20 high-risk intersections, showing their risk score and borough, allowing users to quickly identify the most dangerous locations.
  -   Map Visualization for Geographical Analysis - Created a map to visualize high-risk intersections, with markers indicating the count of people injured or killed at each location for clear geographic insights.
    - Infographic for Leading Causes of Pedestrian Injuries and Fatalities - Designed an infographic to highlight the top 5 causes of pedestrian injuries and fatalities, presenting key risk factors in a visually engaging format.



- Step 15 : Visual filters (Slicers) was added for the field named "Year" and "Month".

 - Step 16 : The report was then published to Power BI Service.

 
 # Report Snapshot (Power BI DESKTOP)

 
![Screenshot (106)](https://github.com/user-attachments/assets/4b8cf5e0-fc5b-473a-b26d-bc2c0af1d4be)

The second page
![Screenshot (107)](https://github.com/user-attachments/assets/cb599c59-279e-4a33-9f41-ff7fd93c5c68)

# Insights

A multiple page report was created on Power BI Desktop & it was then published to Power BI Service.

Following inferences can be drawn from the dashboard;



  Most of the high-risk intersections are concentrated in Manhattan, followed by Brooklyn, then the Bronx, with Queens having the fewest.
### [1] Overview of Pedestrian Collisions:
Total Pedestrian Collisions: 1.41 million.
Pedestrians Injured: 93.5 thousand.
Pedestrians Killed: 1.01 thousand.
These statistics emphasize the significant volume of pedestrian incidents in NYC, with a notable number of injuries and fatalities.
### [2] High-Risk Intersections:
The table lists the top 10-20 high-risk intersections for pedestrian collisions, along with their respective boroughs and risk scores. Here are some key intersections:

West Fordham Road and Major Deegan Expressway in Bronx (Risk Score: 279.32) tops the list.
Other intersections include Tillary Street & Flatbush Avenue Extension (Brooklyn), and 2 Avenue & East 59 Street (Manhattan).
This table provides a targeted focus on locations where city safety interventions could be prioritized.They show a high incidence of collisions, injuries, and fatalities, indicating an urgent need for infrastructure upgrades.
### [3] Borough Breakdown:
Bronx has the highest collision rates (0.45M).
Queens follows closely with 0.38M.
Manhattan has 0.31M.
Brooklyn and Staten Island have 0.21M and 0.06M respectively.
The data indicates that Bronx and Queens have the highest pedestrian collision risks.
### [4] Leading Causes of Pedestrian Injuries:
Driver Inattention: The most significant cause, accounting for the majority of pedestrian injuries (around 17-18K incidents).
Failure to Yield Right-of-Way: Another major factor contributing to injuries.
Backing Unsafely, Pedestrian Error/Confusion, and Unsafe Speed also feature prominently, though they are lower in number.
### [5] Leading Causes of Pedestrian Fatalities:
Failure to Yield and Driver Inattention also contribute significantly to pedestrian fatalities.
Unsafe Speed is another notable factor, particularly in contributing to fatalities.
### [6] Year-on-Year Trends:
The graph on the bottom right shows a general decline in total collisions over time, with a sharp dip likely around 2020 (possibly influenced by the COVID-19 pandemic). After that, the trend appears to level out.

### [7] Hourly Collision Distribution:
Peak Collision Hours:

Peak hours are concentrated between 12:00 PM to 7:00 PM, with the highest collisions around 5:00 PM.
This suggests a need to focus on road infrastructure during rush hours, particularly in high-traffic areas.

The distribution table indicates that collisions occur frequently across all days of the week, but the highest overall counts seem to occur on Fridays and Thursdays.
Sunday, while still having significant collisions, tends to have slightly lower totals compared to weekdays.


# Recommendations

1. Traffic Calming Measures:

Install speed bumps, raised crosswalks, and curb extensions to slow down vehicles and reduce crossing distances.
Add pedestrian refuge islands in large intersections for safer crossing.

2. Signal Timing Adjustments:

Use Leading Pedestrian Intervals (LPI) to give pedestrians a head start.
Extend pedestrian signal phases and implement adaptive traffic signals during peak times (3 PM - 6 PM).

3. Infrastructure Improvements:

Enhance lighting at intersections for better visibility.
Build pedestrian bridges or underpasses in high-traffic areas.
Apply the Complete Streets approach to redesign roads for all users.

4. Targeted Enforcement Campaigns:

Increase speed limit enforcement through speed cameras and patrols.
Launch driver awareness and pedestrian safety education campaigns to reduce accidents from driver inattention and pedestrian errors.

5. Speed Management Solutions:

Introduce variable speed limits during peak hours.
Install speed cameras at dangerous intersections to enforce limits and reduce speeding.

These recommendations aim to reduce collisions by improving infrastructure, adjusting traffic flow, and enhancing enforcement at high-risk intersections.
