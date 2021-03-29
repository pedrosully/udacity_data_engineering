# Sparkify Data Warehouse Implementation

## Context

Sparkify is a music streaming startup with a growing user base and song database.

A startup called Sparkify wants to analyze their overall user activity on their new music streaming app, specifically their song data. Currently, they don't have a way to query and prepare their data for analysis. The data currently lives in S3 (json files) under two different paths: one with JSON logs on user activity in the app, and another one with JSON files containing metadata about the songs in their app. Therefore it's necessary to create a Data Warehouse in Redshift and data extraction processes which are able to support future analytics efforts in order to have the data always available in a cloud solution that can be accessed directly by most BI and analytics tools.

## Database Schema Choice
### The ideal relational database schema for this context is the Star Schema because:

    -The star schema is optimized for reads and aggregations
    -The JSON files in S3 are not in an usuable state for most BI and analytics tools
    -The star schema allows users to find answers with actionable insights by performing ad-hoc analysis
    -The data source files' structure and data types are predictable
    -The star schema can be scalable as the user base grows and the streaming app generates more data
    

## Instructions to Set up the Data Warehouse using Python Scripts

1. To be able to create the data warehouse cluster the following fields need to be provided. This file should be saved as *dwh.cfg* in the project root folder.

**Note:** The only two fields you won't be able to fill right away are the "Host" in the Cluster Section (or endpoint in Redshift) and the "ARN" in the IAM_Role section. These two fields will show up in the Terminal after the IaC.py script is run, therefore you will have to add these to the *dwh.cg* file after having run the script. These two fields will be assigned to the following two variables after the data warehouse is available (wait time of 3 minutes): DWH_ENDPOINT and DWH_ROLE_ARN. 

```
[CLUSTER]
HOST=''
DB_NAME=''
DB_USER=''
DB_PASSWORD=''
DB_PORT=5439

[IAM_ROLE]
ARN=

[S3]
LOG_DATA='s3://udacity-dend/log_data'
LOG_JSONPATH='s3://udacity-dend/log_json_path.json'
SONG_DATA='s3://udacity-dend/song_data'

[AWS]
KEY=
SECRET=

[DWH]
DWH_CLUSTER_TYPE       = multi-node
DWH_NUM_NODES          = 4
DWH_NODE_TYPE          = dc2.large
DWH_CLUSTER_IDENTIFIER = 
DWH_DB                 = 
DWH_DB_USER            = 
DWH_DB_PASSWORD        = 
DWH_PORT               = 5439
DWH_IAM_ROLE_NAME      = 
```

2. Run the create_cluster.py script to set up the data warehouse, the IAM role with S3 access to be associated with it and the appropriate VPC rules to be able to interact with the cluster externally using Python scripts or a SQL client.

    ``` bash 
    python create_cluster.py
    ```


## ETL Process using Python Scripts

3. Run the create_tables script to drop any existing tables, set up the staging tables (to format/transform the json S3 raw data) in order to then set up the fact and dimensions tables.

    ``` bash
    python create_tables.py
    ```

4. Run the etl.py script to extract data from the json files in S3, stage it in two different tables in Redshift (song plays events and songs metadata), and finally store it in the fact and dimensions tables.

    ``` bash 
    python etl.py
    ```
5. Run the query_checks.py script to verify that all tables were created correctly and that the json S3 raw data was inserted correctly into the tables. This script will return a total row count for all the tables in the Data Warehouse.
    
    ``` bash 
    python query_checks.py
    ```

6. Finally, run the clean_resources.py to delete the cluster and to detach the S3 policy associated with the data warehouse along with the IAM Role (wait time of 30 seconds). This script will display some of the Data Warehouse information in the Terminal, specifically the "deleting" status of the cluster.

    ``` bash 
    python clean_resources.py
    ```

## Project Files

This project includes six script files and one configuration file:

- create_cluster.py sets up the data warehouse, the IAM role with S3 access to be associated with it and the appropriate VPC rules
- create_tables.py sets up the fact and dimensions tables
- etl.py sets up staging tables in Redshift based on the S3 data and inserts the staging tables data into the facts and dimensions tables in Redshift
- sql_queries.py sets up the SQL statements needed to drop, create and query the tables
- query_checks.py returns a total row count for each table in Redshift in order to verify all tables were created and that the correct data was inserted 
- clean_resources.py deletes data warehouse cluster, detachs S3 policy from the role associated with the data warehouse and deletes the role
- dwh.cfg contains the necessary information to create the cluster and insert data in the Data Warehouse
- README.md is the current file
