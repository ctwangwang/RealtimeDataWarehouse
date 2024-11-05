# Realtime Data Warehouse

## Overview

This project aims to build a real-time data warehouse system using a modern data engineering stack. The system is designed to collect, process, store, and visualize data in real time, providing near-instantaneous insights. It utilizes a combination of Apache Airflow, Redpanda, Apache Pinot, and other open-source tools to create a scalable and fault-tolerant pipeline.

## Features

- **Real-time Data Ingestion**: Using Redpanda to ingest data streams in real time.
- **Data Orchestration**: Apache Airflow is used to manage and schedule data pipelines.
- **Stream Processing and Aggregation**: Apache Pinot is used for real-time OLAP queries and data aggregation.
- **Dashboard Visualization**: Superset, Streamlit, or Elasticsearch is used for creating interactive dashboards to visualize the data.

## Architecture Diagram

The following is an overview of the system architecture:

1. **Data Source**: Data is generated from financial institutions and ingested into the system.
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

3. **Start the Services**

   Run the following command to start Airflow, Redpanda, Pinot, and Superset:

   ```sh
   docker-compose up -d
   ```

4. **Access Services**
   
   - **Airflow**: Access the Airflow UI at [http://localhost:8080](http://localhost:8080)
   - **Redpanda**: Broker URL is available on `localhost:9092`
   - **Pinot Console**: Available at [http://localhost:9000](http://localhost:9000)
   - **Superset**: Access Superset at [http://localhost:8088](http://localhost:8088)

## Configuration

The configuration files for Airflow, Redpanda, and Pinot can be found under the `config/` directory.

- Airflow: `config/airflow/airflow.cfg`
- Redpanda: `config/redpanda/redpanda.yaml`
- Pinot: `config/pinot/pinot-controller.conf`

Feel free to modify them as per your requirements.

## Data Flow

1. **Financial Institution Data** is pushed to Redpanda.
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
├── config
│   ├── airflow
│   ├── redpanda
│   └── pinot
├── docker-compose.yml
├── README.md
└── src
    └── dags
```

## Contributing

Contributions are welcome! Please feel free to open issues or submit pull requests for enhancements, bug fixes, or documentation improvements.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.

## Contact

If you have any questions, feel free to reach out via GitHub issues or contact me directly.

Enjoy real-time data analytics!

---

### Next Steps
- Set up sample financial data streams and see the pipeline in action.
- Create your own Airflow DAG to process custom data.
- Experiment with different connectors for various data sources.
