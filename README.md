// ========== DATE TABLES (Dynamic range) ==========

EnablementDateTable =
VAR MinDate = CALCULATE(MIN(rfq_joined[Enablement Date]), REMOVEFILTERS(rfq_joined))
VAR MaxDate = CALCULATE(MAX(rfq_joined[Enablement Date]), REMOVEFILTERS(rfq_joined))
RETURN
ADDCOLUMNS (
    CALENDAR(MinDate, MaxDate),
    "Year", YEAR([Date]),
    "Month Number", MONTH([Date]),
    "Month Name", FORMAT([Date], "MMMM"),
    "Year-Month", FORMAT([Date], "YYYY-MM")
)

BusinessMetricsDateTable =
VAR MinDate = CALCULATE(MIN(rfq_joined[Date]), REMOVEFILTERS(rfq_joined))
VAR MaxDate = CALCULATE(MAX(rfq_joined[Date]), REMOVEFILTERS(rfq_joined))
RETURN
ADDCOLUMNS (
    CALENDAR(MinDate, MaxDate),
    "Year", YEAR([Date]),
    "Month Number", MONTH([Date]),
    "Month Name", FORMAT([Date], "MMMM"),
    "Year-Month", FORMAT([Date], "YYYY-MM")
)

// ========== FILTERED DV01 ==========

FilteredDV01 :=
VAR EnabledPairs =
    CALCULATETABLE(
        SUMMARIZE(
            rfq_joined,
            rfq_joined[Trader ID],
            rfq_joined[Product Group],
            rfq_joined[EMA Product]
        ),
        TREATAS(VALUES(EnablementDateTable[Date]), rfq_joined[Enablement Date])
    )
RETURN
CALCULATE(
    SUM(rfq_joined[DV01]),
    TREATAS(SELECTCOLUMNS(EnabledPairs, "Trader ID", rfq_joined[Trader ID]), rfq_joined[Trader ID]),
    TREATAS(SELECTCOLUMNS(EnabledPairs, "Product Group", rfq_joined[Product Group]), rfq_joined[Product Group]),
    TREATAS(SELECTCOLUMNS(EnabledPairs, "EMA Product", rfq_joined[EMA Product]), rfq_joined[EMA Product]),
    TREATAS(VALUES(BusinessMetricsDateTable[Date]), rfq_joined[Date])
)

// ========== FILTERED NOMINAL ==========

FilteredNominal :=
VAR EnabledPairs =
    CALCULATETABLE(
        SUMMARIZE(
            rfq_joined,
            rfq_joined[Trader ID],
            rfq_joined[Product Group],
            rfq_joined[EMA Product]
        ),
        TREATAS(VALUES(EnablementDateTable[Date]), rfq_joined[Enablement Date])
    )
RETURN
CALCULATE(
    SUM(rfq_joined[Nominal]),
    TREATAS(SELECTCOLUMNS(EnabledPairs, "Trader ID", rfq_joined[Trader ID]), rfq_joined[Trader ID]),
    TREATAS(SELECTCOLUMNS(EnabledPairs, "Product Group", rfq_joined[Product Group]), rfq_joined[Product Group]),
    TREATAS(SELECTCOLUMNS(EnabledPairs, "EMA Product", rfq_joined[EMA Product]), rfq_joined[EMA Product]),
    TREATAS(VALUES(BusinessMetricsDateTable[Date]), rfq_joined[Date])
)

// ========== FILTERED TICKETS ==========

FilteredTickets :=
VAR EnabledPairs =
    CALCULATETABLE(
        SUMMARIZE(
            rfq_joined,
            rfq_joined[Trader ID],
            rfq_joined[Product Group],
            rfq_joined[EMA Product]
        ),
        TREATAS(VALUES(EnablementDateTable[Date]), rfq_joined[Enablement Date])
    )
RETURN
CALCULATE(
    SUM(rfq_joined[Tickets]),
    TREATAS(SELECTCOLUMNS(EnabledPairs, "Trader ID", rfq_joined[Trader ID]), rfq_joined[Trader ID]),
    TREATAS(SELECTCOLUMNS(EnabledPairs, "Product Group", rfq_joined[Product Group]), rfq_joined[Product Group]),
    TREATAS(SELECTCOLUMNS(EnabledPairs, "EMA Product", rfq_joined[EMA Product]), rfq_joined[EMA Product]),
    TREATAS(VALUES(BusinessMetricsDateTable[Date]), rfq_joined[Date])
)