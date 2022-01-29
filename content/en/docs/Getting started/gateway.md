---
categories: ["Examples"]
tags: ["test", "sample", "docs"]
title: "Clymene Gateway Getting Start"
linkTitle: "Clymene Gateway"
date: 2017-01-05
description: >
  The Clymene Gateway is an optional service that can receive metric data from the agent through gRPC communication
---
The Clymene Gateway is an optional service that can receive metric data from the agent through gRPC communication.  

1. gRPC Service
2. Time-series data insert to Database(ElasticSearch, Prometheus, ETC) (Optional)

### How to setting gRPC server
```
--gateway.grpc-server.host-port string      The host:port (e.g. 127.0.0.1:15610 or :15610) of the gateway's GRPC server (default ":15610")
--gateway.grpc.tls.cert string              Path to a TLS Certificate file, used to identify this server to clients
--gateway.grpc.tls.client-ca string         Path to a TLS CA (Certification Authority) file used to verify certificates presented by clients (if unset, all clients are permitted)
--gateway.grpc.tls.enabled                  Enable TLS on the server
--gateway.grpc.tls.key string               Path to a TLS Private Key file, used to identify this server to clients
```

### How to set up the Storage Type
##### 1. Setting environmental variables

ElasticSearch
```
TS_STORAGE_TYPE=elasticsearch
```
Kafka
```
TS_STORAGE_TYPE=kafka
```
prometheus
```
TS_STORAGE_TYPE=prometheus
```
cortex
```
TS_STORAGE_TYPE=cortex
```
opentsdb
```
TS_STORAGE_TYPE=opentsdb
```
influxdb
```
TS_STORAGE_TYPE=influxdb
```
tdengine
```
TS_STORAGE_TYPE=tdengine
```
Several
```
TS_STORAGE_TYPE=elasticsearch,prometheus  # composite write - Write in multiple databases at the same time.
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
<img src="https://user-images.githubusercontent.com/25188468/150611990-61dc11df-1deb-4755-bf28-3a531bd77888.png" width="70%" height="70%" alt="architecture_v1.4.0_gateway">