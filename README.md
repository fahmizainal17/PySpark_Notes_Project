# PySpark Notes for Data Science

## Initializing SparkSession

The starting point of any PySpark application:

```python
from pyspark.sql import SparkSession
spark = SparkSession.builder \
    .appName("PySpark Cheat Sheet") \
    .config("spark.some.config.option", "some-value") \
    .getOrCreate()
```

## DataFrames Operations

### Creating DataFrames

- **From a list of tuples**:
    ```python
    df = spark.createDataFrame([(1, 'foo'), (2, 'bar')], ["id", "label"])
    ```
- **From a Pandas DataFrame**:
    ```python
    import pandas as pd
    pandas_df = pd.DataFrame({"id": [1, 2], "label": ["foo", "bar"]})
    df = spark.createDataFrame(pandas_df)
    ```

### Reading and Writing Data

- **Reading Data**:
    ```python
    df = spark.read.csv("path/to/file.csv", header=True, inferSchema=True)
    df = spark.read.json("path/to/file.json")
    ```
- **Writing Data**:
    ```python
    df.write.csv("path/to/output", mode="overwrite")
    df.write.json("path/to/output", mode="append")
    ```

### Displaying Data

```python
df.show()
df.printSchema()
```

### Data Manipulation

- **Selecting and Filtering**:
    ```python
    df.select("column1", "column2").show()
    df.filter(df["column"] > 0).show()
    ```
- **Grouping and Aggregating**:
    ```python
    df.groupBy("column").count().show()
    df.groupBy("column").agg({"column2": "sum"}).show()
    ```
- **Joining DataFrames**:
    ```python
    df1.join(df2, df1["id"] == df2["id"]).show()
    ```

## Spark SQL

Enables running SQL queries programmatically:

```python
df.createOrReplaceTempView("table")
spark.sql("SELECT * FROM table WHERE column > 0").show()
```

## Working with RDDs

Resilient Distributed Datasets (RDDs) are fundamental to PySpark:

```python
rdd = spark.sparkContext.parallelize([1, 2, 3])
rdd.map(lambda x: x * x).collect()
```

## Performance Tuning

Optimizing performance with caching and repartitioning:

- **Caching Data**:
    ```python
    df.cache() # or df.persist()
    ```
- **Repartitioning**:
    ```python
    df.repartition(numPartitions)
    df.coalesce(numPartitions)
    ```

## Additional SQL Operations

### Handling DataFrames

- **Adding, Updating, and Removing Columns**:
    ```python
    df = df.withColumn('newColumn', df['existingColumn'] + 1)
    df = df.drop('unwantedColumn')
    df = df.withColumnRenamed('oldColumnName', 'newColumnName')
    ```
- **Working with JSON and Parquet Files**:
    ```python
    df = spark.read.json("customer.json")
    df.write.save("output.parquet")
    ```

### Querying Data

- **Select, Filter, and Sort**:
    ```python
    df.select("firstName").show()
    df.filter(df["age"] > 24).show()
    df.sort("age", ascending=False).collect()
    ```
- **Aggregations and Statistics**:
    ```python
    df.groupBy("age").count().show()
    df.describe().show()
    ```

### Repartitioning for Efficiency

- **Adjusting DataFrame Partitions**:
    ```python
    df.repartition(10).rdd.getNumPartitions()
    df.coalesce(1).rdd.getNumPartitions()
    ```

This enhanced note combines the initial PySpark guide with added details on SQL basics, DataFrame manipulations, and performance tuning to serve as a comprehensive resource for data science projects using PySpark.
