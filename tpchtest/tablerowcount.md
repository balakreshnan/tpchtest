# Row count on all the tables

## information

- Data in parquet format
- Can be accessed through spark
- can be accessed through Azure synapse serverless sql

## Count each data set and disply the total rows

- To explain schema of parquet

```
EXEC sp_describe_first_result_set N'
	SELECT
    *
FROM
    OPENROWSET(
        BULK ''https://storageaccountname.dfs.core.windows.net/tpchdata/LINEITEM/*.parquet'',
        FORMAT=''PARQUET''
    ) AS [result]';
```

- display from above query

![alt text](https://github.com/balakreshnan/tpchtest/blob/main/images/tpch1data.jpg "Service Health")

- Now run count queries to get each table count

- Line item count

```
select count_big (*) from lineitem; -- 59,999,994,267
```

- Total number of rows: 59,999,994,267
- Close to 59 billion

- Orders count

```
select count_big (*) from orders; -- 15,000,000,000
```

- Total orders row count: 15,000,000,000
- close to 15 billion

- Customers count

```
select count_big (*) from customer; -- 1,500,000,000
```

- Total customer rows: 1,500,000,000
- close to 1.5 billion

- Supplier row count

```
select count_big (*) from supplier; -- 100,000,000
```

- Total supplier row count: 100,000,000

- Nation Count

```
select count_big (*) from nation; -- 25
```

- Nation row count: 25

- Count Region row

```
select count_big (*) from region; -- 5
```

- Total region row count: 5

- Part's count

```
select count_big (*) from part; -- 2,000,000,000
```

- Total row count: 2,000,000,000
- 2 Billion row