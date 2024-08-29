# Measures

#### **What is a Measure?**
A **measure** is a calculation created in Power BI using the Data Analysis Expressions (DAX) formula language. Measures are designed to perform calculations on your data that aggregate over rows and produce a single scalar value. They are dynamic and recalculated based on interactions in your reports, such as filtering and slicing.

#### **What is DAX?**
**DAX (Data Analysis Expressions)** is a formula language used in Power BI, Power Pivot, and Analysis Services. It is a powerful language consisting of functions, operators, and constants that support advanced data manipulation and querying to produce meaningful results in your reports.



#### **How Measures are Constructed and Used in Power BI**

1. **Creating Measures:**
   - For a consistent workflow, instead of creating measures in the Power BI "service" (read: the online website), use the desktop application.
   - The current method of creating measures involves opening the relevant shared `.pbix` file from the OneDrive. From here measures can be created and tested. Once built, the `.pbix` file can be "published" to the service.
   - To create a measure, you can right-click on a table in the Fields pane and select "New measure," or use the Modeling tab and select "New Measure."
   - When defining a measure, you use DAX formulas to perform calculations on columns and tables in your dataset.

     An example measure:
     ```DAX
        All ePCRs = CALCULATE(
            COUNTROWS('DwEms Fact_Incident'),
            FILTER(
                'DwEms Fact_Incident',
                'DwEms Fact_Incident'[Incident_Agency_Short_Name] = "portlandfi"
            ),
            FILTER(
                'DwEms Dim_Incident',
                'DwEms Dim_Incident'[Incident_Status] IN { "CQI Reviewed", "Crew Edits Complete", "Crew Documentation Finished", "Exported To State" }
            )
        )
     ```

2. **Using Measures:**

Measures are used in Power BI reports to present aggregated data such as sums, counts, averages, minimums, maximums, or more complex calculations.  You can use measures in visualizations, such as bar charts, pie charts, tables, matrices, and others.  Measures automatically respect the filter context of the visuals they are used in. This means they will recalculate dynamically based on any filters, slicers, or drill-down operations applied in the report.  Measures recalculate based on user interactions, ensuring relevant and up-to-date insights. Power BI optimizes measures for performance, making them efficient in handling large datasets.






---



```DAX
All Incidents = COUNTROWS('DwEms Fact_Incident')
```

```DAX
ePCRs - 3.5.1 = CALCULATE(
    [All Incidents],
    -- This captures ePCRs made with the "new" medical form
    -- This form launched 2024.05.20 for station (3, 8, 9 and 31)
    -- This form launched 2024.07.01 for CHAT
    -- This form launched 2024.08.19 for the rest of PF&R
    'DwEms Fact_Incident'[Incident_Form_Number] = 118
)
```