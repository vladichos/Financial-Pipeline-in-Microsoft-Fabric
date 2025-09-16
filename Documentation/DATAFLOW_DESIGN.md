# Dataflow Design

## Overview
The primary purpose of the dataflow gen2 is to transform data as it moves it from one place to another, so since the data is being moved from the lakehouse to the gold warehouse, there's going to be heavy transformations in it but in this section im not going to go deep into DAX code, you can refer to the Code folder for that part.

### Index
First we create an index in order to compute the addedd columns after, this because we need to track previous rows in future calculations.

### Daily Return Percentage
**Purpose:** Measure the daily price change as a percentage.
**Implementation:**
```powerquery
// Calculate Daily Return Percentage
let
    Source = #"Sorted Data",
    AddPreviousClose = Table.AddColumn(Source, "previous_close", 
        each [close]_{n-1}, type number),
    AddDailyReturn = Table.AddColumn(AddPreviousClose, "daily_return_pct", 
        each ([close] - [previous_close]) / [previous_close] * 100, type number)
in
    AddDailyReturn
