---
title: "3.clymene-gateway"
date: 2017-01-05
weight: 3
description: >
  configuration for clymene gateway
---
### configuration for clymene gateway
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: clymene-gateway
  namespace: clymene
  labels:
    app: clymene-gateway
spec:
  selector:
    matchLabels:
      app: clymene-gateway
  replicas: 1
  template:
    metadata:
      labels:
        app: clymene-gateway
    spec:
      containers:
        - name: gateway
          image: bourbonkk/clymene-gateway:latest
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
  name: clymene-gateway
  namespace: clymene
  labels:
    app: clymene-gateway
spec:
  type: NodePort
  ports:
    - name: grpc
      port: 15610
      targetPort: 15610
      nodePort: 30610
  selector:
    app: clymene-gateway
```
