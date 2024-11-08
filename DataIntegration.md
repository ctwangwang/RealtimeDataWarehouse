# Apache Pinot ETL Pipeline Overview

This README.md provides an overview of the various scripts used in the ETL pipeline for integrating with Apache Pinot. The pipeline includes generating data, managing schema definitions, creating tables, and ultimately loading the data into Apache Pinot using Airflow and custom operators.

## Components Overview

The ETL pipeline consists of the following components:

1. **Data Generation (`account_dim_generator.py`, `transaction_facts_generator.py`)**: Scripts for generating or fetching data.
2. **Schema Management (`schema_dag.py`)**: DAG responsible for creating or updating Pinot schemas.
3. **Table Management (`table_dag.py`)**: DAG responsible for creating or updating Pinot tables.
4. **Data Loading (`loader_dag.py`)**: DAG for loading generated data into Pinot.
5. **Custom Operators (`pinot_schema_operator.py`, `pinot_table_operator.py`, `kafka_operator.py`)**: Operators for direct interaction with Apache Pinot and Kafka to perform schema, table, and streaming operations.

## Detailed Integration Workflow

### 1. Data Generation (`account_dim_generator.py`, `transaction_facts_generator.py`)

- **`account_dim_generator.py`**:
  - **Purpose**: This script is responsible for generating the actual data that will be loaded into Apache Pinot.
  - **Description**: It could generate mock data, transform data from other sources, or prepare data for ingestion. This data is saved in a file or streamed to be used downstream for ingestion into Pinot.

- **`transaction_facts_generator.py`**:
  - **Purpose**: This script generates transactional data (facts) that will be ingested into Pinot.
  - **Description**: The transaction facts data is typically different from dimension data, representing time-based activities or events. The data generated by this script is streamed to Redpanda (Kafka-compatible streaming platform) for real-time ingestion into Pinot.

### 2. Schema Management (`schema_dag.py`)

- **Purpose**: Manage the Pinot schema.
- **Description**: This Apache Airflow DAG is used to create or update the Pinot schema using a custom operator (`pinot_schema_operator.py`). The schema defines the data structure in Pinot, specifying field names, types, etc.
- **Integration**: It sends requests to the Pinot controller API to ensure the schema is properly configured before data ingestion.

### 3. Table Management (`table_dag.py`)

- **Purpose**: Manage Pinot tables.
- **Description**: The `table_dag.py` Airflow DAG is used to create or update tables in Apache Pinot. It uses the custom operator (`pinot_table_operator.py`) to interact with Pinot's table configuration, such as setting up partitions and indexing strategies.
- **Integration**: Ensures that a Pinot table is correctly defined before data is ingested, supporting both offline and real-time use cases.

### 4. Data Loading (`loader_dag.py`)

- **Purpose**: Load the generated data into Pinot.
- **Description**: This Airflow DAG loads the data generated from the `account_dim_generator.py` into the appropriate Pinot table. The data source must match the schema created earlier to ensure seamless ingestion.
- **Integration**: Involves calling Pinot ingestion APIs or streaming the data into the Pinot table.

### 5. Custom Operators (`pinot_schema_operator.py`, `pinot_table_operator.py`, `kafka_operator.py`)

- **`pinot_schema_operator.py`**: Handles interactions related to Pinot schemas. It manages schema creation and updates by sending HTTP requests to the Pinot controller.
- **`pinot_table_operator.py`**: Handles Pinot table operations, such as creating, modifying, or deleting tables. These operators help automate the Pinot setup tasks in Airflow.
- **`kafka_operator.py`**: Manages interaction with Kafka (in this case, Redpanda, which is Kafka-compatible) to handle streaming of transactional data generated by `transaction_facts_generator.py`. It helps set up the Kafka topic and facilitates data flow from Redpanda to Pinot for real-time analytics.

## Workflow Summary

The integration of these scripts ensures a complete ETL flow that operates as follows:

1. **Data Preparation**: 
   - **Dimension Data**: The `account_dim_generator.py` script generates and prepares dimension data.
   - **Transaction Data**: The `transaction_facts_generator.py` script generates transactional data that will be streamed via Redpanda.

2. **Schema Setup**: The `schema_dag.py` DAG ensures the correct schema is created or updated in Pinot using the `pinot_schema_operator.py`.

3. **Table Setup**: The `table_dag.py` DAG defines or updates the Pinot table where data will be stored, using `pinot_table_operator.py`.

4. **Data Loading**:
   - **Dimension Data Loading**: The `loader_dag.py` loads the dimension data into the Pinot table, ensuring it matches the schema.
   - **Transaction Data Streaming**: The transaction data generated is streamed to Redpanda and then ingested into Pinot using Kafka integration (`kafka_operator.py`). This ensures real-time ingestion and indexing of the transaction data in Pinot.

## Example Use Case
- **Dimension Data Generation**: `account_dim_generator.py` generates account-level data.
- **Transactional Data Generation**: `transaction_facts_generator.py` generates transaction facts and streams them using Redpanda.
- **Schema and Table Management**: `schema_dag.py` and `table_dag.py` ensure that Pinot is properly set up to store both dimension and transaction data.
- **Data Ingestion**: `loader_dag.py` ingests dimension data, while `kafka_operator.py` manages the real-time ingestion of transaction data into Pinot.

The use of custom operators (`pinot_schema_operator.py`, `pinot_table_operator.py`, and `kafka_operator.py`) simplifies the interaction with Pinot and Kafka, providing an efficient way to manage schemas, tables, and real-time streaming through Apache Airflow.

## Conclusion
This ETL pipeline provides a scalable and efficient way to manage data ingestion into Apache Pinot, covering everything from data generation (both dimension and transaction), schema and table management, to the final ingestion step. It leverages Airflow for orchestration, Pinot for fast analytics, Redpanda for real-time data streaming, and custom Python operators to bridge the gap between these systems.



