# Realtime Data Warehouse

## Overview

This project aims to build a real-time data warehouse system using a modern data engineering stack. The system is designed to collect, process, store, and visualize data in real time, providing near-instantaneous insights. It utilizes a combination of Apache Airflow, Redpanda, Apache Pinot, and other open-source tools to create a scalable and fault-tolerant pipeline.

## Features

- **Real-time Data Ingestion**: Using Redpanda to ingest data streams in real time.
- **Data Orchestration**: Apache Airflow is used to manage and schedule data pipelines.
- **Stream Processing and Aggregation**: Apache Pinot is used for real-time OLAP queries and data aggregation.
- **Dashboard Visualization**: Superset, Streamlit, or Elasticsearch is used for creating interactive dashboards to visualize the data.
- **Data Simulation**: Scripts are provided to simulate financial data for ingestion.

## Architecture Diagram

The following is an overview of the system architecture:

1. **Data Source**: Data is simulated from financial institutions using provided scripts and ingested into the system.
2. **Airflow**: Manages and orchestrates data workflows.
3. **Redpanda**: A streaming platform used to manage real-time data ingestion.
4. **Pinot**: Stores processed data for fast querying and supports real-time OLAP queries.
5. **Superset/Streamlit/Elasticsearch**: Visualization tools for interactive and real-time dashboards.

## Prerequisites

To run this project locally, you need:

- Docker and Docker Compose
- Python 3.x (for Streamlit or Superset setup)

## Quick Start

1. **Clone the Repository**

   ```sh
   git clone https://github.com/ctwangwang/RealtimeDataWarehouse.git
   cd RealtimeDataWarehouse
   ```

2. **Set Up Environment**

   Make sure Docker is installed and running on your machine. The `docker-compose.yml` file will set up all the required services.

3. **Simulate Financial Data**

   Use the provided Python scripts to simulate financial data. The scripts are located in the `dags/` folder.

   ```sh
   python data_generators/account_dim_generator.py
   python data_generators/branch_dim_generator.py
   python data_generators/customer_dim_generator.py
   python data_generators/transaction_facts_generator.py
   ```

4. **Start the Services**

   Run the following command to start Airflow, Redpanda, Pinot, and Superset:

   ```sh
   docker-compose up -d
   ```

5. **Access Services**
   
   - **Airflow**: Access the Airflow UI at [http://localhost:8080](http://localhost:8080)
   - **Redpanda**: Broker URL is available on `localhost:9092`
   - **Pinot Console**: Available at [http://localhost:9000](http://localhost:9000)
   - **Superset**: Access Superset at [http://localhost:8088](http://localhost:8088)


## Data Flow

1. **Financial Institution Data** is simulated using Python scripts and pushed to Redpanda.
2. **Airflow** orchestrates the ETL workflows, manages dependencies, and schedules jobs.
3. **Pinot** stores processed data and enables fast querying for real-time analytics.
4. **Superset/Streamlit/Elasticsearch** visualize the processed data in dashboards.

## Example Use Cases

- Real-time Financial Monitoring: Stream financial data and build dashboards for monitoring transactions.
- Customer Analysis: Process real-time customer interaction data and build analytics for customer behavior.

## Running Airflow DAGs

To submit a DAG to Airflow, place your DAG file in the `dags/` folder and use the Airflow CLI or Web UI to trigger the DAG. Example:

```sh
airflow dags trigger your_dag_id
```

## Useful Commands

- **Start Airflow Shell**:

  ```sh
  docker exec -it airflow-scheduler bash
  ```

- **List Redpanda Topics**:

  ```sh
  rpk topic list --brokers localhost:9092
  ```

- **Check Pinot Controller Status**:

  ```sh
  curl -X GET http://localhost:9000/controller/api/systemStatus
  ```

## Directory Structure

```
.
├── docker-compose.yml
├── README.md
├── dags
│   ├── account_dim_generator.py
│   ├── branch_dim_generator.py
│   ├── customer_dim_generator.py
│   └── transaction_facts_generator.py
│   └── loader_dag.py
│   └── schema_dag.py
│   └── table_dag.py
├── plugins
│   └── kafka_operator.py
│   └── pinot_schema_operator.py
│   └── pinot_table_operator.py

```



---

### Next Steps
- Set up sample financial data streams and see the pipeline in action.
- Create your own Airflow DAG to process custom data.
- Experiment with different connectors for various data sources.
