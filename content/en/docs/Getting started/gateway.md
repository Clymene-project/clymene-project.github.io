---
categories: ["Examples"]
tags: ["test", "sample", "docs"]
title: "Clymene Gateway Getting Start"
linkTitle: "Clymene Gateway"
date: 2017-01-05
weight: 4
description: >
  The Clymene Gateway is an optional service that can receive metric data from the agent through gRPC, HTTP communication
---
The Clymene Gateway is an optional service that can receive metric data from the agent through gRPC, HTTP communication.  

1. gRPC Service
2. Time-series data insert to Database(ElasticSearch, Prometheus, ETC) (Optional)

### How to setting gateway
```
--admin.http.host-ports string              The host:ports (e.g. 127.0.0.1:15690 or :15690) for the admin server, including health check, /metrics, etc. (default ":15690")
--gateway.grpc-server.host-port string      The host:port (e.g. 127.0.0.1:15610 or :15610) of the gateway's GRPC server (default ":15610")
--gateway.grpc.tls.cert string              Path to a TLS Certificate file, used to identify this server to clients
--gateway.grpc.tls.client-ca string         Path to a TLS CA (Certification Authority) file used to verify certificates presented by clients (if unset, all clients are permitted)
--gateway.grpc.tls.enabled                  Enable TLS on the server
--gateway.grpc.tls.key string               Path to a TLS Private Key file, used to identify this server to clients
--gateway.http-server.host-port string      The host:port (e.g. 127.0.0.1:15610 or :15611) of the gateway's HTTP server (default ":15611")
--gateway.http.tls.cert string              Path to a TLS Certificate file, used to identify this server to clients
--gateway.http.tls.client-ca string         Path to a TLS CA (Certification Authority) file used to verify certificates presented by clients (if unset, all clients are permitted)
--gateway.http.tls.enabled                  Enable TLS on the server
--gateway.http.tls.key string               Path to a TLS Private Key file, used to identify this server to clients
--log-level string                          Minimal allowed log Level. For more levels see https://github.com/uber-go/zap (default "info")
--metrics-backend string                    Defines which metrics backend to use for metrics reporting: expvar, prometheus, none (default "prometheus")
--metrics-http-route string                 Defines the route of HTTP endpoint for metrics backends that support scraping (default "/metrics")
```

### How to set up the Storage Type
##### 1. Setting environmental variables

ElasticSearch
```
STORAGE_TYPE=elasticsearch
```
Kafka
```
STORAGE_TYPE=kafka
```
prometheus
```
STORAGE_TYPE=prometheus
```
cortex
```
STORAGE_TYPE=cortex
```
opentsdb
```
STORAGE_TYPE=opentsdb
```
influxdb
```
STORAGE_TYPE=influxdb
```
tdengine
```
STORAGE_TYPE=tdengine
```
Several
```
STORAGE_TYPE=elasticsearch,prometheus  # composite write - Write in multiple databases at the same time.
```

##### 2. Option description by storage type
- [Kafka option](http://clymene-project.github.io/docs/database-options/kafka)
- [ElasticSearch option](http://clymene-project.github.io/docs/database-options/elasticsearch)
- [Prometheus option](http://clymene-project.github.io/docs/database-options/prometheus)
- [cortex option](http://clymene-project.github.io/docs/database-options/cortex)
- [gateway option](http://clymene-project.github.io/docs/database-options/gateway)
- [Opentsdb option](http://clymene-project.github.io/docs/database-options/opentsdb)
- [influxdb option](http://clymene-project.github.io/docs/database-options/influxdb)
- [TDengine options](http://clymene-project.github.io/docs/database-options/tdengine)

### Use gateway Architecture
<img src="https://user-images.githubusercontent.com/25188468/152249028-701c3372-dee8-49dd-b284-dd04e9da0597.png" width="70%" height="70%" alt="architecture_v1.4.0_gateway">
