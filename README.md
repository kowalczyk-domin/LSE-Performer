# LSE Performer – Stock Data Processing

## Overview

The Performer is responsible for processing individual transactions created by the Dispatcher and retrieving the latest stock data from the London Stock Exchange website.

Each transaction represents a single financial instrument and is processed independently to ensure scalability, reliability, and predictable execution time.

## Execution Model

The Performer is designed primarily for **unattended execution** in UiPath Orchestrator but can also be started manually using **UiPath Assistant** for testing or local execution scenarios.

Transactions are fetched from an Orchestrator Queue and processed sequentially or in parallel, depending on the available robot capacity.

## Responsibilities

- Retrieve Queue Items prepared by the Dispatcher  
- Validate transaction business data  
- Navigate the London Stock Exchange website  
- Search for the target financial instrument  
- Validate instrument identity against expected company name  
- Extract the latest stock price and timestamp (UTC)  
- Append processing results to an in-memory result table  

## Error Handling Strategy

The Performer does not use business-level retry mechanisms.  
Each transaction is processed once to ensure fast and predictable execution.

- **Business exceptions** (e.g. missing or mismatched company data) cause the transaction to be skipped and logged  
- **System exceptions** are handled according to REFramework rules and reported in Orchestrator  

This approach prioritizes reliability, transparency, and controlled execution time.

## Result Persistence

Processing results are persisted at the end of the job:

- Locally as a CSV file  
- Uploaded to UiPath Orchestrator Storage Bucket for centralized storage and backup  

## Key Design Principles

- Stateless transaction processing  
- Clear separation of responsibilities from the Dispatcher  
- Cloud-ready and unattended-first design  
- Configuration-driven behavior via Config.xlsx  
- Deterministic cleanup of browser and application resources  

---

The Performer can be easily extended to support additional data sources, alternative websites, or enhanced validation rules without changing the core execution model.
