# Apache-Kafka-End-to-End-Streaming-Pipeline-2026-
This repository contains my first Apache Kafka streaming pipeline, built to simulate a real-world e-commerce event processing system.

## Architecture Overview

Producer (Python) → Kafka Topics → PySpark Structured Streaming (Databricks) → Kafka Topics

| Topic Name        | Purpose                         |
| ----------------- | ------------------------------- |
| `orders_raw`      | Raw order events                |
| `orders_enriched` | Cleaned & enriched orders       |
| `category_sales`  | Aggregated category-level sales |
| `orders_flagged`  | Fraud-suspected orders          |
| `user_events`     | User interaction events         |
| `order_user_join` | Joined order + user events      |

## Pipeline Flow
1️⃣ Orders Ingestion

Python producer streams order data to orders_raw

2️⃣ Orders Enrichment

PySpark consumer reads orders_raw

Adds:

total_amount = quantity * price

processing_time

Filters order_status = 'CREATED'

Writes to orders_enriched

3️⃣ Category Aggregation

Reads orders_enriched

Calculates total orders per category

Writes results to category_sales

Kafka message key = category

4️⃣ Fraud Detection

Rules applied on orders_enriched:

total_amount > 50000

OR payment_mode = COD AND category = electronics

Writes flagged orders to orders_flagged

5️⃣ Stream Join

user_events produced via Python

Joined with orders_enriched

## Tech Stack

Apache Kafka

Python (Kafka Producer)

PySpark Structured Streaming

Databricks

Kafka Topics as Streaming Sinks

Output written to order_user_join

