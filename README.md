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