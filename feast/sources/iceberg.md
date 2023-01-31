---
layout: default
title: Iceberg
parent: Sources
nav_order: 1
description: "Yummy iceberg source."
permalink: /feast/sources/iceberg
---

# Iceberg

With `IcebergSource` you can consume [Iceberg](https://iceberg.apache.org/) to fetch the features.

Example `iceberg` datasource configuration:

```python
from yummy import IcebergSource

my_stats_csv = IcebergSource(
    path="local.mytable_dbz.debeziumcdc_postgres_public_mystats_fv1",
    timestamp_field="datetime",
)
```

`IcebergSource` is supported only for the spark backend.

To use it you will need to include addtional spark packages (eg. `org.apache.iceberg:iceberg-spark-runtime-3.2_2.12:0.13.2`)

Example for `local filesystem`:
```
PYSPARK_PYTHON=/opt/conda/bin/python3 PYSPARK_DRIVER_PYTHON=/opt/conda/bin/python3 FEAST_USAGE=False IS_TEST=True spark-submit \
    --packages org.apache.iceberg:iceberg-spark-runtime-3.2_2.12:0.13.2,org.apache.hadoop:hadoop-aws:3.3.1,software.amazon.awssdk:s3:2.17.131 \
    --conf "spark.driver.memory=5g" \
    --conf "spark.executor.memory=5g" \
    --conf "spark.sql.extensions=org.apache.iceberg.spark.extensions.IcebergSparkSessionExtensions" \
    --conf "spark.sql.catalog.local=org.apache.iceberg.spark.SparkCatalog" \
    --conf "spark.sql.catalog.local.type=hadoop" \
    ./iceberg-local.py
```


Example for `minio s3`:
```
PYSPARK_PYTHON=/opt/conda/bin/python3 PYSPARK_DRIVER_PYTHON=/opt/conda/bin/python3 FEAST_USAGE=False IS_TEST=True spark-submit \
    --packages org.apache.iceberg:iceberg-spark-runtime-3.2_2.12:0.13.2,org.apache.hadoop:hadoop-aws:3.3.1,software.amazon.awssdk:s3:2.17.131 \
    --conf "spark.driver.memory=5g" \
    --conf "spark.executor.memory=5g" \
    --conf "spark.sql.extensions=org.apache.iceberg.spark.extensions.IcebergSparkSessionExtensions" \
    --conf "spark.sql.catalog.local=org.apache.iceberg.spark.SparkCatalog" \
    --conf "spark.sql.catalog.local.type=hadoop" \
    --conf "spark.hadoop.fs.s3a.access.key=minioadmin" \
    --conf "spark.hadoop.fs.s3a.secret.key=minioadmin" \
    --conf "spark.hadoop.fs.s3a.endpoint=http://minio:9000" \
    ./iceberg-s3.py
```

