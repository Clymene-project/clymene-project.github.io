---
title: "Architecture"
linkTitle: "Architecture"
weight: 2
description: >
---

The Clymene project is a platform for collecting time series data and log data. So there are two pipelines: timeseries and logs. All agents can collect data and store it directly in the database,
Or, depending on the situation, you can use the Clymene components to configure a variety of architectures. It can be configured with an architecture that can be used with a lot of traffic.
The Clymene Project provides various storage types, so choose a storage type you are familiar with. And build great monitoring systems using familiar dashboards.
<img src="https://user-images.githubusercontent.com/25188468/165649203-7382735f-0c1f-4388-93f3-65b57b06d7f4.png" width="90%" height="90%" alt="architecture_v2.1.0">  


## Components

### Pipeline 1. TimeSeries data collection

#### Clymene Agent([Getting Started](https://github.com/Clymene-project/Clymene/blob/main/docs/clymene-agent/README.md))

```dockerhub : bourbonkk/clymene-agent:v2.x.x```  
```redhatQuay: quay.io/clymene/clymene-agent:v2.x.x```  
![Docker Pulls](https://img.shields.io/docker/pulls/bourbonkk/clymene-agent.svg?maxAge=86400) [![Docker Repository on Quay](https://quay.io/repository/clymene/clymene-agent/status "Docker Repository on Quay")](https://quay.io/repository/clymene/clymene-agent)   
The Clymene-agent is service that collects time series data(does not use disks)

1. Service Discovery
    - [Prometheus's Service Discovery](https://docs.sysdig.com/en/docs/sysdig-monitor/integrations-for-sysdig-monitor/collect-prometheus-metrics/enable-prometheus-native-service-discovery/)
      feature finds Metric collection endpoints.
2. scrape time series data
3. Time-series data transfer to gateway(gRPC) (Optional)
4. Time-series data transfer to kafka (Optional)
5. Time-series data insert to Database([supported DB](https://github.com/Clymene-project/Clymene/blob/main/docs/clymene-agent/README.md#Option-description-by-storage-type)) (Optional)

#### Clymene Ingester(Optional) ([Getting Started](https://github.com/Clymene-project/Clymene/blob/main/docs/clymene-ingester/README.md))

```dockerhub : bourbonkk/clymene-ingester:v2.x.x```  
```redhatQuay: quay.io/clymene/clymene-ingester:v2.x.x```  
![Docker Pulls](https://img.shields.io/docker/pulls/bourbonkk/clymene-ingester.svg?maxAge=86400) [![Docker Repository on Quay](https://quay.io/repository/clymene/clymene-ingester/status "Docker Repository on Quay")](https://quay.io/repository/clymene/clymene-ingester)  
The Clymene ingester is an optional service responsible for insert time series data loaded on kafka into the database.

1. Kafka message consume
2. Time-series data insert to Database([supported DB](https://github.com/Clymene-project/Clymene/blob/main/docs/clymene-ingester/README.md#Option-description-by-storage-type)) (Optional)

#### Clymene Gateway(Optional) ([Getting Started](https://github.com/Clymene-project/Clymene/blob/main/docs/clymene-gateway/README.md))

```dockerhub : bourbonkk/clymene-gateway:v2.x.x```  
```redhatQuay: quay.io/clymene/clymene-gateway:v2.x.x```  
![Docker Pulls](https://img.shields.io/docker/pulls/bourbonkk/clymene-gateway.svg?maxAge=86400) [![Docker Repository on Quay](https://quay.io/repository/clymene/clymene-gateway/status "Docker Repository on Quay")](https://quay.io/repository/clymene/clymene-gateway)  
The Clymene Gateway is an optional service that can receive metric data from the another component through gRPC or HTTP
communication.

1. gRPC, HTTP Service
2. Time-series data insert to Database([supported DB](https://github.com/Clymene-project/Clymene/blob/main/docs/clymene-gateway/README.md#Option-description-by-storage-type)) (Optional)


### Pipeline 2. Logs collection

#### Clymene Promtail([Getting Started](https://github.com/Clymene-project/Clymene/blob/main/docs/clymene-promtail/README.md))

```dockerhub : bourbonkk/clymene-promtail:v2.x.x```  
```redhatQuay: quay.io/clymene/clymene-promtail:v2.x.x```  
![Docker Pulls](https://img.shields.io/docker/pulls/bourbonkk/clymene-promtail.svg?maxAge=86400) [![Docker Repository on Quay](https://quay.io/repository/clymene/clymene-promtail/status "Docker Repository on Quay")](https://quay.io/repository/clymene/clymene-agent)   
The Clymene-promtail customized loki's log collection agent for the Clymene project.

1. [Service Discovery](https://clymene-project.github.io/docs/service-discovery/promtail-config/)
2. log collection
3. log data transfer to gateway(gRPC or HTTP)
4. log data transfer to kafka
5. log data insert to Database([supported DB](https://github.com/Clymene-project/Clymene/blob/main/docs/clymene-promtail/README.md#Option-description-by-storage-type)) (Optional)

#### Promtail Ingester(Optional) ([Getting Started](https://github.com/Clymene-project/Clymene/blob/main/docs/promtail-ingester/README.md))

```dockerhub : bourbonkk/promtail-ingester:v2.x.x```  
```redhatQuay: quay.io/clymene/promtail-ingester:v2.x.x```  
![Docker Pulls](https://img.shields.io/docker/pulls/bourbonkk/promtail-ingester.svg?maxAge=86400) [![Docker Repository on Quay](https://quay.io/repository/clymene/promtail-ingester/status "Docker Repository on Quay")](https://quay.io/repository/clymene/promtail-ingester)  
Promtail ingester is an optional service responsible for insert log data loaded on kafka into the database.

1. Kafka message consume
2. Time-series data insert to Database([supported DB](https://github.com/Clymene-project/Clymene/blob/main/docs/promtail-ingester/README.md#Option-description-by-storage-type)) (Optional)

#### Promtail Gateway(Optional) ([Getting Started](https://github.com/Clymene-project/Clymene/blob/main/docs/promtail-gateway/README.md))

```dockerhub : bourbonkk/promtail-gateway:v2.x.x```  
```redhatQuay: quay.io/clymene/promtail-gateway:v2.x.x```  
![Docker Pulls](https://img.shields.io/docker/pulls/bourbonkk/promtail-gateway.svg?maxAge=86400) [![Docker Repository on Quay](https://quay.io/repository/clymene/promtail-gateway/status "Docker Repository on Quay")](https://quay.io/repository/clymene/promtail-gateway)  
The Promtail Gateway is an optional service that can receive log data from the another component through gRPC or HTTP
communication.

1. gRPC, HTTP Service
2. Time-series data insert to Database([supported DB](https://github.com/Clymene-project/Clymene/blob/main/docs/promtail-gateway/README.md#Option-description-by-storage-type)) (Optional)
