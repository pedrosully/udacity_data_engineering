# Sparkify Data Warehouse Implementation

## Context

Sparkify is a music streaming startup with a growing user base and song database.

A startup called Sparkify wants to analyze their overall user activity on their new music streaming app, specifically their song data. Currently, they don't have a way to query and prepare their data for analysis. The data currently lives in S3 (json files) under two different paths: one with JSON logs on user activity in the app, and another one with JSON files containing metadata about the songs in their app. Therefore it's necessary to create a Data Warehouse in Redshift and data extraction processes which are able to support future analytics efforts in order to have the data always available in a cloud solution that can be accessed directly by most BI and analytics tools.

## Instructions to Set up the Data Warehouse using Python Scripts

1. To be able to create the data warehouse cluster the following fields need to be provided. This file should be saved as *dwh.cfg* in the project root folder. 
**Note:** The only two fields you won't be able to fill right away are the "Host" in the Cluster Section (or endpoint in Redshift) and the "ARN" in the IAM_Role section. These two fields will show up in the Terminal after the IaC.py script is run, these two fields will be assigned to the following two variables after the data warehouse is available (wait time of 3 minutes): DWH_ENDPOINT and DWH_ROLE_ARN. 

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

2. Run the create_cluster.py script to set up the data warehouse, the IAM role with S3 access to be associated with it and the appropriate VPC rules to be able to interact with the cluster externally using scripts or a SQL client.

    ``` bash 
    python create_cluster.py
    ```


## ETL Process

3. Run the create_tables script to drop any existing tables, set up the staging tables (to format/transform the S3 raw data) in order to then set up the fact and dimensions tables.

    ``` bash
    python create_tables.py
    ```

4. Finally, run the etl.py script to extract data from the json files in S3, stage it in two different tables in redshift (events and songs), and finally store it in the fact and dimensional tables.

    ``` bash 
    python etl.py
    ```






## OPTIONAL: Question for the reviewer
 
If you have any question about the starter code or your own implementation, please add it in the cell below. 

For example, if you want to know why a piece of code is written the way it is, or its function, or alternative ways of implementing the same functionality, or if you want to get feedback on a specific part of your code or get feedback on things you tried but did not work.

Please keep your questions succinct and clear to help the reviewer answer them satisfactorily. 

> **_Your question_**