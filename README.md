Dynamic Enabled Count =
VAR selection = SELECTEDVALUE('Measure Selector'[Measure Label])
RETURN
IF(
    selection = "Enabled Count" || selection = "Both",
    CALCULATE(COUNTROWS(MIDaily), MIDaily[Normalised Status] = "Enabled"),
    0
)

Dynamic Not Enabled Count =
VAR selection = SELECTEDVALUE('Measure Selector'[Measure Label])
RETURN
IF(
    selection = "Not Enabled Count" || selection = "Both",
    CALCULATE(COUNTROWS(MIDaily), MIDaily[Normalised Status] <> "Enabled"),
    0
)