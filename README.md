# Patient Vital Monitoring System

A real-time patient vital signs monitoring system built with Apache Beam and Google Cloud Platform. This system implements a streaming medallion architecture (Bronze-Silver-Gold) for processing patient health data with automatic error detection and risk assessment.

## 📊 Data Architecture Visualization

![Data Flow Architecture](data/Visual_Data.png)

## 🏗️ Architecture Overview

This project implements a **Medallion Architecture** for real-time patient data processing:

- **Bronze Layer**: Raw data ingestion from Pub/Sub, stored as-is in Google Cloud Storage
- **Silver Layer**: Cleaned and validated data with quality checks and enrichment
- **Gold Layer**: Aggregated metrics per patient stored in BigQuery for analytics

## 🌟 Features

- **Real-time Data Simulation**: Generates realistic patient vital signs with configurable error injection
- **Stream Processing**: Apache Beam pipeline for continuous data processing
- **Data Quality**: Automated validation and filtering of invalid records
- **Risk Assessment**: Real-time risk scoring based on vital signs
- **Scalable Architecture**: Built on GCP services for production-grade scalability
- **Error Handling**: Intentional error injection for testing data quality pipelines

## 📁 Project Structure

```
Patient-Vital-Monitoring/
├── data/
│   └── Visual_Data.png          # Architecture visualization
├── dataflow/
│   ├── streaming_medallion_pipeline.py  # Main Apache Beam pipeline
│   └── .env                      # GCP configuration for pipeline
├── simulator/
│   ├── patient_vitals_simulator.py     # Patient data simulator
│   └── .env                      # GCP configuration for simulator
└── README.md
```

## 🚀 Getting Started

### Prerequisites

- Python 3.7+
- Google Cloud Platform account
- Apache Beam
- Google Cloud Pub/Sub topic and subscription
- Google Cloud Storage bucket
- BigQuery dataset

### Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd Patient-Vital-Monitoring
```

2. Install required dependencies:
```bash
pip install apache-beam[gcp] google-cloud-pubsub python-dotenv
```

3. Set up environment variables:

Create `.env` files in both `simulator/` and `dataflow/` directories:

**simulator/.env**:
```env
GCP_PROJECT=your-project-id
PUBSUB_TOPIC=your-topic-name
PATIENT_COUNT=20
STREAM_INTERVAL=2
ERROR_RATE=0.1
```

**dataflow/.env**:
```env
GCP_PROJECT=your-project-id
PUBSUB_SUBSCRIPTION=your-subscription-name
BRONZE_PATH=gs://your-bucket/bronze/
SILVER_PATH=gs://your-bucket/silver/
BIGQUERY_TABLE=your-project:dataset.table
TEMP_LOCATION=gs://your-bucket/temp
STAGING_LOCATION=gs://your-bucket/staging
REGION=us-central1
```

### Running the System

1. **Start the data simulator**:
```bash
cd simulator
python patient_vitals_simulator.py
```

2. **Run the streaming pipeline**:
```bash
cd dataflow
python streaming_medallion_pipeline.py
```

## 📊 Data Model

### Patient Vital Signs

- `patient_id`: Unique patient identifier (P001-P020)
- `timestamp`: ISO 8601 timestamp
- `heart_rate`: Heart rate in BPM (60-120)
- `spo2`: Blood oxygen saturation (90-100%)
- `temperature`: Body temperature in Celsius (36-39°C)
- `bp_systolic`: Systolic blood pressure (100-140 mmHg)
- `bp_diastolic`: Diastolic blood pressure (60-90 mmHg)

### Risk Assessment

The system calculates a `risk_score` based on vital signs and assigns a `risk_level`:
- **Low**: Risk score < 0.3
- **Moderate**: Risk score 0.3-0.6
- **High**: Risk score > 0.6

## 🔧 Configuration

### Error Injection

The simulator includes configurable error injection for testing:
- Missing fields
- Negative values
- Out-of-range values

Configure via `ERROR_RATE` in simulator/.env (default: 0.1 = 10% error rate)

### Data Validation Rules

The pipeline validates:
- All required fields are present
- SpO2: 0 < value ≤ 100
- Heart Rate: 0 < value < 200
- Temperature: 30 ≤ value ≤ 45

## 📈 Monitoring and Analytics

Query aggregated data in BigQuery:
```sql
SELECT 
  patient_id,
  avg_heart_rate,
  avg_spo2,
  avg_temperature,
  max_risk_level
FROM `your-project.dataset.table`
WHERE max_risk_level = 'High'
ORDER BY avg_heart_rate DESC;
```

## 🛠️ Technology Stack

- **Apache Beam**: Stream processing framework
- **Google Cloud Pub/Sub**: Message streaming service
- **Google Cloud Storage**: Data lake storage
- **Google BigQuery**: Analytics database
- **Python**: Primary programming language

## 📝 License

This project is open source and available for educational purposes.

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📧 Contact

For questions or support, please open an issue in the repository.
