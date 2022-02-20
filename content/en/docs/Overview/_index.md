---
title: "Overview"
linkTitle: "Overview"
weight: 1
description: >
  Overview introduces why Clymene should be used, architecture, and components.
---
<img align="right" width="400" height="400" src="https://user-images.githubusercontent.com/25188468/148681479-3ddf237c-6e5d-49a1-a517-8b3bfa92f54e.png" alt="clymene_logo">
Clymene, developed as an architecture that collects time series data from various environments, can provide observability by collecting time series data from multiple complex and various environments and systems in one place.

Clymene agent does not use WAL(Write Ahead Log), it uses fewer resources than [Prometheus](https://prometheus.io) when collecting metrics.

In addition, various architectures can be constructed using ingester and gateway components.

## Clymene-project?

Clymene is an open source developed based on Prometheus’ time-series data collection method and the architecture of the Jaegertracing project, an open source of service tracing in a distributed environment. 
Prometheus’ Service Discovery features make it easy to find endpoints that can collect time series data from multiple environments, and can be configured with a powerful high-availability (HA) architecture using Jaeger’s backend structure.

## Why do I have to use Clymene?

Help your user know if your project will help them. Useful information can include: 

- **Time-series data collection platform:** Clymene uses fewer resources than [Prometheus](https://prometheus.io) to collect metrics. In addition, various architectures can be constructed using ingester and gateway components.
- **Collect metrics from various environments:** Multiple Kubernetes clusters and non-kubernetes environments, not one Kubernetes cluster, also when you want to collect and monitor metrics in one place.
- **heterogeneous database support:** Time series data created with Prometheus exporter can be stored in various databases such as ElasticSearch, Prometheus, Cortex, kdb++, OpenTSDB, InfluxDB and TDengine
- **Long term storage:** Clymene supports a variety of databases, allowing users to handle time series data in familiar databases.

## Components

#### Clymene Agent
The Clymene-agent is service that collects time series data(does not use disks)

1. Service Discovery
    - [Prometheus's Service Discovery](https://docs.sysdig.com/en/docs/sysdig-monitor/integrations-for-sysdig-monitor/collect-prometheus-metrics/enable-prometheus-native-service-discovery/)
      feature finds Metric collection endpoints.
2. scrape time series data
3. Time-series data transfer to gateway(gRPC) (Optional)
4. Time-series data transfer to kafka (Optional)
5. Time-series data insert to Database([supported DB](https://github.com/Clymene-project/Clymene/blob/main/docs/clymene-agent/README.md#Option-description-by-storage-type)) (Optional)

#### Clymene Promtail
The Clymene-promtail customized loki's log collection agent for the Clymene project.

1. [Service Discovery](https://clymene-project.github.io/docs/service-discovery/promtail-config/)
2. log collection
3. log data transfer to gateway(gRPC) (TODO)
4. log data transfer to kafka (TODO)
5. log data insert to Database([supported DB](https://github.com/Clymene-project/Clymene/blob/main/docs/clymene-promtail/README.md#Option-description-by-storage-type)) (Optional)

#### Clymene Ingester
The Clymene ingester is an optional service responsible for insert time series data loaded on kafka into the database.

1. Kafka message consume
2. Time-series data insert to Database([supported DB](https://github.com/Clymene-project/Clymene/blob/main/docs/clymene-ingester/README.md#Option-description-by-storage-type)) (Optional)


#### Clymene Gateway
The Clymene Gateway is an optional service that can receive metric data from the another component through gRPC
communication.

1. gRPC Service
2. Time-series data insert to Database([supported DB](https://github.com/Clymene-project/Clymene/blob/main/docs/clymene-gateway/README.md#Option-description-by-storage-type)) (Optional)


## Roadmap
* Clymene's short-term goal is to support heterogeneous databases in Grafana as a query.
* [AI/ML platform for clymene](https://github.com/Clymene-project/clymene-analyzer)
* [Please give me an idea!](https://github.com/Clymene-project/Clymene/discussions)
* [Add log management collection function](https://github.com/grafana/loki)

