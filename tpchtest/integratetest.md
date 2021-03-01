# TPC-H SF10000 test with Azure Syanpse Analytics Integrate and Dataflow

## Use Case

- Run TPC-H Test using Spark SQL
- Run TPC-H test usign Spark Scala dataframe
- Prepare the data needed for testing
- Cloud based architecture testing
- Storage and compute split testing

## Pre requsitie

- TPC-H Schema - http://www.tpc.org/tpc_documents_current_versions/pdf/tpc-h_v2.17.1.pdf
- Load data before processing - https://github.com/balakreshnan/tpchtest/blob/main/DataLoad/tpchtest/dataload.md
- Container name for data loaded - tpchdata
- Azure Account
- Azure blob storage account
- Azure Databricks
- Azure Keyvault

## Azure Synapse Integrate - data flow code

- Create a data flow
- Create all the sources
- Create Joins
- Create output sink

![alt text](https://github.com/balakreshnan/tpchtest/blob/main/images/integrate1.jpg "Service Health")

- Connect to customer and other sources and check the schema

![alt text](https://github.com/balakreshnan/tpchtest/blob/main/images/integrate3.jpg "Service Health")

- Create Join between orders with lineitem and customers and suppliers.

![alt text](https://github.com/balakreshnan/tpchtest/blob/main/images/integrate4.jpg "Service Health")

- Check the output sink and validate the schema
- All columns should show

![alt text](https://github.com/balakreshnan/tpchtest/blob/main/images/integrate5.jpg "Service Health")

- Create a new pipeline
- Set the parallism to process

![alt text](https://github.com/balakreshnan/tpchtest/blob/main/images/integrate2.jpg "Service Health")