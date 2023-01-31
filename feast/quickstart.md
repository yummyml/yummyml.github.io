---
layout: default
title: Quickstart
parent: Feast
nav_order: 1
description: "Yummy documentation."
permalink: /quickstart
---

# Yummy Quickstart

### Install yummy:

```bash
pip install yummy
```

```bash
pip install git+https://github.com/yummyml/yummy.git
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
project: repo
registry: s3://data/registry.db
provider: yummy.YummyProvider
backend: polars
online_store:
    ...
offline_store:
    type: yummy.YummyOfflineStore
    backend: polars
```

#### Dask

To configure the offline store edit `feature_store.yaml`
```yaml
project: repo
registry: s3://data/registry.db
provider: yummy.YummyProvider
backend: dask
online_store:
    ...
offline_store:
    type: yummy.YummyOfflineStore
```

#### Ray

To configure the offline store edit `feature_store.yaml`
```yaml
project: repo
registry: s3://data/registry.db
provider: yummy.YummyProvider
backend: ray
online_store:
    ...
offline_store:
    type: yummy.YummyOfflineStore
```

#### Spark

To configure the offline store edit `feature_store.yaml`
```yaml
project: repo
registry: s3://data/registry.db
provider: yummy.YummyProvider
backend: spark
backend_config:
    spark.master: "local[*]"
    spark.ui.enabled: "false"
    spark.eventLog.enabled: "false"
    spark.sql.session.timeZone: "UTC"
online_store:
    ...
offline_store:
    type: yummy.YummyOfflineStore
```

### Features definition

Example `features.py`:
```python
from datetime import timedelta
from feast import Entity, Field, FeatureView
from yummy import ParquetSource, CsvSource, DeltaSource
from feast.types import Float32, Int32

my_stats_parquet = ParquetSource(
    path="/home/jovyan/notebooks/ray/dataset/all_data.parquet",
    timestamp_field="datetime",
)

my_stats_delta = DeltaSource(
    path="dataset/all",
    timestamp_field="datetime",
    #range_join=10,
)

my_stats_csv = CsvSource(
    path="/home/jovyan/notebooks/ray/dataset/all_data.csv",
    timestamp_field="datetime",
)

my_entity = Entity(name="entity_id", description="entity id",)

mystats_view_parquet = FeatureView(
    name="my_statistics_parquet",
    entities=[my_entity],
    ttl=timedelta(seconds=3600*24*20),
    schema=[
        Field(name="entity_id", dtype=Int32),
        Field(name="p0", dtype=Float32),
        Field(name="p1", dtype=Float32),
        Field(name="p2", dtype=Float32),
        Field(name="p3", dtype=Float32),
        Field(name="p4", dtype=Float32),
        Field(name="p5", dtype=Float32),
        Field(name="p6", dtype=Float32),
        Field(name="p7", dtype=Float32),
        Field(name="p8", dtype=Float32),
        Field(name="p9", dtype=Float32),
        Field(name="y", dtype=Float32),
    ], online=True, source=my_stats_parquet, tags={},)

mystats_view_delta = FeatureView(
    name="my_statistics_delta",
    entities=[my_entity],
    ttl=timedelta(seconds=3600*24*20),
    schema=[
        Field(name="entity_id", dtype=Int32),
        Field(name="d0", dtype=Float32),
        Field(name="d1", dtype=Float32),
        Field(name="d2", dtype=Float32),
        Field(name="d3", dtype=Float32),
        Field(name="d4", dtype=Float32),
        Field(name="d5", dtype=Float32),
        Field(name="d6", dtype=Float32),
        Field(name="d7", dtype=Float32),
        Field(name="d8", dtype=Float32),
        Field(name="d9", dtype=Float32),
    ], online=True, source=my_stats_delta, tags={},)

    
mystats_view_csv = FeatureView(
    name="my_statistics_csv",
    entities=[my_entity],
    ttl=timedelta(seconds=3600*24*20),
    schema=[
        Field(name="entity_id", dtype=Int32),
        Field(name="c1", dtype=Float32),
        Field(name="c2", dtype=Float32),
    ], online=True, source=my_stats_csv, tags={},)
```


## Historical fetch

```python
from feast import FeatureStore
import pandas as pd
import time
from datetime import datetime
from yummy import select_all

store = FeatureStore(repo_path=".")

start_time = time.time()
training_df = store.get_historical_features(
    entity_df=select_all(datetime(2022, 9, 14, 23, 59, 42)), 
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

To fetch historical features you need to specify: `entity_df` and `features`.

### entity_df

`entity_df` is the list of entities with `event_timestamps` you want to fetch.
You can manually build the data frame with the list of entites:
```
import pandas as pd
from datetime import datetime

entity_df = pd.DataFrame.from_dict(
    {
        "entity_id": [1, 2, 3],
        "event_timestamp": [
            datetime.now() - timedelta(minutes=1),
            datetime.now() - timedelta(minutes=2),
            datetime.now() - timedelta(minutes=3),
        ],
    }
)
```

Or in yummy you can use `select_all`:
```
from yummy import select_all
from datetime import datetime

entity_df = select_all(datetime(2022, 9, 14, 23, 59, 42))
```

with `select_all` all entities will be selected from the first feature view.

