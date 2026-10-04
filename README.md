# FinGuard — Real-Time Credit Card Fraud Detection Platform

FinGuard is a real-time fraud detection solution designed to simulate credit card transactions, ingest them through Kafka, process them in a Databricks Lakehouse pipeline, and surface suspicious activity through alerts and a monitoring dashboard.

<p align="center">
  <img src="./assets/IMG_0070.PNG" alt="FinGuard platform architecture" width="1200" />
</p>

## What this project does

The project creates an end-to-end fraud monitoring workflow for financial transactions:

- Generates realistic customer, merchant, and transaction activity
- Simulates both normal and fraudulent payment behavior
- Publishes transaction events to Kafka in near real time
- Processes raw messages through a bronze, silver, and gold medallion architecture
- Detects suspicious patterns such as watchlist matches and high-risk transaction behavior
- Sends alerts and exposes a dashboard for monitoring fraud activity

## Dashboard

A fraud monitoring dashboard was created to visualize suspicious transaction trends and alert activity.

- Dashboard PDF: [FinGuard Dashboard PDF](./assets/FinGuard-Dashboard.pdf)

## Key Features

- Real-time credit card transaction simulation
- Kafka-based event streaming
- Databricks structured streaming with a medallion architecture
- Fraud watchlist matching and rule-based risk scoring
- High-value and card-based fraud alert notifications
- Gold-layer dashboards and operational monitoring
- Secure configuration using environment variables and secret-scoped access for production workloads

## Architecture

The project is split into two major parts:

1. Kafka producer layer
   - Generates realistic customer, merchant, and transaction data
   - Simulates normal and fraudulent activity
   - Publishes JSON payloads to a Kafka topic

2. Databricks streaming pipeline
   - Reads Kafka topic data in the bronze layer
   - Cleans and enriches records in silver tables
   - Applies fraud logic and aggregations in gold tables
   - Sends real-time alerts and updates downstream systems

## Repository Structure

```text
FinGuard/
├── README.md
├── assets/
│   ├── IMG_0070.PNG
│   └── FinGuard-Dashboard.pdf
├── FinGuard/                  # Python virtual environment
├── kafka_producer/            # Transaction generator and Kafka integration
│   ├── config.py
│   ├── consumer.py
│   ├── customer_generator.py
│   ├── data/
│   ├── fraud_engine.py
│   ├── merchant_generator.py
│   ├── models.py
│   ├── producer_fraud_card.py
│   ├── producer_fraud_transaction.py
│   ├── producer_normal.py
│   ├── requirements.txt
│   ├── transaction_generator.py
│   ├── update_csv_email.py
│   └── utils.py
├── finguard_project/          # Databricks / streaming project files
└── .git/
```

## Getting Started

### Prerequisites

- Python 3.10+
- Kafka access credentials or a local test cluster
- Databricks workspace access for streaming pipeline deployment
- Optional: Gmail SMTP credentials for alerting

### 1. Activate the environment

If you are using the included environment:

```bash
source FinGuard/bin/activate
```

Or create your own local environment:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 2. Install dependencies

```bash
cd kafka_producer
pip install -r requirements.txt
```

### 3. Configure Kafka settings

Create a `.env` file in the `kafka_producer` folder with values similar to:

```env
BOOTSTRAP_SERVERS=your-bootstrap-servers:9092
API_KEY=your_api_key
API_SECRET=your_api_secret
TOPIC_NAME=credit_card_transactions
TRANSACTIONS_PER_SECOND=5
FRAUD_PERCENTAGE=0.08
TOTAL_CUSTOMERS=1000
TOTAL_MERCHANTS=200
RANDOM_SEED=42
```

### 4. Run the producer

```bash
cd kafka_producer
python producer_normal.py
```

You can also run specialized fraud producers:

```bash
python producer_fraud_transaction.py
python producer_fraud_card.py
```

## Streaming Pipeline Components

The pipeline under the Databricks project includes:

- `bronze/` for raw Kafka or Auto Loader ingestion
- `silver/` for cleansing, enrichment, and deduplication
- `gold/` for alerting logic and aggregate metrics
- `alert/` for email notifications and downstream actions

Examples of implemented logic include:

- fraud watchlist matching
- transaction velocity detection
- high-value transaction alerting
- golden table aggregation by minute
- email notifications for suspicious activity

## Notes

This repository contains a practical implementation of a real-time fraud monitoring design using Kafka, Spark Structured Streaming, and Databricks Lakehouse components. It is well suited for demonstration, proof-of-concept, and learning scenarios.

## License

This project is intended for educational and demonstration purposes. Update the license if you plan to publish or distribute it externally.
