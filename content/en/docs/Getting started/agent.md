---
categories: ["Examples"]
tags: ["test", "sample", "docs"]
title: "Clymene-agent Getting Start"
linkTitle: "Clymene Agent"
date: 2017-01-18
description: >
  The Clymene-agent is service that collects time series data
---
The Clymene-agent is service that collects time series data(does not use disks)  

### How to create a scrape target setting yaml
1. Config file Option
```bash
clymene-agent --config.file=/etc/clymene/clymene.yml
```
2. How to write yaml - [See Prometheus Configuration for more information](https://prometheus.io/docs/prometheus/latest/configuration/configuration/)
```yaml
global:
   scrape_interval: 15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.  
   evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.

scrape_configs:
- job_name: 'localhost'  
  static_configs:  
    - targets: [ 'localhost:9100' ]   

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
gateway
```
TS_STORAGE_TYPE=gateway
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


### Use only agent Architecture
<img src="https://user-images.githubusercontent.com/25188468/150611946-c3f07298-03ee-42ba-8937-9da22735da45.png" width="70%" height="70%" alt="architecture_v1.4.0">