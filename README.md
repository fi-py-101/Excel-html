flowchart TD
  Start([Incoming event])

  Start -->|Fresh order| Fresh[Sales: Fresh Order]
  Start -->|Alert: unwind/roll/reset| Alert[Alert received]

  Fresh --> Dashboard[Open Dashboard]
  Alert --> Dashboard

  Dashboard --> SelectProduct[Select Product (HSTECH Lev/Inv, Samsung Lev/Inv, SK Hynix)]

  SelectProduct --> DecisionType{Is this a Fresh order or an Action?}

  DecisionType -->|Fresh| FreshFlow[Enter parameters & confirm]
  DecisionType -->|Unwind| UnwindFlow[Unwind flow]
  DecisionType -->|Roll| RollFlow[Roll flow]
  DecisionType -->|Reset| ResetFlow[Reset flow]

  subgraph Fresh_sub [Fresh order path]
    FreshFlow --> EnterParams[Enter trade params: qty, direction, currency, settle date]
    EnterParams --> ValidateFresh[Validation & warnings]
    ValidateFresh -->|OK| GenBookFresh[Generate booking lines or Book]
    ValidateFresh -->|Issues| AlertOps[Send to Ops for review]
    GenBookFresh --> Euclid[Euclid / Booking System]
  end

  subgraph Unwind_sub [Unwind path]
    UnwindFlow --> ShowPositionsU[Display outstanding positions (full details)]
    ShowPositionsU --> SelectPosU[Select one/multiple positions]
    SelectPosU --> EditValuesU[Optional: edit prices/FX/dates]
    EditValuesU --> ValidateU[Validation & warnings]
    ValidateU -->|OK| GenBookU[Generate booking lines or Book]
    GenBookU --> Euclid
    ValidateU -->|Issues| AlertOps
  end

  subgraph Roll_sub [Roll path]
    RollFlow --> ShowPositionsR[Display outstanding positions & roll candidates]
    ShowPositionsR --> SelectPosR[Select position to roll or create custom roll]
    SelectPosR --> EditValuesR[Optional: edit roll params, basis, FX]
    EditValuesR --> ValidateR[Validation & warnings]
    ValidateR -->|OK| GenBookR[Generate booking lines or Book]
    GenBookR --> Euclid
    ValidateR -->|Issues| AlertOps
  end

  subgraph Reset_sub [Reset path]
    ResetFlow --> ShowPositionsS[Display outstanding positions & highlight reset dates]
    ShowPositionsS --> SelectPosS[Select positions to reset]
    SelectPosS --> ShowDetailsS[Show relevant dates, start price, end price, FX, day-count, basis]
    ShowDetailsS --> EditValuesS[Optional: enter custom values]
    EditValuesS --> ValidateS[Validation & warnings]
    ValidateS -->|OK| GenBookS[Generate booking lines or Book]
    GenBookS --> Euclid
    ValidateS -->|Issues| AlertOps
  end

  Euclid --> NotifyHedge[Notify Hedge Desk / Generate hedge ticket]
  Euclid --> NotifySettle[Send to Settlement/Finance]
  Euclid --> Recon[Reconciliation & PnL]
  NotifyHedge --> HedgeDesk[Hedge Execution (Futures Desk)]
  HedgeDesk --> Recon
  Recon --> OpsClose[Close trade / update dashboard]
  AlertOps --> OpsTeam[Ops team review inbox]
  OpsTeam --> OpsClose

  classDef userAction fill:#e8f5e9,stroke:#333
  classDef system fill:#e6f0ff,stroke:#333
  class Start,Fresh,Alert,Dashboard,SelectProduct,userAction
  class Euclid,NotifyHedge,NotifySettle,Recon,HedgeDesk,system