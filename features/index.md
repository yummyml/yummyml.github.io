---
layout: default
title: Features
nav_order: 4
description: "Yummy feature server."
permalink: /features
---

# Yummy Feature Server

The yummy feature server is based on the rust implementation wrapped by [pyo3](https://github.com/PyO3/pyo3) thus you don't need 
to install rust environment and you can simply run it using command line:
```bash
yummy serve -h 0.0.0.0 -p 6566 -f feature_store.yaml
```

Rust feature server have been implemented using [Actix](https://actix.rs/) server.
The server is fully compatible with [feast feature server](https://docs.feast.dev/reference/feature-servers) 
and is blazing fast (~1ms - full benchmark will delivered soon).
You can use it with the features materialized with Feast to online store.
Currently `Redis` online store implementation is available and `http` protocol is supported.

The payload is fully compatible with Feast server:

### Example request/response:

Request with features list:
```json
{          
  "features": [
                "driver_hourly_stats:conv_rate",
                "driver_hourly_stats:acc_rate",
                "driver_hourly_stats:avg_daily_trips",                      
            ],
  "entities": {"driver_id": [1001,1002,1003,1004,1005]},
  "full_feature_names": true,
"
```

Request with [feature service](https://docs.feast.dev/getting-started/concepts/feature-retrieval#feature-services):
```json
{
  "feature_service": "driver_activity_basic",
  "entities": {"driver_id": [1001,1002,1003,1004,1005]},
  "full_feature_names": true,
}
```

Example response:
```json
{"metadata": {"feature_names": ["driver_id",
   "driver_hourly_stats__conv_rate",
   "driver_hourly_stats__acc_rate",
   "driver_hourly_stats__avg_daily_trips"]},
 "results": [{"values": [1001, 1002, 1003, 1004, 1005],
   "statuses": ["PRESENT", "PRESENT", "PRESENT", "PRESENT", "PRESENT"],
   "event_timestamps": ["2022-10-04T22:46:16",
    "2022-10-04T22:46:16",
    "2022-10-04T22:46:16",
    "2022-10-04T22:46:16",
    "2022-10-04T22:46:16"]},
  {"values": [0.44467267, null, 0.775576, 0.719485, null],
   "statuses": ["PRESENT", "PRESENT", "PRESENT", "PRESENT", "PRESENT"],
   "event_timestamps": ["2022-10-04T22:46:16",
    "2022-10-04T22:46:16",
    "2022-10-04T22:46:16",
    "2022-10-04T22:46:16",
    "2022-10-04T22:46:16"]},
  {"values": [0.36920926, null, 0.8855987, 0.09924329, null],
   "statuses": ["PRESENT", "PRESENT", "PRESENT", "PRESENT", "PRESENT"],
   "event_timestamps": ["2022-10-04T22:46:16",
    "2022-10-04T22:46:16",
    "2022-10-04T22:46:16",
    "2022-10-04T22:46:16",
    "2022-10-04T22:46:16"]},
  {"values": [821, null, 381, 587, null],
   "statuses": ["PRESENT", "PRESENT", "PRESENT", "PRESENT", "PRESENT"],
   "event_timestamps": ["2022-10-04T22:46:16",
    "2022-10-04T22:46:16",
    "2022-10-04T22:46:16",
    "2022-10-04T22:46:16",
    "2022-10-04T22:46:16"]}]}
```

