---
layout: default
title: Quickstart
nav_order: 2
description: "Yummy documentation."
permalink: /quickstart
---

# Yummy Quickstart

### Install yummy:

```bash
pip install yummy
```

```bash
pip install git+https://github.com/qooba/yummy.git
```

### Create a feature repository:
```bash
feast init feature_repo
cd feature_repo
```

### Offline store:

#### Polars

To configure the offline store edit `feature_store.yaml`
```yaml
project: feature_repo
registry: data/registry.db
provider: local
online_store:
    ...
offline_store:
    type: yummy.YummyOfflineStore
    backend: polars
```

#### Dask

To configure the offline store edit `feature_store.yaml`
```yaml
project: feature_repo
registry: data/registry.db
provider: local
online_store:
    ...
offline_store:
    type: yummy.YummyOfflineStore
    backend: dask
```

#### Ray

To configure the offline store edit `feature_store.yaml`
```yaml
project: feature_repo
registry: data/registry.db
provider: local
online_store:
    ...
offline_store:
    type: yummy.YummyOfflineStore
    backend: ray
```

#### Spark

To configure the offline store edit `feature_store.yaml`
```yaml
project: feature_repo
registry: data/registry.db
provider: local
online_store:
    ...
offline_store:
    type: yummy.YummyOfflineStore
    backend: spark
    spark_conf:
        spark.master: "local[*]"
        spark.ui.enabled: "false"
        spark.eventLog.enabled: "false"
        spark.sql.session.timeZone: "UTC"
```


### Features definition

Example `features.py`:
```python
from google.protobuf.duration_pb2 import Duration
from feast import Entity, Feature, FeatureView, ValueType
from yummy import ParquetDataSource, CsvDataSource, DeltaDataSource

my_stats_parquet = ParquetDataSource(
    path="/home/jovyan/notebooks/ray/dataset/all_data.parquet",
    event_timestamp_column="datetime",
)

my_stats_delta = DeltaDataSource(
    path="dataset/all",
    event_timestamp_column="datetime",
    #range_join=10,
)

my_stats_csv = CsvDataSource(
    path="/home/jovyan/notebooks/ray/dataset/all_data.csv",
    event_timestamp_column="datetime",
)

my_entity = Entity(name="entity_id", value_type=ValueType.INT64, description="entity id",)

mystats_view_parquet = FeatureView(
    name="my_statistics_parquet",
    entities=["entity_id"],
    ttl=Duration(seconds=3600*24*20),
    features=[
        Feature(name="p0", dtype=ValueType.FLOAT),
        Feature(name="p1", dtype=ValueType.FLOAT),
        Feature(name="p2", dtype=ValueType.FLOAT),
        Feature(name="p3", dtype=ValueType.FLOAT),
        Feature(name="p4", dtype=ValueType.FLOAT),
        Feature(name="p5", dtype=ValueType.FLOAT),
        Feature(name="p6", dtype=ValueType.FLOAT),
        Feature(name="p7", dtype=ValueType.FLOAT),
        Feature(name="p8", dtype=ValueType.FLOAT),
        Feature(name="p9", dtype=ValueType.FLOAT),
        Feature(name="y", dtype=ValueType.FLOAT),
    ], online=True, input=my_stats_parquet, tags={},)

mystats_view_delta = FeatureView(
    name="my_statistics_delta",
    entities=["entity_id"],
    ttl=Duration(seconds=3600*24*20),
    features=[
        Feature(name="d0", dtype=ValueType.FLOAT),
        Feature(name="d1", dtype=ValueType.FLOAT),
        Feature(name="d2", dtype=ValueType.FLOAT),
        Feature(name="d3", dtype=ValueType.FLOAT),
        Feature(name="d4", dtype=ValueType.FLOAT),
        Feature(name="d5", dtype=ValueType.FLOAT),
        Feature(name="d6", dtype=ValueType.FLOAT),
        Feature(name="d7", dtype=ValueType.FLOAT),
        Feature(name="d8", dtype=ValueType.FLOAT),
        Feature(name="d9", dtype=ValueType.FLOAT),
    ], online=True, input=my_stats_delta, tags={},)

    
mystats_view_csv = FeatureView(
    name="my_statistics_csv",
    entities=["entity_id"],
    ttl=Duration(seconds=3600*24*20),
    features=[
        Feature(name="c1", dtype=ValueType.FLOAT),
        Feature(name="c2", dtype=ValueType.FLOAT),
    ], online=True, input=my_stats_csv, tags={},)
```


## Historical fetch

```python
from feast import FeatureStore
import pandas as pd
import time

store = FeatureStore(repo_path=".")

start_time = time.time()
training_df = store.get_historical_features(
    entity_df=entity_df, 
    features = [
        'my_statistics_parquet:p1',
        'my_statistics_parquet:p2',
        'my_statistics_delta:d1',
        'my_statistics_delta:d2',
        'my_statistics_csv:c1',
        'my_statistics_csv:c2'
    ],
).to_df()


print("--- %s seconds ---" % (time.time() - start_time))

training_df
```

