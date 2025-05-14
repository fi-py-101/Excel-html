// ================= DATE TABLES =================

EnablementDateTable =
CALENDAR(MIN(rfq_joined[Enablement Date]), MAX(rfq_joined[Enablement Date]))

BusinessMetricsDateTable =
CALENDAR(MIN(rfq_joined[Date]), MAX(rfq_joined[Date]))

// ================= FILTERED DV01 =================

FilteredDV01 =
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

// ================= FILTERED NOMINAL =================

FilteredNominal =
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

// ================= FILTERED TICKETS =================

FilteredTickets =
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