# PySpark_Notes_Project

```markdown
# PySpark Cheat Sheet

## Setting Up Spark Session

```python
from pyspark.sql import SparkSession
spark = SparkSession.builder \
    .appName("PySpark Cheat Sheet") \
    .getOrCreate()
```

## Dataframe Operations

### Creating DataFrames

```python
# From a list of tuples
df = spark.createDataFrame([(1, 'foo'), (2, 'bar')], ["id", "label"])

# From a Pandas dataframe
import pandas as pd
pandas_df = pd.DataFrame({"id": [1, 2], "label": ["foo", "bar"]})
df = spark.createDataFrame(pandas_df)
```

### Reading Data

```python
df = spark.read.csv("path/to/file.csv", header=True, inferSchema=True)
df = spark.read.json("path/to/file.json")
```

### Writing Data

```python
df.write.csv("path/to/output", mode="overwrite")
df.write.json("path/to/output", mode="append")
```

### Showing Data

```python
df.show()
df.printSchema()
```

### Selecting and Filtering

```python
df.select("column1", "column2").show()
df.filter(df["column"] > 0).show()
```

### Grouping and Aggregating

```python
df.groupBy("column").count().show()
df.groupBy("column").agg({"column2": "sum"}).show()
```

### Joining DataFrames

```python
df1.join(df2, df1["id"] == df2["id"]).show()
```

## Spark SQL

```python
df.createOrReplaceTempView("table")
spark.sql("SELECT * FROM table WHERE column > 0").show()
```

## Working with RDDs

```python
rdd = spark.sparkContext.parallelize([1, 2, 3])
rdd.map(lambda x: x * x).collect()
```

## Performance Tuning

- **Caching Data:** Use `df.cache()` or `df.persist()` to cache dataframes across operations.
- **Repartitioning:** Repartition or coalesce datasets using `df.repartition(numPartitions)` or `df.coalesce(numPartitions)` to optimize shuffling and improve execution times.

This cheat sheet covers basic yet fundamental aspects of PySpark, including setting up the environment, manipulating dataframes, and optimizing performance. For more comprehensive details, the official PySpark documentation is an excellent resource.
```

