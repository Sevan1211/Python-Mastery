# Data — Learning Path

**MANDATORY: Complete modules in this exact order. Do not skip ahead.**

Each module must be fully completed and mastered before moving to the next. Earlier modules are prerequisites for later ones.

**Prerequisite**: Complete Fundamentals through at least module 11 (Common Built-ins). Backend modules 1-5 recommended.

---

## Module Order (Easiest → Hardest)

### Phase 1: Data Manipulation Foundations
*The tools you'll use every single day as a data engineer.*

| # | Module | Folder | Prerequisites | Why This Order |
|---|--------|--------|---------------|----------------|
| 1 | **NumPy** | `numpy/` | Fundamentals: Lists, Functions, Loops | Array creation, indexing, slicing, broadcasting, vectorized operations. The numerical foundation under pandas and ML. |
| 2 | **Pandas Core** | `pandas/` | NumPy | Series, DataFrame, indexing (loc/iloc), filtering, basic operations. The #1 tool for data manipulation. |
| 3 | **Data Cleaning** | `data_cleaning/` | Pandas Core | Missing values, duplicates, type casting, string cleaning, outlier handling. Real data is always messy. |
| 4 | **Joins / GroupBy / Window** | `joins_groupby_window/` | Data Cleaning | merge, join, groupby, agg, transform, window functions. The analytical patterns that power reporting and features. |

### Phase 2: Data Engineering Core
*Building pipelines that move and transform data reliably.*

| # | Module | Folder | Prerequisites | Why This Order |
|---|--------|--------|---------------|----------------|
| 5 | **ETL with Python** | `etl/` | Joins/GroupBy/Window | Extract-Transform-Load patterns, pipeline architecture, idempotency, error handling. The core DE workflow. |
| 6 | **Data Modeling** | `data_modeling/` | ETL | Star schema, snowflake, fact/dimension tables, normalization, SCD. How to structure data for analytics. |
| 7 | **Orchestration** | `orchestration/` | Data Modeling | Airflow concepts, DAGs, scheduling, dependencies, retries. How pipelines run in production. |

### Phase 3: Scale & Quality
*Working with big data and ensuring data is trustworthy.*

| # | Module | Folder | Prerequisites | Why This Order |
|---|--------|--------|---------------|----------------|
| 8 | **PySpark** | `pyspark/` | Orchestration, Pandas Core | Distributed DataFrames, transformations, actions, partitioning. When pandas can't handle the scale. |
| 9 | **Warehouse Patterns** | `warehouse_patterns/` | PySpark, Data Modeling | Staging, incremental loads, slowly changing dimensions, materialized views. Production warehouse architecture. |
| 10 | **Data Quality** | `data_quality/` | Warehouse Patterns | Great Expectations, schema validation, freshness checks, anomaly detection. Trust your data. |

### Phase 4: ML Foundations & Feature Engineering
*The bridge between data engineering and ML/AI systems.*

| # | Module | Folder | Prerequisites | Why This Order |
|---|--------|--------|---------------|----------------|
| 11 | **ML Fundamentals** | `ml_fundamentals/` | Data Quality, NumPy, Pandas | Supervised/unsupervised, train/test/val, metrics, basic models. Understand what ML systems need from data. |
| 12 | **Feature Engineering** | `feature_engineering/` | ML Fundamentals | Feature extraction, encoding, scaling, feature stores. Building the inputs ML models consume. |
| 13 | **Experiment Tracking** | `experiment_tracking/` | Feature Engineering | MLflow, experiment logging, model versioning, artifact management. Track what you train. |

### Phase 5: ML Infrastructure & Production
*Deploying and operating ML systems at scale.*

| # | Module | Folder | Prerequisites | Why This Order |
|---|--------|--------|---------------|----------------|
| 14 | **Model Pipelines** | `model_pipelines/` | Experiment Tracking | Training pipelines, preprocessing steps, pipeline serialization. Reproducible ML workflows. |
| 15 | **Model Serving** | `model_serving/` | Model Pipelines, Backend: FastAPI | REST inference APIs, batch prediction, latency optimization. Getting models into production. |
| 16 | **MLOps** | `mlops/` | Model Serving | CI/CD for ML, model registries, automated retraining, A/B testing. Operating ML in production. |
| 17 | **Batch vs Streaming** | `batch_vs_streaming/` | MLOps, Orchestration | Batch ETL vs streaming (Kafka, Flink concepts), lambda/kappa architecture. Choosing the right processing model. |
| 18 | **Monitoring / Drift** | `monitoring_drift/` | Batch vs Streaming | Data drift, model drift, performance monitoring, alerting. Keeping ML systems healthy over time. |

---

## Gate Rules

Before advancing from module N to module N+1:

- [ ] All DataFrame transformation drills completed
- [ ] ETL mini pipeline working end-to-end (where applicable)
- [ ] Debugging broken pipeline task completed
- [ ] Performance optimization task attempted
- [ ] Data quality tests written
- [ ] Can explain production architecture decisions from memory
- [ ] Agent has updated `mastery_score.md` and `progress.md`

## Current Position

**Prerequisite**: Complete Fundamentals first.
**Active Module**: Not started
