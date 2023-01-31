---
layout: default
title: Parquet
parent: Sources
nav_order: 3
description: "Yummy parquet source."
permalink: /feast/sources/parquet
---

# Parquet

Example `parquet` datasource configuration:

```python
from yummy import ParquetSource

my_stats_csv = ParquetSource(
    path="/home/jovyan/notebooks/dataset/all_data.parquet",
    timestamp_field="datetime",
)
```

additionally you can setup: 
`s3_endpoint_override: str`

You can read: 
* single file: `path="/home/jovyan/notebooks/dataset/all_data.parquet"`
* directory: `path="/home/jovyan/notebooks/dataset/"`
* selected files: `path="/home/jovyan/notebooks/dataset/2022-*.parquet"`

You can also read from:
* `local filesystem`
* `s3` store (you can use `s3_endpoint_override` to use custom s3 like [minio](https://min.io/)

