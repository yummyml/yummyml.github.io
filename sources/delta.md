---
layout: default
title: Delta
parent: Sources
nav_order: 2
description: "Yummy delta source."
permalink: /sources/delta
---

# Delta

With `DeltaDataSource` you can consume [Delta Lake](https://delta.io/) to fetch the features.

Example `delta` datasource configuration:

```python
from yummy import DeltaDataSource

my_stats_csv = DeltaDataSource(
    path="/home/jovyan/notebooks/dataset/delta/",
    event_timestamp_column="datetime",
)
```

additionally you can setup: 
`s3_endpoint_override: str`

You can also read from:
* `local filesystem`
* `s3` store (you can use `s3_endpoint_override` to use custom s3 like [minio](https://min.io/)

`DeltaDataSource` we be handled differently for `yummy` backends.
For `polars`, `dask`, `ray` the rust [delta-rs](https://github.com/delta-io/delta-rs) implementation will be used.

For `spark` the spark delta reader will be used. To use it you will need to include addtional 
spark packages (eg. `io.delta:delta-core_2.12:1.1.0`)

For example to run pyspark with jupyter driver:
```
#!/bin/bash

export PYSPARK_DRIVER_PYTHON=jupyter
export PYSPARK_DRIVER_PYTHON_OPTS="lab --notebook-dir=/home/jovyan --ip='0.0.0.0' --port=8888 --no-browser --allow-root --NotebookApp.password='' --NotebookApp.token=''"

pyspark \
    --packages io.delta:delta-core_2.12:1.1.0 \
    --conf "spark.sql.extensions=io.delta.sql.DeltaSparkSessionExtension" \
    --conf "spark.sql.catalog.spark_catalog=org.apache.spark.sql.delta.catalog.DeltaCatalog" \
    --conf "spark.driver.memory=5g" \
    --conf "spark.executor.memory=5g"
```
