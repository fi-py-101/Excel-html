flowchart TD
  Sales[Sales desk: sends trade instruction (email/Slack/Excel)]
  OpsManual[Ops: Manual review of outstanding positions (Excel)]
  MarketData[Market Data (Bloomberg) - manual lookup]
  CalcExcel[Excel: Equity amount & price calculations]
  PrepareBooking[Prepare Euclid booking sheet (manual)]
  Validate[Manual validation / checks]
  BookingTeam[Booking team: upload to Euclid / book trades]
  Euclid[Euclid booking system]
  HedgeDesk[Hedge / Futures desk executes hedge]
  Reconcile[Reconciliation & PnL]
  Exceptions[Exceptions inbox / emails]

  Sales --> OpsManual
  OpsManual --> MarketData
  MarketData --> CalcExcel
  OpsManual --> CalcExcel
  CalcExcel --> PrepareBooking
  PrepareBooking --> Validate
  Validate -->|OK| BookingTeam
  Validate -->|Issue| Exceptions
  BookingTeam --> Euclid
  Euclid --> HedgeDesk
  HedgeDesk --> Reconcile
  Euclid --> Reconcile
  Exceptions --> OpsManual

  classDef manual fill:#ffe6e6,stroke:#cc0000
  classDef system fill:#e6f7ff,stroke:#0073e6
  class Sales,OpsManual,CalcExcel,PrepareBooking,Validate,BookingTeam,Exceptions manual
  class MarketData,Euclid,HedgeDesk,Reconcile system