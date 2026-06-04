# Cribl Log Processing Pipeline for Splunk

## Overview

This project demonstrates how Cribl Stream can be used as a data processing layer between a data source and Splunk Enterprise.

The objective was to collect infrastructure metrics from a Linux host, process them through Cribl, enrich events with metadata, and forward them to Splunk for analysis.

Rather than sending raw data directly to Splunk, Cribl provides the ability to enrich, filter, route, and optimize events before they are indexed.


## Architecture

```text
System Metrics Source
         │
         ▼
     Cribl Stream
         │
         ▼
 Metrics_Optimization
      Pipeline
         │
         ▼
 Metrics_Optimization
        Route
         │
         ▼
 Splunk Single Instance
         │
         ▼
      Search &
      Analysis
```


## Project Objectives

* Configure a System Metrics source within Cribl Stream
* Create a custom processing pipeline
* Enrich incoming events with additional metadata
* Route events through Cribl before forwarding to Splunk
* Validate successful ingestion within Splunk
* Demonstrate core Cribl administration and engineering capabilities

---

## Environment

| Component        | Value                  |
| ---------------- | ---------------------- |
| Operating System | Kali Linux             |
| Cribl Version    | Community Edition      |
| Splunk Version   | Splunk Enterprise      |
| Source Type      | System Metrics         |
| Destination Type | Splunk Single Instance |


## Source Configuration

### Source Type

```text
System Metrics
```

### Input ID

```text
in_system_metrics
```

### Polling Interval

```text
10 Seconds
```

### Metrics Collected

* CPU Utilization
* Memory Utilization
* Available Memory
* Disk Operations
* Network Throughput
* Host Statistics


## Pipeline Configuration

### Pipeline Name

```text
Metrics_Optimization
```

The purpose of the pipeline is to enrich events before they are sent to Splunk.

### Function Used

```text
Eval
```

### Added Fields

```javascript
environment = "lab"
project = "cribl-metrics-optimization"
owner = "soc-team"
```

### Example Event

Before enrichment:

```json
{
  "host": "kali"
}
```

After enrichment:

```json
{
  "host": "kali",
  "environment": "lab",
  "project": "cribl-metrics-optimization",
  "owner": "soc-team"
}
```


## Route Configuration

### Route Name

```text
Metrics_Optimization_Route
```

### Route Settings

| Setting     | Value                         |
| ----------- | ----------------------------- |
| Filter      | true                          |
| Pipeline    | Metrics_Optimization          |
| Destination | splunk:splunk_single_instance |
| Final       | Enabled                       |

### Route Workflow

1. Receive metrics from the System Metrics source.
2. Process events through the Metrics_Optimization pipeline.
3. Enrich events with metadata.
4. Forward events to Splunk Enterprise.


## Destination Configuration

### Destination Type

```text
Splunk Enterprise
```

The destination receives processed events from Cribl and indexes them into Splunk for searching and analysis.


## Validation

### Source Validation

Verified metrics were actively generated using Cribl Live Data.

**Result:** PASS

### Pipeline Validation

Verified enrichment fields were successfully added to incoming events.

Expected fields:

```text
environment
project
owner
```

**Result:** PASS

### Route Validation

Verified events successfully traversed the configured route.

**Result:** PASS

### Splunk Validation

#### Verify Project Field

```spl
index=* project="cribl-metrics-optimization"
```

#### Count Events by Host

```spl
index=* project="cribl-metrics-optimization"
| stats count by host
```

#### Count Events by Environment

```spl
index=* project="cribl-metrics-optimization"
| stats count by environment
```

**Result:** PASS


## Skills Demonstrated

### Cribl Stream

* Source Configuration
* Pipeline Development
* Event Enrichment
* Route Management
* Destination Configuration
* Data Validation

### Splunk

* Search Development
* Data Validation
* Event Analysis
* Metadata Correlation

### Linux

* System Administration
* Metrics Collection
* Service Validation
* Troubleshooting


## Business Value

Cribl provides a flexible processing layer that allows organizations to:

* Enrich data before indexing
* Standardize event formats
* Improve search efficiency
* Simplify downstream analysis
* Reduce ingestion costs

This project demonstrates the foundational workflow used to achieve those objectives.


## Future Improvements

### Metric Filtering

Retain only:

* CPU Metrics
* Memory Metrics
* Disk Metrics

Remove unnecessary metrics before indexing.

### Multi-Route Architecture

Create dedicated routes for:

* Infrastructure Metrics
* Security Logs
* Application Logs

### Dashboard Development

Build Splunk dashboards for:

* CPU Utilization
* Memory Utilization
* Disk Usage
* Host Health Monitoring



SOC Analyst | SIEM Engineer | Splunk & Cribl Enthusiast
