---
title: "6.promtail-gateway"
date: 2017-01-05
weight: 6
description: >
  configuration for promtail gateway
---
### configuration for promtail gateway
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
        - name: promtail-agent
          image: bourbonkk/promtail-gateway:latest
          imagePullPolicy: Always
          ports:
            - containerPort: 15694
          args:
            - --kafka.producer.brokers=kafka.kafka:9092
            - --log-level=info
          env:
            - name: STORAGE_TYPE
              value: kafka
      securityContext:
        runAsUser: 1000
---
apiVersion: v1
kind: Service
metadata:
  name: promtail-gateway
  namespace: clymene
  labels:
    app: promtail-gateway
spec:
  type: NodePort
  ports:
    - name: grpc
      port: 15610
      targetPort: 15610
      nodePort: 30611
  selector:
    app: promtail-gateway

```
