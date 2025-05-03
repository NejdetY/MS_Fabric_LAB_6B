# MS_Fabric_LAB_6B
# Querying a Data Warehouse in Microsoft Fabric

This project showcases how to create, query, and manage a sample data warehouse using **Microsoft Fabric**.  
It walks through fundamental operations such as creating a workspace, running SQL queries, verifying data consistency, creating views, and cleaning up resources.

---

## Project Overview

**Objective**:  
Learn the essential steps to query a data warehouse in Microsoft Fabric, perform data analysis, and manage data quality.

**Tools Used**:
- Microsoft Fabric (Trial License)
- SQL Query Editor within Fabric

---

## Steps Performed

### 1. Create a Fabric Workspace
- Navigated to [Microsoft Fabric Home](https://app.fabric.microsoft.com/home?experience=fabric).
- Created a new workspace with Fabric capacity enabled (Trial, Premium, or Fabric).

### 2. Create a Sample Data Warehouse
- Created a new sample data warehouse named `sample-dw` from the "Create" menu.
- The warehouse was automatically populated with taxi ride analysis sample data.

### 3. Query the Data Warehouse
Executed several SQL queries:

- **Monthly Trip Summary**:
    ```sql
    SELECT D.MonthName, COUNT(*) AS TotalTrips, SUM(T.TotalAmount) AS TotalRevenue
    FROM dbo.Trip AS T
    JOIN dbo.[Date] AS D ON T.[DateID] = D.[DateID]
    GROUP BY D.MonthName;
    ```

- **Day of the Week Trip Averages**:
    ```sql
    SELECT D.DayName, AVG(T.TripDurationSeconds) AS AvgDuration, AVG(T.TripDistanceMiles) AS AvgDistance
    FROM dbo.Trip AS T
    JOIN dbo.[Date] AS D ON T.[DateID] = D.[DateID]
    GROUP BY D.DayName;
    ```

- **Top 10 Pickup and Drop-off Locations**:
    ```sql
    SELECT TOP 10 G.City, COUNT(*) AS TotalTrips
    FROM dbo.Trip AS T
    JOIN dbo.Geography AS G ON T.PickupGeographyID = G.GeographyID
    GROUP BY G.City
    ORDER BY TotalTrips DESC;
    
    SELECT TOP 10 G.City, COUNT(*) AS TotalTrips
    FROM dbo.Trip AS T
    JOIN dbo.Geography AS G ON T.DropoffGeographyID = G.GeographyID
    GROUP BY G.City
    ORDER BY TotalTrips DESC;
    ```

### 4. Verify Data Consistency
- Checked for trips with unusually long durations (>24 hours):
    ```sql
    SELECT COUNT(*) FROM dbo.Trip WHERE TripDurationSeconds > 86400;
    ```

- Checked and **removed** trips with negative trip durations:
    ```sql
    SELECT COUNT(*) FROM dbo.Trip WHERE TripDurationSeconds < 0;
    DELETE FROM dbo.Trip WHERE TripDurationSeconds < 0;
    ```

### 5. Create and Save a View
- Created a filtered view for trips in January (`Month = 1`):

    ```sql
    SELECT D.DayName, AVG(T.TripDurationSeconds) AS AvgDuration, AVG(T.TripDistanceMiles) AS AvgDistance
    FROM dbo.Trip AS T
    JOIN dbo.[Date] AS D ON T.[DateID] = D.[DateID]
    WHERE D.Month = 1
    GROUP BY D.DayName;
    ```

- Saved the query as a view named **vw_JanTrip** under `Schemas » dbo » Views`.

---

## Key Learnings

- How to create and manage a workspace in Microsoft Fabric.
- How to run and optimize SQL queries using the Fabric SQL editor.
- How to detect and fix data inconsistencies in a data warehouse.
- How to create reusable SQL views for reporting and analytics.

---

## Further Information

For more details, refer to the official documentation:  
👉 [Query using the SQL query editor in Microsoft Fabric](https://learn.microsoft.com/en-us/fabric/data-warehouse/query-data-warehouse-sql)

---

> This project was completed as part of my Microsoft Fabric hands-on learning journey.
