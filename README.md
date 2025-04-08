Enabled Count = 
CALCULATE(
    COUNTROWS(MIDaily),
    MIDaily[Normalized Status] = "Enabled"
)

Not Enabled Count = 
CALCULATE(
    COUNTROWS(MIDaily),
    MIDaily[Normalized Status] <> "Enabled"
)

Total Requests = 
COUNTROWS(MIDaily)