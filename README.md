// 1. IsBacklog: Flag backlog RFEs (RFE date on or before 31-Dec-2024)
IsBacklog = 
IF(MIDaily[RFE Request Date] <= DATE(2024,12,31), "Backlog", "Non-Backlog")

// 2. IsEnabled: Mark Enabled vs Not Enabled
IsEnabled = 
IF(MIDaily[Normalized Status] = "Enabled", "Yes", "No")

// 3. DaysToEnable: Days taken to enable RFEs (if enabled)
DaysToEnable = 
IF(MIDaily[IsEnabled] = "Yes", 
    DATEDIFF(MIDaily[RFE Request Date], MIDaily[Enablement Date], DAY),
    BLANK()
)

// 4. OutstandingAge: Days since RFE was raised (if not enabled)
OutstandingAge = 
IF(MIDaily[IsEnabled] = "No", 
    DATEDIFF(MIDaily[RFE Request Date], TODAY(), DAY),
    BLANK()
)

// 5. EnabledWithin30: Flags RFEs enabled within 30 days
EnabledWithin30 = 
IF(MIDaily[IsEnabled] = "Yes" && 
   DATEDIFF(MIDaily[RFE Request Date], MIDaily[Enablement Date], DAY) <= 30,
   "Yes", "No"
)

// 6. RFE_Month: YYYY-MM of RFE request
RFE_Month = 
FORMAT(MIDaily[RFE Request Date], "YYYY-MM")

// 7. Enablement_Month: YYYY-MM of enablement
Enablement_Month = 
FORMAT(MIDaily[Enablement Date], "YYYY-MM")