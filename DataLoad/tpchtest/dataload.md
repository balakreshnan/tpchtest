# Load TPCH_SF10000 from one Blob storage to another

## Use case

- To perform TPC-H test we need data
- Data is available in another region in Azure
- Move the data to your own region to test

## Prerequistie

- Azure Account
- Azure Blob Storage
- Azure Data factory
- Get the source blob connection string details

## Azure Data Factory

- Create a copy pipeline
- Follow the settings as below
- i am using 16 cores

![alt text](https://github.com/balakreshnan/ptc/blob/main/images/IoTStrategy.jpg "Service Health")

