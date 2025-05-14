SelectedBusinessMetric =
VAR SelectedMeasure = SELECTEDVALUE(MeasureSelector[Measure])
RETURN
SWITCH(
    SelectedMeasure,
    "DV01", [FilteredDV01],
    "Nominal", [FilteredNominal],
    "Tickets", [FilteredTickets],
    BLANK()
)