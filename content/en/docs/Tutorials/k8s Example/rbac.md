---
title: "1.RBAC"
date: 2017-01-05
weight: 1
description: >
  rbac configuration for clymene agent
---
### rbac configuration for clymene agent
```yaml
# rbac.yaml
apiVersion: v1
kind: Namespace
metadata:
  name: clymene
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: clymene-agent
  namespace: clymene
---
apiVersion: rbac.authorization.k8s.io/v1  # kubernetes 1.22+
kind: ClusterRole
metadata:
  name: clymene-agent
  namespace: clymene
rules:
  - apiGroups: [""]
    resources:
      - nodes
      - nodes/metrics
      - nodes/proxy
      - services
      - endpoints
      - pods
    verbs:
      - get
      - list
      - watch
  - apiGroups:
      - extensions
      - networking.k8s.io
    resources:
      - ingresses
      - configmaps
    verbs:
      - get
      - list
      - watch
  - nonResourceURLs:
      - /metrics
      - /metrics/cadvisor
    verbs:
      - get
---
apiVersion: rbac.authorization.k8s.io/v1 # kubernetes 1.22+
kind: ClusterRoleBinding
metadata:
  name: clymene
subjects:
  - kind: ServiceAccount
    name: clymene-agent
    namespace: clymene
roleRef:
  kind: ClusterRole
  name: clymene-agent
  apiGroup: rbac.authorization.k8s.io
```
