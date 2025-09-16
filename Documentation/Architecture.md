# Architecture Design

## Overview
Implementation of Medallion Architecture in Microsoft Fabric for IBM financial data processing.

## Bronze Layer (Raw Data)
**Purpose:** To store raw, unprocessed data.
Used Vantage API to retrieve data from the stock market and store it in a file within a lakehouse

![](../Screenshots/fileinLakehouse.png)
