---
title: "2.clymene-agent"
date: 2017-01-05
weight: 2
description: >
  configuration for clymene agent
---
### configuration for clymene agent
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: clymene-agent
  namespace: clymene
  labels:
    app: clymene-agent
spec:
  selector:
    matchLabels:
      app: clymene-agent
  replicas: 1
  template:
    metadata:
      labels:
        app: clymene-agent
    spec:
      serviceAccount: clymene-agent
      serviceAccountName: clymene-agent
      containers:
        - name: agent
          image: bourbonkk/clymene-agent:latest
          imagePullPolicy: Always
          ports:
            - containerPort: 15691
            - containerPort: 15692
          args:
            - --config.file=/etc/clymene/clymene.yml
            - --gateway.grpc-client.host-port=clymene-gateway:15610
            - --log-level=info
          env:
            - name: STORAGE_TYPE
              value: gateway
          volumeMounts:
            - mountPath: /etc/clymene/
              name: config-volume
      volumes:
        - name: config-volume
          configMap:
            name: clymene-agent-config
      securityContext:
        runAsUser: 1000
---
apiVersion: v1
kind: Service
metadata:
  name: clymene-agent
  namespace: clymene
  labels:
    app: clymene-agent
spec:
  type: ClusterIP
  ports:
    - name: metric
      port: 15691
      targetPort: 15691
    - name: admin
      port: 15692
      targetPort: 15692
  selector:
    app: clymene-agent
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: clymene-agent-config
  namespace: clymene
data:
  clymene.yml: |
    global:
      scrape_interval:     15s
    scrape_configs:
      - job_name: 'kubernetes-apiservers'
        kubernetes_sd_configs:
          - role: endpoints
        scheme: https
        tls_config:
          ca_file: /var/run/secrets/kubernetes.io/serviceaccount/ca.crt
        bearer_token_file: /var/run/secrets/kubernetes.io/serviceaccount/token
        relabel_configs:
          - source_labels: [__meta_kubernetes_namespace, __meta_kubernetes_service_name, __meta_kubernetes_endpoint_port_name]
            action: keep
            regex: default;kubernetes;https
          - action: replace
            target_label: cluster
            replacement: clymene-cluster

      - job_name: 'kubernetes-nodes'
        scheme: https
        tls_config:
          ca_file: /var/run/secrets/kubernetes.io/serviceaccount/ca.crt
        bearer_token_file: /var/run/secrets/kubernetes.io/serviceaccount/token
        kubernetes_sd_configs:
          - role: node
        relabel_configs:
          - action: labelmap
            regex: __meta_kubernetes_node_label_(.+)
          - target_label: __address__
            replacement: kubernetes.default.svc:443
          - source_labels: [__meta_kubernetes_node_name]
            regex: (.+)
            target_label: __metrics_path__
            replacement: /api/v1/nodes/${1}/proxy/metrics
          - action: replace
            target_label: cluster
            replacement: clymene-cluster
            
      - job_name: 'kubernetes-pods'
        kubernetes_sd_configs:
          - role: pod
        relabel_configs:
          - source_labels: [__meta_kubernetes_pod_annotation_prometheus_io_scrape]
            action: keep
            regex: true
          - source_labels: [__meta_kubernetes_pod_annotation_prometheus_io_path]
            action: replace
            target_label: __metrics_path__
            regex: (.+)
          - source_labels: [__address__, __meta_kubernetes_pod_annotation_prometheus_io_port]
            action: replace
            regex: ([^:]+)(?::\d+)?;(\d+)
            replacement: $1:$2
            target_label: __address__
          - action: labelmap
            regex: __meta_kubernetes_pod_label_(.+)
          - source_labels: [__meta_kubernetes_namespace]
            action: replace
            target_label: kubernetes_namespace
          - source_labels: [__meta_kubernetes_pod_name]
            action: replace
            target_label: kubernetes_pod_name
          - action: replace
            target_label: cluster
            replacement: clymene-cluster
      - job_name: 'kube-state-metrics'
        static_configs:
          - targets: ['kube-state-metrics.kube-system.svc.cluster.local:8080']
            labels:
              cluster: clymene-cluster
              
      - job_name: 'kubernetes-cadvisor'
        scheme: https
        tls_config:
          ca_file: /var/run/secrets/kubernetes.io/serviceaccount/ca.crt
        bearer_token_file: /var/run/secrets/kubernetes.io/serviceaccount/token
        kubernetes_sd_configs:
          - role: node
        relabel_configs:
          - action: labelmap
            regex: __meta_kubernetes_node_label_(.+)
          - target_label: __address__
            replacement: kubernetes.default.svc:443
          - source_labels: [__meta_kubernetes_node_name]
            regex: (.+)
            target_label: __metrics_path__
            replacement: /api/v1/nodes/${1}/proxy/metrics/cadvisor
          - action: replace
            target_label: cluster
            replacement: clymene-cluster
            
      - job_name: 'kubernetes-service-endpoints'
        kubernetes_sd_configs:
          - role: endpoints
        relabel_configs:
          - source_labels: [__meta_kubernetes_service_annotation_prometheus_io_scrape]
            action: keep
            regex: true
          - source_labels: [__meta_kubernetes_service_annotation_prometheus_io_scheme]
            action: replace
            target_label: __scheme__
            regex: (https?)
          - source_labels: [__meta_kubernetes_service_annotation_prometheus_io_path]
            action: replace
            target_label: __metrics_path__
            regex: (.+)
          - source_labels: [__address__, __meta_kubernetes_service_annotation_prometheus_io_port]
            action: replace
            target_label: __address__
            regex: ([^:]+)(?::\d+)?;(\d+)
            replacement: $1:$2
          - action: labelmap
            regex: __meta_kubernetes_service_label_(.+)
          - source_labels: [__meta_kubernetes_namespace]
            action: replace
            target_label: kubernetes_namespace
          - source_labels: [__meta_kubernetes_service_name]
            action: replace
            target_label: kubernetes_name
          - action: replace
            target_label: cluster
            replacement: clymene-cluster
```
