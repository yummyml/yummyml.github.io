---
layout: default
title: Polars
parent: Backends
nav_order: 2
description: "Yummy polars backend."
permalink: /feast/backends/polars
---

# Polars

Polars backend is based on the [Polars](https://www.pola.rs/) DataFrame library.
Currently the Polars backend works only in the single node mode (you cannot distribute calculations accross the cluster).
But it is really fast and this is the best option if you want to setup simple MLOps solution. 

To configure the polars offline store edit `feature_store.yaml`
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

