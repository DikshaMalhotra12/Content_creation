
#  Solution: Retail Data Transformation Pipeline Using DBT and Prefect

## Overview
This solution implements an automated and reproducible data pipeline using **DBT** for data transformation and **Prefect** for orchestration. It processes the [Global Super Store dataset](https://www.kaggle.com/datasets/apoorvaappz/global-super-store-dataset) to enable analytics for a retail business.

---

## 1. Environment Setup

### 1.1 Create Virtual Environment (optional)
```bash
python -m venv env
source env/bin/activate  # On Windows: env\Scripts\activate
```

### 1.2 Install Required Packages
```bash
pip install pandas sqlalchemy prefect dbt-core dbt-postgres
```

---

## 2. Load Raw Data into SQLite/Postgres

We ingest the CSVs (`Orders.csv`, `Returns.csv`, `People.csv`) into a database (SQLite or PostgreSQL) using Python and SQLAlchemy.

```python
import pandas as pd
from sqlalchemy import create_engine

# Use SQLite for simplicity
engine = create_engine("sqlite:///global_super_store.db")

# Load data
orders = pd.read_csv("Orders.csv")
returns = pd.read_csv("Returns.csv")
people = pd.read_csv("People.csv")

# Write to database
orders.to_sql("raw_orders", engine, if_exists="replace", index=False)
returns.to_sql("raw_returns", engine, if_exists="replace", index=False)
people.to_sql("raw_people", engine, if_exists="replace", index=False)
```

---

## 3. Create DBT Project

### 3.1 Initialize Project
```bash
dbt init global_store_dbt
```

### 3.2 Configure Profile (`~/.dbt/profiles.yml`)
Example for SQLite:
```yaml
global_store_dbt:
  target: dev
  outputs:
    dev:
      type: sqlite
      threads: 1
      database: global_super_store.db
```

---

## 4. Create Models in DBT

### 4.1 Sources (in `models/src_raw.yml`)
```yaml
version: 2
sources:
  - name: raw
    schema: main
    tables:
      - name: raw_orders
      - name: raw_returns
```

### 4.2 Staging Models (in `models/staging/stg_orders.sql`)
```sql
select
  "Order ID" as order_id,
  "Order Date" as order_date,
  "Customer ID" as customer_id,
  "Sales" as sales,
  "Region" as region
from {{ source('raw', 'raw_orders') }}
```

### 4.3 Fact and Dimension Models
**`models/marts/fact_sales.sql`**
```sql
select
  order_id,
  customer_id,
  sum(sales) as total_sales
from {{ ref('stg_orders') }}
group by 1, 2
```

**`models/marts/dim_customers.sql`**
```sql
select distinct
  customer_id,
  region
from {{ ref('stg_orders') }}
```

---

## 5.  Run and Test DBT Models

```bash
dbt run
dbt test
```

---

## 6. Prefect Orchestration

### 6.1 Define Tasks in Python
```python
from prefect import flow, task
import subprocess
import pandas as pd
from sqlalchemy import create_engine

@task
def load_data():
    engine = create_engine("sqlite:///global_super_store.db")
    pd.read_csv("Orders.csv").to_sql("raw_orders", engine, if_exists="replace", index=False)
    pd.read_csv("Returns.csv").to_sql("raw_returns", engine, if_exists="replace", index=False)
    pd.read_csv("People.csv").to_sql("raw_people", engine, if_exists="replace", index=False)

@task
def run_dbt():
    subprocess.run(["dbt", "run"], check=True)

@task
def test_dbt():
    subprocess.run(["dbt", "test"], check=True)

@flow
def global_store_etl_flow():
    load_data()
    run_dbt()
    test_dbt()

if __name__ == "__main__":
    global_store_etl_flow()
```

---

## 7. 📊 Monitoring and Logs
- Use `prefect orion start` to launch the Prefect UI.
- Register and monitor flow runs.
- Enable notifications, retries, and alerts via Prefect decorators or Cloud (optional).

---

## 8. Output Deliverables

| Output                  | Description |
|------------------------|-------------|
| `fact_sales`           | Aggregated sales per order and customer |
| `dim_customers`        | Unique customer-region mapping |
| DBT Test Results       | Quality checks on nulls, duplicates, valid values |
| Prefect Logs & DAG     | Complete orchestration trace for each run |

---

## Conclusion

By combining **DBT's modular SQL transformations** with **Prefect's modern orchestration**, we created a reliable, scalable, and observable data pipeline tailored for retail analytics. This setup ensures:
- Reproducibility
- Fast troubleshooting
- Business-ready insights at scale

You can extend this pipeline to schedule daily updates, integrate with a BI tool, or migrate from SQLite to cloud-based data warehouses like Snowflake, BigQuery, or Redshift.

---
