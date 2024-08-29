

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

---

```DAX
ePCRs 3.5.1 = CALCULATE(
    [All Incidents],
    -- The "beta test" medical form is 118
    'DwEms Fact_Incident'[Incident_Form_Number] = 118,
    -- Use this to exclude the training agency, etc
    'DwEms Fact_Incident'[Incident_Agency_Short_Name] = "portlandfi"
)
```

```DAX
ePCRs 3.5.1 - completed = CALCULATE(
    [ePCRs 3.5.1], 
    -- ePCRs with these status are considered completed
    'DwEms Dim_Incident'[Incident_Status] IN { "CQI Reviewed", "Crew Edits Complete", "Crew Documentation Finished", "Exported To State" }
)
```

```DAX
ePCRs (all) complete = 
CALCULATE(
    [ePCRs - 3.5.0],
    'DwEms Dim_Incident'[Incident_Status] IN {"Exported To State", "Crew Documentation Finished", "CQI Reviewed", "Flag For CQI Followup", "Crew Edits Complete"}
)
```

```DAX
ePCRs - INcomplete and Old = 
CALCULATE(
    [ePCRs - INcomplete],
    'DwEms Dim_Incident'[Incident_Status] IN {"In Progress", "Auto Posted", "Needs Crew Attention"},
    'DwEms Fact_Incident'[CreatedOn] <= NOW() - 72/24
)
```

```DAX
```
