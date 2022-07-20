---
layout: default
title: Home
nav_order: 1
description: "Yummy documentation."
permalink: /
---

# [Yummy](https://github.com/yummyml/yummy) - delicious [Feast](https://github.com/feast-dev/feast) extension
<img src="{{ site.relative_url }}assets/images/yummy_transparent.png" alt="yummy" width="500" />

Yummy project adds possiblity to run [Feast](https://github.com/feast-dev/feast) on multiple backends:
* [polars](https://github.com/pola-rs/polars)
* [dask](https://github.com/dask/dask)
* [ray](https://github.com/ray-project/ray)
* [spark](https://github.com/apache/spark)

This gives flexibility in setting up the feature store on existing environments and using its capabilities.
Moreover using Yummy you can combine multiple and different datasources during historical fetch task.

## Why ?

Here is short explanation why we need yummy:

<div class="video-container">
    <iframe src="https://www.youtube.com/embed/YinQxF4Gx54" frameborder="0" allowfullscreen></iframe>
</div>

## Getting started

| Sources/Backends  | [Spark](/backends/spark) | [Polars](/backends/polars) | [Dask](/backends/dask) | [Ray](/backends/ray) |
| ------------- | ------------- | ------------- | ------------- | ------------- |
| [Iceberg   Source](/sources/iceberg)  | ✔️  |  ❌  |  ❌  |  ❌ |
| [Delta     Source](/sources/delta)  | ✔️  |  ✔️  |  ✔️  |  ✔️ |
| [Parquet   Source](/sources/parquet)  | ✔️  |  ✔️  |  ✔️  |  ✔️ |
| [CSV       Source](/sources/csv)  | ✔️  |  ✔️  |  ✔️  |  ✔️ |




