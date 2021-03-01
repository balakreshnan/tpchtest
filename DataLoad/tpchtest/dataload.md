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

![alt text](https://github.com/balakreshnan/tpchtest/blob/main/images/dataload1.jpg "Service Health")

- Now connect to source
- use your source blob connection string

![alt text](https://github.com/balakreshnan/tpchtest/blob/main/images/dataload2.jpg "Service Health")

- source blob linkedn service connection details

![alt text](https://github.com/balakreshnan/tpchtest/blob/main/images/dataload3.jpg "Service Health")

- Now configure your own blob container as destination

![alt text](https://github.com/balakreshnan/tpchtest/blob/main/images/dataload4.jpg "Service Health")

- your blob connection details for linked services

![alt text](https://github.com/balakreshnan/tpchtest/blob/main/images/dataload5.jpg "Service Health")

- Make sure select binary copy
- Save the pipeline
- Now publish the pipeline
- Click Add trigger and click now
- Wait for the job to complete
- It took about 3:53 minutes to complete

![alt text](https://github.com/balakreshnan/tpchtest/blob/main/images/dataload6.jpg "Service Health")

- Performance details on how much was transferred

![alt text](https://github.com/balakreshnan/tpchtest/blob/main/images/dataload7.jpg "Service Health")

- 3.4 TB or data was transferred from west us to Central us
- Took about 3 hours and 54 minutes
- using 16 cores as parallelism
- maximum throughput 258 MB/s was the data transfer rate