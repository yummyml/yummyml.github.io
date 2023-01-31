---
layout: default
title: Dask
parent: Backends
nav_order: 3
description: "Yummy dask backend."
permalink: /feast-backends-dask
---

# Dask

Dask backend is based on the [Dask](https://www.dask.org/) library.
You can use `dask` on the single machine and on the cluster thus you can
simply start with the simple solution and then scale it.
Additional `dask` advantage is that after featching historical features you can 
continue with the distributed training. 

To configure the polars offline store edit `feature_store.yaml`
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


