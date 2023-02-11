---
categories: ["Examples"]
tags: ["test", "sample", "docs"]
title: "Promtail Gateway Getting Start"
linkTitle: "Promtail Gateway"
date: 2017-01-18
weight: 6
description: >
  The Promtail Gateway is an optional service that can receive logs data from the agent through gRPC, HTTP communication.
---
The Promtail Gateway is an optional service that can receive logs data from the agent through gRPC, HTTP communication.

1. gRPC, HTTP Service
2. Logs data insert to Database(ElasticSearch, Loki, ETC) (Optional)

## How to setting promtail-gateway
```
--gateway.grpc-server.host-port string          The host:port (e.g. 127.0.0.1:15610 or :15610) of the gateway's GRPC server (default ":15610")
--gateway.grpc-server.tls.cert string           Path to a TLS Certificate file, used to identify this server to clients
--gateway.grpc-server.tls.client-ca string      Path to a TLS CA (Certification Authority) file used to verify certificates presented by clients (if unset, all clients are permitted)
--gateway.grpc-server.tls.enabled               Enable TLS on the server
--gateway.grpc-server.tls.key string            Path to a TLS Private Key file, used to identify this server to clients
--gateway.http-server.host-port string          The host:port (e.g. 127.0.0.1:15610 or :15611) of the gateway's HTTP server (default ":15611")
--gateway.http-server.tls.cert string           Path to a TLS Certificate file, used to identify this server to clients
--gateway.http-server.tls.client-ca string      Path to a TLS CA (Certification Authority) file used to verify certificates presented by clients (if unset, all clients are permitted)
--gateway.http-server.tls.enabled               Enable TLS on the server
--gateway.http-server.tls.key string            Path to a TLS Private Key file, used to identify this server to clients
```

## How to set up the Storage Type
#### 1. Setting environmental variables


ElasticSearch
```
STORAGE_TYPE=elasticsearch
```
Loki
```
STORAGE_TYPE=loki
```

Promtail-gateway
```
STORAGE_TYPE=gateway
```

Kafka
```
STORAGE_TYPE=kafka
```

#### 2. Option description by storage type

- [ElasticSearch option](http://clymene-project.github.io/docs/database-options/elasticsearch)
- [Loki option](http://clymene-project.github.io/docs/database-options/loki)
- [Kafka option](http://clymene-project.github.io/docs/database-options/kafka)
- [Promtail-gateway](http://clymene-project.github.io/docs/database-options/gateway)


### Docker-compose Example
```yaml
version: '2'
services:
  clymene-ingester:
    image: bourbonkk/promtail-gateway:latest
    ports:
      - "15610:15610"
    environment:
      - STORAGE_TYPE=elasticsearch
    command:
      - --log-level=debug
      - --es.server-urls=http://[ELASTICSEARCH-IP]:9200
```

### k8s Example
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: promtail-gateway
  namespace: clymene
  labels:
    app: promtail-gateway
spec:
  selector:
    matchLabels:
      app: promtail-gateway
  replicas: 1
  template:
    metadata:
      labels:
        app: promtail-gateway
    spec:
      containers:
        - name: promtail-gateway
          image: bourbonkk/promtail-gateway:latest
          imagePullPolicy: Always
          ports:
            - containerPort: 15610
          args:
            - --es.server-urls=http://[ELASTICSEARCH-IP]:9200
            - --log-level=info
          env:
            - name: STORAGE_TYPE
              value: elasticsearch
      securityContext:
        runAsUser: 1000
```

### Use gateway Architecture
<img src="https://user-images.githubusercontent.com/25188468/214289870-3ed5c5d9-c831-40b4-8216-169eb8c2786c.png" width="70%" height="70%">
