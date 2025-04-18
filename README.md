Total RFEs Raised = 
COUNTROWS(MIDaily)

RFEs Enabled Within 30 Days = 
CALCULATE(
    COUNTROWS(MIDaily),
    MIDaily[IsEnabled] = "Yes" &&
    MIDaily[EnabledWithin30] = "Yes"
)

This chart shows how many RFEs were raised each month and how many of those were enabled within 30 days of the request. Use filters above to slice by Platform and Product Group.