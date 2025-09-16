# Dataflow Design

## Overview
The primary purpose of the dataflow gen2 is to transform data as it moves it from one place to another, so since the data is being moved from the lakehouse to the gold warehouse, there's going to be heavy transformations in it but in this section so the DAX code is provided as well. The folllowing image shows all the new columns and steps performed in the dataflow.

![](../Screenshots/dataflow-gen2-design.png)


### Index
First we create an index in order to compute the addedd columns after, this because we need to track previous rows in future calculations.

### Return Percentage
**Purpose:** Measure the daily price change as a percentage.

**Formula:**
Return (%) = [(Current Close - Previous Close) / Previous Close] × 100

**DAX code:**
```powerquery
try (([close] - #"Added index"[close]{[Index]-1}) / (#"Added index"[close]{[Index]-1}))*100 otherwise null
```


### Simple moving AVG 5
**Purpose:** Smooth price data to identify trends.

**Formula:**
SMA5 = (Price_t + Price_{t-1} + Price_{t-2} + Price_{t-3} + Price_{t-4}) / 5

**DAX code:**
```powerquery
try ([close] + #"Added index"[close]{[Index]-1} + #"Added index"[close]{[Index]-2} + #"Added index"[close]{[Index]-3} + #"Added index"[close]{[Index]-4})/5 otherwise null
```
### Volatility
**Purpose:** Measure price risk using standard deviation of returns.

**Formula:**
Volatility = STDEV(Returns last 5 periods)

**DAX code:**
```powerquery
try List.StandardDeviation({ #"Added index"[close]{[Index]-4}, #"Added index"[close]{[Index]-3}, #"Added index"[close]{[Index]-2}, #"Added index"[close]{[Index]-1}, [close] }) otherwise null
```

### Volume Ratio
**Purpose:** Compare current volume against 5-period average.

**Formula:**
Volume Ratio = Current Volume / 5-Period Average Volume

**DAX code:**
```powerquery
try [volume] / List.Average({ #"Added index"[volume]{[Index]-4}, #"Added index"[volume]{[Index]-3}, #"Added index"[volume]{[Index]-2}, #"Added index"[volume]{[Index]-1}, #"Added index"[volume]{[Index]} }) otherwise null
```
