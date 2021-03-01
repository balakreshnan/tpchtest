# TPC-H SF10000 test with Azure Synapse workspace Spark using Spark SQL

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
- Azure Keyvault

## Cluster Size

- Databricks runtime 7.5ML
- Total Nodes 4
- Each node 8 cores and 28GB RAM
- Use Spark 3.01
- Use scala 2.12

![alt text](https://github.com/balakreshnan/tpchtest/blob/main/images/synapsespark1.jpg "Service Health")

- Create a notebook now
- First get blob storage keyvault secret for primary key

```
val keyVaultName = "keyvaultname";
val secretName = "keyname";
val blobsecret = "keyname"
val accbbstorekey = mssparkutils.credentials.getSecret(keyVaultName, blobsecret)
```

- Configure blob storage for spark to access

```
spark.conf.set(
  "fs.azure.account.key.accbbstore.blob.core.windows.net",
  accbbstorekey)
```

- Let's read the customer data

```
val customer = spark.read.parquet("wasbs://containername@storagename.blob.core.windows.net/CUSTOMER/*.parquet")
```

- Display Customer

```
display(customer)
```

- now create temporary view to run sql commands

```
%sql
CREATE TEMPORARY VIEW customer
USING org.apache.spark.sql.parquet
OPTIONS (
  path "wasbs://containername@storagename.blob.core.windows.net/CUSTOMER/*.parquet"
)
```

- Let's load line item

```
val lineitem = spark.read.parquet("wasbs://containername@storagename.blob.core.windows.net/LINEITEM/*.parquet")
```

- display line item

```
display(linetem)
```

- Create temporary view to run sql

```
%sql
CREATE TEMPORARY VIEW lineitem
USING org.apache.spark.sql.parquet
OPTIONS (
  path "wasbs://containername@storagename.blob.core.windows.net/LINEITEM/*.parquet"
)
```

- Load nation dataset

```
val nation = spark.read.parquet("wasbs://containername@storagename.blob.core.windows.net/NATION/*.parquet")
```

- display nation

```
display(nation)
```

- now create the temporary view

```
%sql
CREATE TEMPORARY VIEW nation
USING org.apache.spark.sql.parquet
OPTIONS (
  path "wasbs://containername@storagename.blob.core.windows.net/NATION/*.parquet"
)
```

- load orders into data frame

```
val orders = spark.read.parquet("wasbs://containername@storagename.blob.core.windows.net/ORDERS/*.parquet")
```

- display orders

```
display(orders)
```

- Now create temporary view to run Spark SQL

```
%sql
CREATE TEMPORARY VIEW orders
USING org.apache.spark.sql.parquet
OPTIONS (
  path "wasbs://containername@storagename.blob.core.windows.net/ORDERS/*.parquet"
)
```

- now let's load part dataset

```
val part = spark.read.parquet("wasbs://containername@storagename.blob.core.windows.net/PART/*.parquet")
```

- Display part data set

```
display(part)
```

- Now lets create temporary view for spark SQL

```
%sql
CREATE TEMPORARY VIEW part
USING org.apache.spark.sql.parquet
OPTIONS (
  path "wasbs://containername@storagename.blob.core.windows.net/PART/*.parquet"
)
```

- Let's load parts supplier

```
val partsupp = spark.read.parquet("wasbs://containername@storagename.blob.core.windows.net/PARTSUPP/*.parquet")
```

- display parts supplier

```
display(partsupp)
```

- Let now create temporary view for spark SQL

```
%sql
CREATE TEMPORARY VIEW partsupp
USING org.apache.spark.sql.parquet
OPTIONS (
  path "wasbs://containername@storagename.blob.core.windows.net/PARTSUPP/*.parquet"
)
```

- let's load Region dataset

```
val region = spark.read.parquet("wasbs://containername@storagename.blob.core.windows.net/REGION/*.parquet")
```

- display region dataset

```
display(region)
```

- Let's create the temporary view for Spark SQL

```
%sql
CREATE TEMPORARY VIEW region
USING org.apache.spark.sql.parquet
OPTIONS (
  path "wasbs://containername@storagename.blob.core.windows.net/REGION/*.parquet"
)
```

- Let's load supplier data set now

```
val supplier = spark.read.parquet("wasbs://containername@storagename.blob.core.windows.net/SUPPLIER/*.parquet")
```

- display the data set

```
display(supplier)
```

- Lets create temporary view for spark sql

```
%sql
CREATE TEMPORARY VIEW supplier
USING org.apache.spark.sql.parquet
OPTIONS (
  path "wasbs://containername@storagename.blob.core.windows.net/SUPPLIER/*.parquet"
)
```

- Now let's run a TPC-H query
- Find the performance

```
%sql

select
       l_returnflag,
       l_linestatus,
       sum(l_quantity) as sum_qty,
       sum(l_extendedprice) as sum_base_price,
       sum(l_extendedprice * (1-l_discount)) as sum_disc_price,
       sum(l_extendedprice * (1-l_discount) * (1+l_tax)) as sum_charge,
       avg(l_quantity) as avg_qty,
       avg(l_extendedprice) as avg_price,
       avg(l_discount) as avg_disc,
       count(*) as count_order
 from
       lineitem
 where
       l_shipdate <= date_add(to_date('1998-12-01'), -90)
 group by
       l_returnflag,
       l_linestatus
 order by
       l_returnflag,
       l_linestatus;
```

![alt text](https://github.com/balakreshnan/tpchtest/blob/main/images/synapsespark2.jpg "Service Health")


- Now another query

```
%sql
Select 
       a.l_returnflag,
       a.l_linestatus,
       sum(a.l_quantity) as sum_qty,
       sum(a.l_extendedprice) as sum_base_price,
       sum(a.l_extendedprice * (1-a.l_discount)) as sum_disc_price,
       sum(a.l_extendedprice * (1-a.l_discount) * (1+a.l_tax)) as sum_charge,
       avg(a.l_quantity) as avg_qty,
       avg(a.l_extendedprice) as avg_price,
       avg(a.l_discount) as avg_disc,
       count(*) as count_order
from 
lineitem a join orders b on a.L_ORDERKEY = b.O_ORDERKEY 
join customer c on b.O_CUSTKEY = c.C_CUSTKEY
join partsupp d on a.L_SUPPKEY = d.PS_SUPPKEY
join part e on d.PS_PARTKEY = e.P_PARTKEY
join supplier f on d.PS_SUPPKEY = f.S_SUPPKEY
join nation g on f.S_NATIONKEY = g.N_NATIONKEY
join region h on g.N_REGIONKEY = h.R_REGIONKEY
 where
       a.l_shipdate <= date_add(to_date('1998-12-01'), -90)
 group by
       a.l_returnflag,
       a.l_linestatus
 order by
       a.l_returnflag,
       a.l_linestatus
```