---
title: "Overview"
linkTitle: "Overview"
weight: 1
description: >
  Overview introduces why Clymene should be used, architecture, and components.
---
<img align="right" width="400" height="400" src="https://user-images.githubusercontent.com/25188468/148681479-3ddf237c-6e5d-49a1-a517-8b3bfa92f54e.png" alt="clymene_logo">
Clymene, developed as an architecture that collects time series data and logs from various environments, can provide observability by collecting time series data and logs from multiple complex and various environments and systems in one place.

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
- **Logs collection platform:** Clymene-promtail customized loki's log collection agent for the Clymene project. Logs can be stored in Loki and Elasticsearch and backend configuration with high availability architecture using a variety of components


## Roadmap
1. AI/ML platform for clymene(https://github.com/Clymene-project/clymene-analyzer)
2. push-type agent(node-exporter, cadvisor, process-exporter, etc)
3. Add new pipeline(trace or Something - [discussions](https://github.com/Clymene-project/Clymene/discussions))

