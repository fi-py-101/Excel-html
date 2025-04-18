Outstanding Backlog = 
61297 - 
CALCULATE(
    COUNTROWS(MIDaily),
    MIDaily[IsBacklog] = "Backlog",
    MIDaily[IsEnabled] = "Yes",
    FILTER(
        ALLSELECTED(MIDaily[Enablement_Month]),
        MIDaily[Enablement_Month] <= MAX(MIDaily[Enablement_Month])
    )
)