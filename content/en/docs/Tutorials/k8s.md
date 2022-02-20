---
title: "k8s(Kubernetes) Example"
date: 2017-01-05
weight: 5
description: >
  How to use Clymene at k8s(kubernetes)
---
You can configure various architectures using the Clymene component.

### How to use clymene-agent k8s yaml
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
      containers:
        - name: clymene-agent
          image: bourbonkk/clymene-agent:latest
          imagePullPolicy: Always
          ports:
            - containerPort: 15691
            - containerPort: 15692
          args:
            - --config.file=/etc/clymene/clymene.yml
            - --kafka.producer.brokers=[KAFKA-BROKER]:9092
            - --log-level=info
            # - --gateway.grpc.host-port=clymene-gateway:15610
          env:
            - name: TS_STORAGE_TYPE
              value: kafka
#              value: gateway
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
      - job_name: 'kubernetes-kubelet'
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
```

### How to use clymene-promtail k8s yaml
```yaml
--- # Daemonset.yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: clymene-promtail
  namespace: clymene
  labels:
    app: clymene-promtail
spec:
  selector:
    matchLabels:
      app: clymene-promtail
  template:
    metadata:
      labels:
        app: clymene-promtail
    spec:
      containers:
        - name: clymene-promtail
          image: bourbonkk/clymene-promtail:latest
          imagePullPolicy: Always
          ports:
            - containerPort: 15698
            - containerPort: 9080
          args:
            - --config.file=/etc/promtail/config.yml
            - --es.server-urls=http://elasticsearch:9200
            - --log-level=info
          env:
            - name: STORAGE_TYPE
              value: elasticsearch
          volumeMounts:
            - mountPath: /etc/promtail/
              name: config-volume
      volumes:
        - name: config-volume
          configMap:
            name: promtail-config
      securityContext:
        runAsUser: 1000
--- # Service.yaml
apiVersion: v1
kind: Service
metadata:
  name: clymene-agent
  namespace: clymene
  labels:
    app: clymene-agent
spec:
  ports:
    - name: admin
      port: 15698
      targetPort: 15698
    - name: server
      port: 9080
      targetPort: 9080
  selector:
    app: clymene-agent
--- # configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: promtail-config
  namespace: clymene
data:
  config.yml: |
    server:
      http_listen_port: 9080
      grpc_listen_port: 0

    positions:
      filename: /tmp/positions.yaml

    scrape_configs:
      - job_name: system
        static_configs:
          - targets:
              - localhost
            labels:
              job: varlogs
              __path__: /var/log/*log

      - job_name: kafka-sasl-plain
        kafka:
          use_incoming_timestamp: false
          brokers:
            - localhost:29092
          authentication:
            type: sasl
            sasl_config:
              mechanism: PLAIN
              user: kafkaadmin
              password: kafkaadmin-pass
              use_tls: true
              ca_file: ../../../tools/kafka/secrets/promtail-kafka-ca.pem
              insecure_skip_verify: true
          group_id: kafka_group
          topics:
            - foo
            - ^promtail.*
          labels:
            job: kafka-sasl-plain

--- # Clusterrole.yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: promtail-clusterrole
rules:
  - apiGroups: [""]
    resources:
      - nodes
      - services
      - pods
    verbs:
      - get
      - watch
      - list

--- #ServiceAccount.yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: promtail-serviceaccount

--- #Rolebinding.yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: promtail-clusterrolebinding
subjects:
  - kind: ServiceAccount
    name: promtail-serviceaccount
    namespace: clymene
roleRef:
  kind: ClusterRole
  name: promtail-clusterrole
  apiGroup: rbac.authorization.k8s.io

```

### How to use clymene-ingester k8s yaml
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
        - name: clymene-ingester
          image: bourbonkk/clymene-ingester:latest
          imagePullPolicy: Always
          ports:
            - containerPort: 15694
          args:
            - --prometheus.remote.url=http://prometheus:9090/api/v1/write
            - --log-level=info
            - --kafka.consumer.brokers=clymene-kafka-broker:9092
          env:
            - name: TS_STORAGE_TYPE
              value: prometheus
      securityContext:
        runAsUser: 1000

```


### How to use clymene-gateway k8s yaml
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
        - name: clymene-gateway
          image: bourbonkk/clymene-gateway:latest
          imagePullPolicy: Always
          ports:
            - containerPort: 15610
          args:
            - --es.server-urls=http://[ELASTICSEARCH-IP]:9200
            - --log-level=info
          env:
            - name: TS_STORAGE_TYPE
              value: elasticsearch
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
  ports:
    - name: grpc
      port: 15610
      targetPort: 15610
#      type: NodePort
  selector:
    app: clymene-gateway
```
