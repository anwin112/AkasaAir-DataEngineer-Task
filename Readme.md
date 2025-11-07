# 🧾 Akasa Air Data Engineering Task – Secure Data Analysis Pipeline

### **Project Type:** Data Analysis & Reporting  
### **Tech Stack:** Python, Pandas, SQLite, ipywidgets, dotenv, Matplotlib  
### **Author:** Anwin K Biju  
### **Organization:** Akasa Air (Data Engineer Interview Task)  
### **Date:** 07-11-November 2025

---

##  1. Project Overview

This notebook implements a **secure, modular, and auditable data pipeline** that generates **daily and monthly customer order reports**.

It simulates a real-world data engineering workflow using two main data sources:

1. **Customer Master File (CSV/XLSX)** – securely uploaded through a credential-gated widget.  
2. **Orders Data (SQL/XML)** – fetched using SQL queries and loaded into a DataFrame.

The pipeline integrates:
- Multi-stage **security checks** (Stage-1 and Stage-2 authentication)
- Automated **data sanity validations**
- **Daily historical archiving**
- **KPI generation** (Repeat Customers, Regional Revenue, Top Spenders, Trends)
- Controlled **Excel report export** after authorization

---

##  2. Design Overview

| Stage | Functionality | Description |
|--------|----------------|--------------|
| **Stage-1 Input Security** | Credential-based upload | Credentials validated from `.env` before allowing customer file ingestion |
| **Data Sanity Checks** | Schema enforcement | Auto-normalizes column names, validates required fields |
| **Order Data Fetching** | SQL Query → DataFrame | Uses SQLite demo; replaceable with PostgreSQL/MySQL |
| **Temporal Segmentation** | Daily & historical split | Current-day used for analysis, previous-day archived securely |
| **KPI Engine** | Modular, reusable functions | Independent KPIs: repeat customers, revenue, top spenders |
| **Stage-2 Output Security** | Export authorization | Separate credentials required for report generation |
| **History Management** | `/data_history/YYYY-MM-DD/` | Stores daily snapshots for auditing & reproducibility |
| **Monthly Aggregation (Optional)** | Aggregates daily snapshots | Produces month-to-date summaries |

---

##  3. Implementation Breakdown

### **Cell 1 – Imports & Config**
- Imports pandas, matplotlib, sqlite3, ipywidgets, dotenv
- Loads environment variables (`.env`)
- Defines constants (file size, allowed extensions, required columns)

### **Cell 2–3 – Stage-1 Secure File Upload**
- Interactive widget for credential entry and file upload  
- Reads credentials securely from `.env`  
- Validates file name (`task_DE_new_customers.csv`), type, size, and schema  
- Normalizes column names (e.g., `Customer Name` → `customer_name`)

### **Cell 4–5 – Orders Data Fetch**
- Creates and populates demo **SQLite DB**
- Fetches orders via SQL into a pandas DataFrame  
- Ensures correct column types, date parsing, and normalization

### **Cell 6 – History Archiving**
- Splits orders into:
  - `temp_previous_day_df` → archived for security
  - `current_day_orders_df` → used for daily analysis  
- Saves daily history snapshots to `/data_history/YYYY-MM-DD/`

### **Cell 7–9 – Data Merge & KPI Computation**
- Merges customer and order data (`mobile_number` key)
- Computes KPIs:
  - Repeat Customers  
  - Daily Order Trend  
  - Regional Revenue  
  - Top Spenders (30-day window)

### **Cell 10–12 – Stage-2 Export Security**
- Prompts for export credentials (`EXPORT_USER`, `EXPORT_PASS` from `.env`)
- On authorization, triggers **Excel export** containing KPIs & merged data

### **Cell 13 – Optional Archive**
- Archives previous-day orders to `/archive_previous_day/` for retention and audits

### **Cell 14–15 – Documentation & Monthly Aggregation**
- Includes production hardening checklist  
- Optional monthly KPI aggregation from `/data_history/`

---

## ▶️ How to Run the Notebook

###  Step 1: Enable Jupyter Widgets
Before running the notebook, ensure that `ipywidgets` are enabled.

jupyter nbextension enable --py widgetsnbextension


* If not installed:
pip install ipywidgets

### * Step 2: Create the .env File
In your project root directory, create a file named .env with the following contents:

DATA_PIPELINE_USER=data_pipeline_user
DATA_PIPELINE_PASS=Pssw0rd123@
EXPORT_USER=analyst_user
EXPORT_PASS=AnalystPass

Note: The .env file must be in the same folder as the notebook to be detected by load_dotenv().

### *Step 3: Run the Notebook

Open the notebook and execute all cells in order.

-In Stage-1:

    .Enter credentials (from .env)

    .Upload the file named task_DE_new_customers.csv

-Orders data is automatically fetched via SQL queries.

-Inline KPI tables and visualizations will appear.

-In Stage-2, authorize export → Excel report is generated.

### *Step 4: Output Files
| Type              | Location                                     | Description                           |
| ----------------- | -------------------------------------------- | ------------------------------------- |
| **Excel Report**  | `/exports/Customer_Order_Report_<date>.xlsx` | Authorized KPI report                 |
| **Daily History** | `/data_history/YYYY-MM-DD/`                  | Snapshot of daily data                |
| **Archived Data** | `/archive_previous_day/`                     | Previous-day orders archived securely |


### 5. KPIs Produced
| KPI                        | Description                                           |
| -------------------------- | ----------------------------------------------------- |
| **Repeat Customers**       | Customers with more than one order in the current day |
| **Daily Order Trend**      | Number of orders per day (trend metric)               |
| **Regional Revenue**       | Sum of total revenue grouped by region                |
| **Top Spenders (30 Days)** | Customers ranked by total spend over the last 30 days |

### 6. Security Layers
| Layer                      | Description                                                    |
| -------------------------- | -------------------------------------------------------------- |
| **Stage-1 Authentication** | Validates user credentials before allowing file upload         |
| **Stage-2 Authentication** | Requires separate export credentials before generating reports |
| **SQL Security**           | Parameterized queries prevent SQL injection                    |
| **Secret Management**      | Credentials securely stored in `.env`, never hardcoded         |
| **Audit Trail**            | Daily snapshots and archived backups for audit compliance      |

### 7. Potential Enhancements

| Area                     | Improvement                                                      |
| ------------------------ | ---------------------------------------------------------------- |
| **Database Integration** | Replace SQLite with PostgreSQL/MySQL via SQLAlchemy              |
| **Automation**           | Schedule daily execution using Airflow or Prefect                |
| **Dashboards**           | Add interactive visualization using Plotly or Streamlit          |
| **Notifications**        | Send automated daily KPI summary via email or Slack              |
| **Testing**              | Implement `pytest` for schema, auth, and KPI tests               |
| **Audit Logging**        | Add JSON-based audit logs for upload, export, and archive events |
| **Role Management**      | Integrate with IAM (Okta/Azure AD) for user-level permissions    |
| **Performance**          | Use Polars or Spark for handling large data volumes              |

### 8. Summary
This notebook demonstrates a production-ready data pipeline with:

Secure file ingestion

SQL-based order retrieval

Automated KPI computation

Authorized report export

Historical archiving

Optional monthly aggregation

Features

Modular & educational code structure

Easily extensible to real production environments

Designed for both daily and monthly analytics

 Author: Anwin K Biju

