---
title: "4.clymene-ingester"
date: 2017-01-05
weight: 4
description: >
  configuration for clymene ingester
---
### configuration for clymene ingester
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: clymene-ingester
  namespace: clymene
  labels:
    app: clymene-ingester
spec:
  selector:
    matchLabels:
      app: clymene-ingester
  replicas: 1
  template:
    metadata:
      labels:
        app: clymene-ingester
    spec:
      containers:
        - name: ingester
          image: bourbonkk/clymene-ingester:latest
          imagePullPolicy: Always
          ports:
            - containerPort: 15694
          args:
            - --prometheus.remote.url=http://prometheus-server-http.prometheus:9090/api/v1/write
            - --kafka.consumer.brokers=kafka.kafka:9092
            - --es.server-urls=http://elasticsearch.es:9200
            - --log-level=info
          env:
            - name: STORAGE_TYPE
              value: prometheus,elasticsearch
      securityContext:
        runAsUser: 1000
```
