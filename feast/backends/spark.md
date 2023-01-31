---
layout: default
title: Spark
parent: Backends
nav_order: 1
description: "Yummy spark backend."
permalink: /feast/backends/spark
---

# Spark

Spark backend is based on the [Apache Spark](https://spark.apache.org/) project.
To use spark backend you will need working spark instance, you can also use [Databricks](https://databricks.com/)

To configure the spark offline store edit `feature_store.yaml`
```yaml
project: feature_repo
registry: data/registry.db
provider: local
online_store:
    ...
offline_store:
    type: yummy.YummyOfflineStore
    backend: spark
    config:
        spark.master: "local[*]"
        spark.ui.enabled: "false"
        spark.eventLog.enabled: "false"
        spark.sql.session.timeZone: "UTC"
```

in the `config` section you can put additional spark configuration.




