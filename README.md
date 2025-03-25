% Change vs Last 3 Months = 
VAR CurrentMonthRevenue = SUM(Revenue[Amount])

VAR Last3MonthsAvg = 
    CALCULATE(
        AVERAGE(Revenue[Amount]),
        DATESINPERIOD(Revenue[Month], MAX(Revenue[Month]), -3, MONTH)
    )

RETURN 
IF(
    NOT ISBLANK(Last3MonthsAvg),
    (CurrentMonthRevenue - Last3MonthsAvg) / Last3MonthsAvg,
    BLANK()
)