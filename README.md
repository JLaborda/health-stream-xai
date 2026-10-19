# 🏥 Health Stream XAI: Auditable Real-Time MLOps Pipeline

This repository implements a production-grade, event-driven Machine Learning architecture designed for high-stakes, regulated environments (Healthtech). It simulates the real-time ingestion of clinical telemetry (MIMIC-IV) to predict critical patient risk while ensuring strict regulatory compliance, model explainability, and self-healing capabilities.

## 🎯 Architectural Goals
Deploying ML in healthcare requires more than just high accuracy; it demands auditability, resilience to changing data, and strict temporal data boundaries. This project tackles the three major bottlenecks of production ML:

1. **The "Black Box" Problem:** Models in regulated sectors must justify their decisions. This system integrates an **Explainable AI (XAI) engine** that computes SHAP values on-the-fly only when a high-risk threshold is breached, maintaining low-latency inference while providing actionable clinical context.
2. **Silent Model Degradation:** Patient populations and sensor calibrations change over time. An automated **Data Drift detection loop** monitors the statistical distribution of incoming telemetry against the training baseline, triggering alerts before the model performance drops.
3. **Temporal Data Leakage:** Re-training models on time-series data often leads to "future leakage". The online learning pipeline implements strict **Point-in-Time joins**, ensuring the model only learns from lab results and vitals that were physically available *at the exact moment* of the historical prediction.

## ⚙️ Tech Stack & Microservices
* **Event Streaming:** Apache Kafka / Redpanda (Producer simulating IoT sensor streams).
* **Serving Layer:** FastAPI & Uvicorn for low-latency ML inference.
* **Explainability:** `shap` for real-time feature contribution analysis.
* **Model Monitoring:** EvidentlyAI for automated concept and data drift detection.
* **Orchestration & Tooling:** Docker Compose, `uv` for ultra-fast dependency management, and MLflow for experiment tracking.
