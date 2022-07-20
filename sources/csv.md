---
layout: default
title: CSV
parent: Sources
nav_order: 4
description: "Yummy csv source."
permalink: /sources/csv
---

# CSV

The `csv` datasource is mainly intended for the ad hoc experiments cases.

Example `csv` datasource configuration:

```python
from yummy import CsvDataSource

my_stats_csv = CsvDataSource(
    path="/home/jovyan/notebooks/dataset/all_data.csv",
    event_timestamp_column="datetime",
)
```

additionally you can setup: 
`delimiter: str`, 
`header: bool`, 
`s3_endpoint_override: str`

You can read: 
* single file: `path="/home/jovyan/notebooks/dataset/all_data.csv"`
* directory: `path="/home/jovyan/notebooks/dataset/"`
* selected files: `path="/home/jovyan/notebooks/dataset/2022-*.csv"`

You can also read from:
* `local filesystem`
* `s3` store (you can use `s3_endpoint_override` to use custom s3 like [minio](https://min.io/)

