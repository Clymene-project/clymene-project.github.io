
---
title: "Documentation"
linkTitle: "Documentation"
weight: 20
menu:
  main:
    weight: 20
---

![clymene_logo_v1_side](https://user-images.githubusercontent.com/25188468/149839035-f357caea-d335-4b24-88e7-e957f79dc6be.png)

[![CII Best Practices](https://bestpractices.coreinfrastructure.org/projects/5491/badge)](https://bestpractices.coreinfrastructure.org/projects/5491) ![CodeQL](https://github.com/clymene-project/clymene/workflows/CodeQL/badge.svg) [![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0) ![GitHub release (latest by date)](https://img.shields.io/github/v/release/clymene-project/clymene) [![Go Reference](https://pkg.go.dev/badge/github.com/Clymene-project/Clymene.svg)](https://pkg.go.dev/github.com/Clymene-project/Clymene)  
![Go](https://img.shields.io/badge/go-%2300ADD8.svg?style=for-the-badge&logo=go&logoColor=white) ![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white) ![Kubernetes](https://img.shields.io/badge/kubernetes-%23326ce5.svg?style=for-the-badge&logo=kubernetes&logoColor=white) ![ElasticSearch](https://img.shields.io/badge/-ElasticSearch-005571?style=for-the-badge&logo=elasticsearch) <img src="https://img.shields.io/badge/influxdb-%2322ADF6.svg?&style=for-the-badge&logo=influxdb&logoColor=white"/>  <img src="https://img.shields.io/badge/prometheus-%23E6522C.svg?&style=for-the-badge&logo=prometheus&logoColor=white" />   <img src="https://img.shields.io/badge/OpenTSDB-green?style=for-the-badge"> <img src="https://img.shields.io/badge/cortex-blue?style=for-the-badge">  <img src="https://img.shields.io/badge/tdengine-gray?style=for-the-badge">  


The Clymene is a time series data collection platform for distributed systems inspired by [Prometheus](https://prometheus.io)
and [Jaeger](https://www.jaegertracing.io). Time series data from various environments can be collected and stored in different types of databases. It can be configured in a variety of architectures.


- **Time-series data collection platform:** Clymene uses fewer resources than [Prometheus](https://prometheus.io) to collect metrics. In addition, various architectures can be constructed using ingester and gateway components.
- **heterogeneous database support:** Time series data created with Prometheus exporter can be stored in various databases such as ElasticSearch, Prometheus, Cortex, kdb++, OpenTSDB, InfluxDB and TDengine
