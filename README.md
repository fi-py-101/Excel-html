TradingSessionMap = 
DATATABLE(
    "SessionTag", STRING,
    "StartTime", TIME,
    "EndTime", TIME,
{
    {"Asia Only", TIME(0,0,0), TIME(7,0,0)},
    {"Asia Full", TIME(0,0,0), TIME(9,0,0)},
    {"EU Only", TIME(9,0,0), TIME(14,0,0)},
    {"EU Full", TIME(9,0,0), TIME(16,0,0)},
    {"Others", TIME(16,0,0), TIME(0,0,0)}  // wraps to next day
}
)

RFQ_SessionMap =
GENERATE(
    RFQs_v2,
    FILTER(
        TradingSessionMap,
        VAR t = TIME(HOUR(RFQs_v2[timestamp]), MINUTE(RFQs_v2[timestamp]), SECOND(RFQs_v2[timestamp]))
        RETURN
            IF (
                TradingSessionMap[StartTime] < TradingSessionMap[EndTime],
                t >= TradingSessionMap[StartTime] && t < TradingSessionMap[EndTime],
                // handle wrap-around case like 16:00–00:00
                t >= TradingSessionMap[StartTime] || t < TradingSessionMap[EndTime]
            )
    )
)