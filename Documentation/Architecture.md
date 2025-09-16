# Architecture Design

## Overview
Implementación de Medallion Architecture en Microsoft Fabric para procesamiento de datos financieros de IBM.

## Bronze Layer (Raw Data)
**Propósito:** Almacenar datos crudos sin transformar.

```sql
-- Estructura de la tabla bronze
CREATE TABLE bronze.alpha_vantage_data (
    raw_data STRING,           -- JSON crudo de la API
    ingestion_timestamp TIMESTAMP, -- Fecha/hora de ingesta
    source_url STRING,         -- URL de la API call
    processing_date DATE       -- Fecha de procesamiento
)
