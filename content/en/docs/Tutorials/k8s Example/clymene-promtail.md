---
title: "5.clymene-promtail"
date: 2017-01-05
weight: 5
description: >
  configuration for clymene promtail
---
### configuration for clymene promtail
```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: clymene-promtail
  namespace: clymene
spec:
  selector:
    matchLabels:
      name: promtail
  template:
    metadata:
      labels:
        name: promtail
    spec:
      serviceAccount: clymene-promtail
      serviceAccountName: clymene-promtail
      volumes:
        - name: logs
          hostPath:
            path: HOST_PATH
        - name: promtail-config
          configMap:
            name: promtail-config
      containers:
        - name: promtail-container
          image: bourbonkk/clymene-promtail
          imagePullPolicy: Always
          args:
            - --config.file=/etc/promtail/promtail.yaml
            - --gateway.grpc-client.host-port=[CLYMENE_GATEWAY_NODE_PORT]:30611
          env:
            - name: 'HOSTNAME'
              valueFrom:
                fieldRef:
                  fieldPath: 'spec.nodeName'
            - name: STORAGE_TYPE
              value: gateway
          volumeMounts:
            - name: logs
              mountPath: /var/log/
            - name: promtail-config
              mountPath: /etc/promtail

---
#  configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: promtail-config
  namespace: clymene
data:
  promtail.yaml: |
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
          cluster: target-cluster
---
#  Clusterrole.yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: promtail-clusterrole
  namespace: clymene
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
---
#  ServiceAccount.yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: clymene-promtail
  namespace: clymene

---
#  Rolebinding.yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: promtail-clusterrolebinding
subjects:
  - kind: ServiceAccount
    name: clymene-promtail
    namespace: clymene
roleRef:
  kind: ClusterRole
  name: promtail-clusterrole
  apiGroup: rbac.authorization.k8s.io

```
