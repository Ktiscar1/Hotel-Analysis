# Hotel-Analysis
This dashboard analyzes hotel performance by tracking year-over-year revenue growth, evaluating parking capacity needs based on usage trends, and identifying key patterns in the data to support informed operational and strategic decision-making.

# GOALS
* Is the hotel revenue grwowing yearly?
* Should parking lot size be increased?
* What trends are visible in the data?

# Steps
  1. Create a Database
  2. Query and Analyze Data with SQL
  3. Create a Data Visualization with Power BI

# 1. Create a Database
Database created using SQL Server Managament Studio to analyze the Hotel Booking Data.
Import the the data source to SSMS.

# 2. Querry Data
Data is now prepared in the database for SQL commands. 
In order to fetch data to view the tables the following commands are used:

SELECT * FROM dbo.['2018$']
SELECT * FROM dbo.['2019$']
SELECT * FROM dbo.['2020$']

And then the data is combined to create a single table to analyze:

SELECT * FROM dbo.['2018$']
UNION
SELECT * FROM dbo.['2019$']
UNION
SELECT * FROM dbo.['2020$']

In order to *answer our first question* a new instruction needs to be created. 
This new instruction should display revenue; however, upon analyzing the data, it is clear that revenue is not directly available in the table. Instead, the dataset includes the Average Daily Rate (ADR) along with the fields stays_in_week_nights and stays_in_weekend_nights, which can be used to derive revenue.

SELECT 
  (stays_in_week_nights + stays_in_weekend_nights) * adr
  AS revenue FROM hotels

To calculate the total revenue grouped by year, the arrival_date_year column must be included.

SELECT 
arrival_date_year
SUM((stays_in_week_nights + stays_in_weekend_nights) * adr)
AS revenue FROM hotels GROUP BY arrival_date_year

The revenue trend by hotel tybe can be determined by grouping the data by hotel and then looking for which hotels generated the most revenue.

SELECT 
arrival_date_year, hotel,
SUM((stays_in_week_nights + stays_in_weekend_nights) * adr)
AS revenue FROM hotels GROUP BY arrival_date_year, hotel

To *answer the second question*, car_parking_spaces ands number of guest staying in the hotel will be used.
This is need in order to generate a table that allow to ibserve the space for parking.

SELECT
arrival_date_year, hotel,
SUM((stays_in_week_nights + stays_in_weekend _nights) * adr) AS renenue,
CONCAT (round((sum(required_car_parking_spaces)/SUM(stays_in_week_nights +
stays_in_weekend_nights)) * 100, 2), '%') AS parking_percentage
FROM hotels group by arrival_date_year, hotel

In order to *answer the last question* the dashboard need to be created for visual representation of the data.

# 3. Create a Data Visualization with Power BI

![Dashboard](https://github.com/Ktiscar1/Hotel-Analysis/blob/57b96e12864441c8937953c18c20945bfd30ed2d/Dashboard.png)

The Dashboard show us the following Information:
  1. The revenue increased from 2018 to 2019, but it began to decrease from 2019 to 2020.
  2. The Average Daily Rate (ADR) has increased from 2019 to 2020, from $99.53 to $104.47.
  3. Total number of nights booked by customers decreased from 2019 to 2020.
  4. The discount percentage offered by the hotel has increased from 2019 to 2020 to attract more customers.
