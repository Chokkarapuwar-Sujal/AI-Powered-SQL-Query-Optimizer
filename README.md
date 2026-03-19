
# AI-Powered Database Query Optimization & Index Recommendation System

[![Live Demo](https://img.shields.io/badge/Live%20Demo-Streamlit-red?logo=streamlit)](https://ai-powered-sql-query-optimizer.streamlit.app/)

![Python](https://img.shields.io/badge/Python-3.11+-blue?logo=python)
![Streamlit](https://img.shields.io/badge/Streamlit-1.32+-red?logo=streamlit)
![Plotly](https://img.shields.io/badge/Plotly-5.18+-purple?logo=plotly)
![sqlparse](https://img.shields.io/badge/sqlparse-0.4.4-green)

This project is a Streamlit-based SQL analysis tool that detects query anti-patterns and recommends optimizations. It combines pattern detection, scoring, index recommendations, rewrite suggestions, and interactive visualization in one dashboard.

`db_connection.py` is included for optional MySQL integration. The core analysis, scoring, planning, and simulation workflows work without a live database.

---

## Requirements

To run this project locally, you need:

- Python 3.11+
- pip
- Streamlit
- Plotly
- Pandas
- sqlparse
- mysql-connector-python
- SQLAlchemy
- PyMySQL

---

## Quick Start

```bash
pip install -r requirements.txt
streamlit run app.py
```

Open: [http://localhost:8501](http://localhost:8501)

You can paste your own SQL query or select one from the sample dataset.

---

## What the Project Does

The app lets you:

- Analyze SQL queries.
- Classify query complexity.
- Assign a performance score.
- Recommend indexes.
- Visualize an EXPLAIN-style plan.
- Rewrite inefficient SQL.
- Simulate optimization impact.
- Track query history.
- Export reports as JSON, CSV, or TXT.

---

## Features (18)

| # | Feature | Description |
|---|---|---|
| 1 | SQL Query Input | Accepts `SELECT`, `JOIN`, subqueries, and aggregation queries |
| 2 | Query Analyzer | Detects inefficient SQL patterns automatically |
| 3 | Optimization Engine | Suggests improved SQL with examples |
| 4 | Performance Score | Assigns a 0-100 performance score |
| 5 | Complexity Detection | Classifies queries as Simple, Moderate, or Complex |
| 6 | Index Recommendation | Generates `CREATE INDEX` suggestions |
| 7 | Cost Estimation | Predicts runtime cost as LOW / MEDIUM / HIGH |
| 8 | Performance Charts | Visual analytics using Plotly |
| 9 | Query History | Tracks analyzed queries and score trends |
| 10 | Sample Dataset | Includes 25 annotated test queries |
| 11 | AI Insights | Generates natural-language explanations |
| 12 | Optimization Simulation | Estimates improved query score |
| 13 | Best Practices Panel | Displays SQL performance guidelines |
| 14 | Architecture Viewer | Shows system flow in the app |
| 15 | Export Reports | JSON, CSV, TXT export |
| 16 | Execution Plan Visualizer | Interactive EXPLAIN-style plan tree |
| 17 | Index Impact Simulator | Before/after performance comparison |
| 18 | Query Rewrite Engine | Rewrites inefficient SQL into better alternatives |

---

## Project Structure

```text
AI-Powered SQL Query Optimization & Index Recommendation Engine/
|
|-- app.py
|-- db_connection.py
|-- analyzer.py
|-- optimizer.py
|-- scoring.py
|-- recommendations.py
|-- execution_plan.py
|-- simulator.py
|-- rewrite_engine.py
|-- requirements.txt
|-- README.md
|
|-- data/
|   |-- sample_queries.csv
|
|-- utils/
    |-- __init__.py
    |-- helpers.py
```

---

## System Flow

```text
SQL Query Input
      -> Query Analyzer
      -> Scoring Engine
      -> Optimization + Index Suggestions
      -> Execution Plan Simulation
      -> Rewrite + Impact Simulation
      -> Dashboard Visuals + Export
```


## Setup and Installation

### Prerequisites

- Python 3.11+
- pip
- MySQL server (optional, only if you want to test DB connectivity)

### 1. Clone the Repository

```bash
git clone https://github.com/Chokkarapuwar-Sujal/AI-Powered-SQL-Query-Optimizer.git
cd AI-Powered-SQL-Query-Optimizer
```

### 2. Create and Activate a Virtual Environment

Windows (PowerShell):

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

macOS/Linux:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Run the App

```bash
streamlit run app.py
```

### 5. If `streamlit` Is Not Recognized

```bash
python -m streamlit run app.py
```

---

## Optional MySQL Setup

Create a sample database:

```sql
CREATE DATABASE company_db;
USE company_db;

CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    department VARCHAR(100),
    salary INT,
    hire_date DATE
);

INSERT INTO employees VALUES
(1, 'John', 'Engineering', 90000, '2022-01-10'),
(2, 'Alice', 'HR', 60000, '2021-02-15'),
(3, 'Bob', 'Engineering', 95000, '2023-03-12'),
(4, 'Eve', 'Finance', 80000, '2022-04-18');
```

Configure `db_connection.py`:

```python
import mysql.connector

def connect_db():
    connection = mysql.connector.connect(
        host="localhost",
        user="root",
        password="yourpassword",
        database="company_db"
    )
    return connection
```

---

## Module Overview

### `analyzer.py`

- Detects `SELECT *`, missing `WHERE`, JOIN counts, nested subqueries, missing `LIMIT`, wildcard `LIKE`, and function-on-column patterns.
- Extracts filter columns and JOIN columns.

### `scoring.py`

- Applies 16 scoring rules (penalties + bonuses).
- Returns score, cost tier, rows estimate, and score breakdown.

### `optimizer.py`

- Generates prioritized optimization recommendations (HIGH/MEDIUM/LOW).
- Produces natural-language insight summaries.

### `recommendations.py`

- Extracts table-column pairs from `WHERE`/`ON` clauses.
- Suggests B-tree, composite, covering, and full-text index patterns.

### `execution_plan.py`

- Builds a simulated EXPLAIN-style tree with PostgreSQL-inspired cost model.
- Provides node flattening and plan summary utilities.

### `simulator.py`

- Estimates before/after metrics for score, cost, rows, and time.
- Calculates speedup factor and impact level.

### `rewrite_engine.py`

- Applies automatic rewrites such as:
  - `IN` subquery to `JOIN`
  - `SELECT *` expansion
  - function-on-column rewrites
  - leading wildcard annotation
  - `LIMIT` injection

### `utils/helpers.py`

- SQL formatting, badge helpers, color helpers, report export builders, and history records.

---

## Scoring Reference

| Rule | Type | Delta |
|---|---|---|
| `SELECT *` used | Penalty | -25 |
| Missing `WHERE` clause | Penalty | -20 |
| Excessive JOINs (>2) | Penalty | -15 |
| JOIN detected | Penalty | -10 |
| Nested subquery | Penalty | -10 |
| No `LIMIT` clause | Penalty | -10 |
| Leading wildcard `LIKE` | Penalty | -10 |
| Function on `WHERE` column | Penalty | -10 |
| Aggregate without filter | Penalty | -10 |
| `SELECT DISTINCT` with JOIN | Penalty | -5 |
| `WHERE` present | Bonus | +10 |
| `LIMIT` present | Bonus | +10 |
| Specific columns selected | Bonus | +10 |
| Filter columns identified | Bonus | +5 |
| `GROUP BY` used | Bonus | +5 |
| `ORDER BY` used | Bonus | +5 |

Cost tiers:

| Score Range | Cost Estimate | Rows Scanned |
|---|---|---|
| 80-100 | LOW | ~1K-10K rows |
| 50-79 | MEDIUM | ~10K-500K rows |
| 0-49 | HIGH | ~1M+ rows |

---

## Export Formats

| Format | Purpose |
|---|---|
| JSON | Structured output for programmatic use |
| CSV | Spreadsheet-friendly export |
| TXT | Plain-text report |

Example JSON output:

```json
{
  "query": "SELECT * FROM orders WHERE customer_id = 10",
  "score": 65,
  "optimized_score": 95,
  "issues": ["SELECT * usage"],
  "recommendations": ["Add index on customer_id"]
}
```

---

## Troubleshooting

- `localhost` not opening:
  - Confirm server is running: `streamlit run app.py`
  - Try `http://127.0.0.1:8501`
- Port already in use:
  - `streamlit run app.py --server.port 8502`
- MySQL connection fails:
  - Verify MySQL service is running.
  - Verify `db_connection.py` credentials.

---



