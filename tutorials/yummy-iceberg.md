---
layout: default
title: Yummy Iceberg
parent: Tutorials
nav_order: 1
description: "Yummy Iceberg - tutorial"
permalink: /tutorials/yummy-iceberg
---

How to use [Iceberg](https://iceberg.apache.org/) with [Feast](https://feast.dev/) feature store throught [Yummy](https://www.yummyml.com/) extension.
Additionaly how to setup end to end solution:
![Kafka connect](/assets/images/tutorials/yummy-iceberg/diagram.jpg)

with:
* Postgres
* Kafka connect:
	* [postgress connector](https://debezium.io/documentation/reference/stable/connectors/postgresql.html)
	* [iceberg sink](https://github.com/getindata/kafka-connect-iceberg-sink)
* [Iceberg](https://iceberg.apache.org/) on [Minio](https://min.io/)
* [Feast](https://feast.dev/) & [Yummy](https://www.yummyml.com/)

The kafka connect inegration is based on the article: 
[https://getindata.com/blog/real-time-ingestion-iceberg-kafka-connect-apache-iceberg-sink/](https://getindata.com/blog/real-time-ingestion-iceberg-kafka-connect-apache-iceberg-sink/)

<div class="video-container">
    <iframe src="https://www.youtube.com/embed/kv0iWuSf4jw" frameborder="0" allowfullscreen></iframe>
</div>

Full example: [https://github.com/yummyml/yummy-iceberg-kafka-connect/](https://github.com/yummyml/yummy-iceberg-kafka-connect/)


