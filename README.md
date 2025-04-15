Auto Enablement % = 
DIVIDE(
    CALCULATE(
        COUNTROWS(MIDaily),
        MIDaily[NormalizedStatus] = "Enabled",
        MIDaily[Type] = "Auto"
    ),
    CALCULATE(
        COUNTROWS(MIDaily),
        MIDaily[NormalizedStatus] = "Enabled"
    ),
    0
)