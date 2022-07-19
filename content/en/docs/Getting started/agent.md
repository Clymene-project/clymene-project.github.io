---
categories: ["Examples"]
tags: ["test", "sample", "docs"]
title: "Clymene-agent Getting Start"
linkTitle: "Clymene Agent"
date: 2017-01-18
weight: 1
description: >
  The Clymene-agent is service that collects time series data
---
The Clymene-agent is service that collects time series data(does not use disks)  

### How to create a scrape target setting yaml
1. Config file Option
```bash
clymene-agent --config.file=/etc/clymene/clymene.yml
```
2. How to write yaml - [See Clymene Configuration for more information](https://clymene-project.github.io/docs/service-discovery/)
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

3. config file reload
```yaml
curl -XPOST http://clymene-agent:15692/api/reload

# check clymene-agent logs
  {"level":"info","ts":1643299385.1000407,"caller":"config/config.go:451","msg":"Loading configuration file","filename":"clymene_agent.yml"}
  {"level":"info","ts":1643299385.1012235,"caller":"config/config.go:468","msg":"Completed loading of configuration file","filename":"clymene_agent.yml"}
```

### How to setting clymene-agent

```
--admin.http.host-ports string           The host:ports (e.g. 127.0.0.1:15691 or :15691) for the admin server, including health check, /metrics, etc. (default ":15691")
--config.file string                     configuration file path. (default "/etc/clymene/clymene.yml")
--enable.new-service-discovery-manager   use new service discovery manager (default true)
--http.port int                          http port (default 15692)
--log-level string                       Minimal allowed log Level. For more levels see https://github.com/uber-go/zap (default "info")
--metrics-backend string                 Defines which metrics backend to use for metrics reporting: expvar, prometheus, none (default "prometheus")
--metrics-http-route string              Defines the route of HTTP endpoint for metrics backends that support scraping (default "/metrics")
```
### How to set up the Storage Type
##### 1. Setting environmental variables

ElasticSearch
```
STORAGE_TYPE=elasticsearch
```
Kafka
```
STORAGE_TYPE=kafka
```
prometheus
```
STORAGE_TYPE=prometheus
```
cortex
```
STORAGE_TYPE=cortex
```
gateway
```
STORAGE_TYPE=gateway
```
opentsdb
```
STORAGE_TYPE=opentsdb
```
influxdb
```
STORAGE_TYPE=influxdb
```
tdengine
```
STORAGE_TYPE=tdengine
```
Several
```
STORAGE_TYPE=elasticsearch,prometheus  # composite write - Write in multiple databases at the same time.
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
<img src="https://user-images.githubusercontent.com/25188468/152248922-8b86e107-ed16-4ec1-a68a-48e1016a7521.png" width="70%" height="70%" alt="architecture_v1.4.0">
