---
title: "7.promtail-ingester"
date: 2017-01-05
weight: 7
description: >
  configuration for promtail ingester
---
### configuration for promtail ingester
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: promtail-ingester
  namespace: clymene
  labels:
    app: promtail-ingester
spec:
  selector:
    matchLabels:
      app: promtail-ingester
  replicas: 1
  template:
    metadata:
      labels:
        app: promtail-ingester
    spec:
      containers:
        - name: promtail-agent
          image: bourbonkk/promtail-ingester:latest
          imagePullPolicy: Always
          ports:
            - containerPort: 15694
          args:
            - --kafka.consumer.brokers=kafka.kafka:9092
            - --loki.client.url=http://loki.loki:3100/loki/api/v1/push
            - --es.server-urls=http://elasticsearch.es:9200
            - --log-level=info
          env:
            - name: STORAGE_TYPE
              value: loki,elasticsearch
      securityContext:
        runAsUser: 1000
```
