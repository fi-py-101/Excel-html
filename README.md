-- Step 2: Create Measure Selector Table
Measure Selector = DATATABLE(
    "Measure Label", STRING,
    {
        {"Enabled Count"},
        {"Not Enabled Count"},
        {"Total Count"}
    }
)

-- Step 4: Create Dynamic Measure
Selected Measure Value = 
SWITCH(
    SELECTEDVALUE('Measure Selector'[Measure Label]),
    "Enabled Count", [Enabled Count],
    "Not Enabled Count", [Not Enabled Count],
    "Total Count", [Total Count]
)
Dynamic Enabled Count =
IF(
    SELECTEDVALUE('Measure Selector'[Measure Label]) = "Enabled Count",
    CALCULATE(COUNTROWS(MIDaily), MIDaily[Normalised Status] = "Enabled"),
    0
)

Dynamic Not Enabled Count =
IF(
    SELECTEDVALUE('Measure Selector'[Measure Label]) = "Not Enabled Count",
    CALCULATE(COUNTROWS(MIDaily), MIDaily[Normalised Status] <> "Enabled"),
    0
)
