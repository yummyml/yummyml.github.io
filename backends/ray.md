---
layout: default
title: Ray
parent: Backends
nav_order: 4
description: "Yummy ray backend."
permalink: /backends/ray
---

# Ray

Ray backend is based on the [Dask on Ray](https://docs.ray.io/en/latest/data/dask-on-ray.html) 
implementation. As a `dask` backend you can start on single machine 
and then scale and distributed your solution. You can also continue with 
distributed training.

To configure the polars offline store edit `feature_store.yaml`
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



