# Cribl Log Processing Pipeline for Splunk
Goal: Build a complete log pipeline using Cribl Stream to collect, filter, enrich, route, and send logs to Splunk.


This project demonstrates how to use **Cribl Stream** to ingest, process, enrich, and forward system metrics to a **Splunk Enterprise Single Instance** deployment.

The pipeline was built in a lab environment running Kali Linux and showcases core Cribl concepts including:

* Source Configuration
* Data Routing
* Pipeline Processing
* Event Enrichment
* Splunk Integration
* Data Validation


## 🏗 Architecture

```text
System Metrics Source
        │
        ▼
   Cribl Stream
        │
        ▼
Metrics_Optimization Pipeline
        │
        ├── Add Metadata
        ├── Event Enrichment
        └── Data Processing
        │
        ▼
Splunk Destination
        │
        ▼
Splunk Enterprise
```



## 🎯 Project Objectives

* Collect host metrics using Cribl System Metrics
* Route events through a custom Cribl pipeline
* Enrich events with operational metadata
* Forward processed events to Splunk
* Validate data ingestion and searchability
* Demonstrate practical Cribl administration skills



## 🛠 Environment

| Component         | Version                |
| ----------------- | ---------------------- |
| Cribl Stream      | Community Edition      |
| Splunk Enterprise | 10.x                   |
| Operating System  | Kali Linux             |
| Source Type       | System Metrics         |
| Destination       | Splunk Single Instance |



## 📥 Source Configuration

### Source

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

The source continuously collects CPU, Memory, Network, Disk, and Host metrics from the local system.


## 🚦 Route Configuration

### Route Name

```text
Metrics_Optimization_Route
```

### Filter

```javascript
true
```

### Pipeline

```text
Metrics_Optimization
```

### Destination

```text
splunk:splunk_single_instance
```

### Final

```text
Enabled
```

This route ensures all events from the source are processed through the Metrics Optimization pipeline before being forwarded to Splunk.


## ⚙️ Pipeline Configuration

### Function Used

```text
Eval
```

### Added Metadata

```javascript
environment = "lab"
project     = "cribl-metrics-optimization"
owner       = "soc-team"
```

### Purpose

The pipeline enriches incoming events with metadata that improves searchability and provides operational context.

Example:

```json
{
  "host": "kali",
  "environment": "lab",
  "project": "cribl-metrics-optimization",
  "owner": "soc-team"
}
```

## 🔍 Splunk Validation

### Verify Data Ingestion

```spl
index=*
project="cribl-metrics-optimization"
```

### View Events by Host

```spl
index=*
project="cribl-metrics-optimization"
| stats count by host
```

### View Events by Environment

```spl
index=*
project="cribl-metrics-optimization"
| stats count by environment
```


## 📊 Results

The project successfully demonstrates:

* Cribl Source Management
* Cribl Routing
* Pipeline Processing
* Event Enrichment
* Splunk Data Ingestion
* Metadata-Based Searching

Processed events are enriched before reaching Splunk, allowing analysts to search and filter data more efficiently.


## 🧠 Skills Demonstrated

### Cribl Stream

* Source Configuration
* Route Management
* Pipeline Development
* Event Enrichment
* Destination Configuration
* Troubleshooting Data Flow

### Splunk

* Data Validation
* Search Development
* Event Analysis
* Metadata Correlation


## 🔮 Future Improvements

* Filter unnecessary metrics before indexing
* Reduce Splunk ingest volume
* Create separate routes for CPU, Memory, and Disk metrics
* Implement HEC-based forwarding
* Create Splunk dashboards for infrastructure monitoring
* Add threshold-based alerting


### Key Technologies

* Cribl Stream
* Splunk Enterprise
* Linux
* Data Pipelines
* Event Processing
* Security Operations
