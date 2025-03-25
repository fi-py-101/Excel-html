% Change vs Last 3 Months = 
VAR CurrentMonth = MAX(client_revenue_cumulative[Month])  

VAR CurrentMonthRevenue = 
    CALCULATE(
        SUM(client_revenue_cumulative[Revenue]), 
        client_revenue_cumulative[Month] = CurrentMonth
    )

VAR Last3MonthsAvg = 
    CALCULATE(
        AVERAGE(client_revenue_cumulative[Revenue]),   
        DATESINPERIOD(client_revenue_cumulative[Month], CurrentMonth, -3, MONTH),
        client_revenue_cumulative[Month] <> CurrentMonth  -- Exclude current month
    )

RETURN 
IF(
    NOT ISBLANK(Last3MonthsAvg),  
    (CurrentMonthRevenue - Last3MonthsAvg) / Last3MonthsAvg,  
    BLANK()
)