
## Table of Contents

1. [Introduction](#introduction)
2. [Core Concepts](#core-concepts)

   * SparkContext
   * JavaSparkContext
   * SQLContext
   * SparkSession
3. [Basic Usage](#basic-usage)

   * Reading / Writing
   * JSON / CSV / Parquet
4. [RDD API](#rdd-api)
5. [DataFrame API](#dataframe-api)
6. [UDF Examples](#udf-examples)
7. [Aggregation & Grouping](#aggregation--grouping)
8. [String / Array Operations](#string--array-operations)
9. [Vectorization & ML](#vectorization--ml)
10. [Plotting](#plotting)
11. [PySpark Session Management](#pyspark-session-management)
12. [Spark Streaming](#spark-streaming)
13. [Spark ML Libraries](#spark-ml-libraries)
14. [Installation & Setup](#installation--setup)

    * Scala
    * SBT
    * Spark
15. [HDFS Notes](#hdfs-notes)
16. [Performance](#performance)
17. [Misc Python tooling](#misc-python-tooling)
18. [Full Snippet Archive (Original Notes)](#full-snippet-archive-original-notes)

---

## Introduction

Apache Spark is a distributed processing engine designed to process large datasets quickly using optimized execution and in-memory caching.

Key benefits:

* In-memory computation
* Rich API (DataFrame, SQL, RDD)
* Streaming support
* ML pipelines
* Graph computation

---

## Core Concepts

### SparkContext

* Main entry point for Spark functionality.
* Represents the connection to a Spark cluster.
* Used for creating RDDs, accumulators, broadcast variables.
* Only **one SparkContext per JVM**.
* Must call `stop()` before creating a new one.

### JavaSparkContext

* Java-friendly version of SparkContext.
* Returns `JavaRDD` and works with Java collections.

### SQLContext (Spark < 2.0)

* Entry point for structured data.
* Replaced by SparkSession in Spark 2.0.

### SparkSession

* Main entry point to programming with DataFrame / Dataset APIs.

```python
from pyspark.sql import SparkSession
spark = SparkSession.builder.appName("app").getOrCreate()
```

---

## Basic Usage

### Reading & Writing

```python
# Parquet
# df.write.parquet("people.parquet")
df = spark.read.parquet("path/to/parquet")
```

```python
# JSON / CSV
# df = spark.read.json("file:///path/to/data.json")
# df = spark.read.csv("file:///path/to/data.csv")
```

### Random Split

```python
split_1, split_2 = data_1.randomSplit([0.2, 0.8], 24)
```

### Create DataFrame

```python
dfr = spark.createDataFrame(row, schema=StringType())
```

---

## RDD API

RDD = Resilient Distributed Dataset

* Fundamental data structure.
* Supports **transformations** (lazy) and **actions**.

### Examples

```python
arr.take(5)         # return first 5
arr.top(10)         # top sorted
arr.takeOrdered()   # ordered
```

Transformations:

* map
* filter
* flatMap

Actions:

* collect()   # dangerous — loads all data into RAM
* count()
* reduce()
* saveAsTextFile()

Cache:

```python
arr.cache()
arr.is_cached()
```

---

## DataFrame API

### Inspection

```python
arr.head()
arr.show()
arr.first()
```

### Filter

```python
arr.filter(lambda x: x%2==0).show()
df[df.provider.endswith('emy')]
```

### Order

```python
data.orderBy(data['count'].desc()).show()
```

### DataFrame → Python list

```python
row = [row.count for row in data.select('count').collect()]
row = [int(i) for i in row]
```

### cast

```python
dcf.select(dcf.word_count.cast('string'))
```

---

## Aggregation & Grouping

```python
from pyspark.sql.functions import collect_list, count, avg, sum

# Collect arrays
data_ratio.groupBy(['contact_id','channel_tg_id']).agg(
    collect_list('obstacle_count').alias('obstacle_count')
).show()
```

```python
# Count
split_1.groupBy(['contact_id','channel_tg_id']).agg(
    count('obstacle_count').alias('obstacle_count')
).show()
```

```python
data_punct.groupBy("contact_id").agg(
    F.sum("count_comma"),
    F.count("sms_len")
).show()
```

---

## String / Array operations

### explode

```python
from pyspark.sql import Row
edf = spark.createDataFrame([Row(a=1, tlist=['text1','text2'])])

edf.select(F.explode(edf.tlist)).collect()
```

### array_contains

```python
edf.select(F.array_contains(edf.tlist,'text2')).collect()
```

### Levenshtein

```python
df.select(F.levenshtein('name', 'desc').alias('leven'))
```

### split by RegExp

```python
df.select(F.split(df.cat,'regexp').alias('s'))
```

---

## UDF Examples

```python
max_udf = udf(lambda x: x*2, IntegerType())
df.withColumn("1", max_udf(df.Vectors)).show()
```

Count tokens:

```python
count_tokens = udf(lambda words: len(words), IntegerType)
regex_tokenized.withColumn('tokens', count_tokens(col('words')))
```

---

## Vectorization & ML

```python
cv2 = CountVectorizer(inputCol="stop_token", outputCol='freq', vocabSize=262144, minDF=1000.0)
count_vec = cv.fit(data)
data_cv = count_vec.transform(data)
```

---

## Plotting

> (Matplotlib, Seaborn examples)

```python
plt.style.use('ggplot')
plt.figure(figsize=(15, 15))
plt.plot(x, y, label='До')
```

Histogram:

```python
plt.hist(ty, bins=148)
```

Seaborn:

```python
sns.distplot(gh['marker'], bins=100)
```

---

## PySpark Session Management

Only **one SparkContext** allowed.

Restart if needed.

Caching:

```python
# df.cache()
# df.unpersist()
```

---

## Spark Streaming

* Microbatching model

---

## Spark ML Libraries

* spark.mllib (RDD-based)
* spark.ml (DataFrame-based)

---

## Installation & Setup

### Scala

```bash
sudo apt-get install openjdk-8-jdk
```

### SBT

```bash
echo "deb https://dl.bintray.com/sbt/debian /" | sudo tee -a /etc/apt/sources.list.d/sbt.list
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 2EE0EA64E40A89B84B2DF73499E82A75642AC8
sudo apt-get update
sudo apt-get install sbt
```

### Spark

```
bin/pyspark
bin/spark-submit my_spark_app.py
```

Example interactive:

```python
os.environ["PYSPARK_SUBMIT_ARGS"]='--packages com.databricks:spark-csv_2.10:2.2.0 pyspark-shell'
```

---

## HDFS Notes

Spark by default reads from **HDFS**.

---

## Performance Notes

### Shuffle

Data movement between nodes.

---

## Misc Python Tooling

* Docker
* Spyder
* Numba
* Dask
* Bokeh
* HoloViews
* DataShader
* H2O.ai

---

## Full Snippet Archive (Original Notes)

(Original content preserved — see sections above)
