flowchart TD
  subgraph External
    SALES[Sales Desk]
    BLOOM[Bloomberg / Market Data]
    CLIENT[CSOP ETF]
    EUCLID[Euclid Booking System]
  end

  subgraph BookingEngine[CSOP Booking Engine]
    DB[(Golden Source DB)]
    POS[Live Position Monitor]
    CALC[Price & Cashflow Calculator]
    FILEGEN[Euclid File Generator]
    UI[Booking UI / Approvals]
    ALERTS[Alerts & Validations]
  end

  subgraph DeskOps
    TRS_TRK[TRS Ledger]
    HEDGE[Hedge Execution (Futures Desk)]
    RISK[Risk & PnL System]
    RECON[Reconciliation & Audit]
  end

  SALES -->|New/Unwind/Roll/Reset Instr| UI
  UI -->|validate| ALERTS
  ALERTS -->|ok| CALC

  DB --> POS
  BLOOM --> DB
  BLOOM --> CALC
  DB --> CALC
  POS --> UI

  CALC -->|compute equity amount, basis, funding, comms, FX| FILEGEN
  FILEGEN --> EUCLID
  EUCLID --> TRS_TRK
  TRS_TRK --> HEDGE
  HEDGE -->|futures trades| RISK
  TRS_TRK --> RISK
  RISK --> RECON
  EUCLID --> RECON

  CLIENT -->|requests exposure / CREATIONS| SALES
  CLIENT -->|settlement T+1| EUCLID
  EUCLID -->|settlement instruction| BackOffice[Settlement/Finance]

  subgraph MonthlyCycle[Monthly roll & resets]
    ROLL[Roll computation] --> CALC
    RESET[Reset schedule generator] --> DB
    RESET --> POS
  end

  UI -->|generate CSV/Excel| FILEGEN
  ALERTS -->|exceptions| DeskOpsTeam[Ops team inbox]

  style BookingEngine fill:#f9f,stroke:#333,stroke-width:1px
  style DeskOps fill:#fffbcc,stroke:#333,stroke-width:1px
  style External fill:#ddf,stroke:#333,stroke-width:1px

  click BLOOM "https://www.bloomberg.com" "Market data"