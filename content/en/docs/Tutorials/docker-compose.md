---
title: "Docker-compose Example"
date: 2017-01-05
weight: 1
description: >
  How to use Clymene at Docker Compose.
---

You can configure various architectures using the Clymene component.

### How to use clymene-agent Docker-compose
```yaml
version: '2'
services:
  clymene-agent:
    image: bourbonkk/clymene-agent:latest
    ports:
      - "15691:15691"
      - "15692:15692"
    environment:
      - STORAGE_TYPE=kafka # database you want to use
      # - STORAGE_TYPE=gateway
    volumes:
      - ./config/clymene_scrape_config.yml:/etc/clymene/clymene.yml
    command:
      - --log-level=debug
      - --kafka.producer.brokers=[KAFKA-IP]:9092
      # - --gateway.grpc.host-port=localhost:15610
```

### How to use clymene-promtail Docker-compose
```yaml
version: '2'
services:
  clymene-promtail:
    image: bourbonkk/clymene-promtail:latest
    ports:
      - "15698:15698"
      - "9080:9080"
    environment:
      - STORAGE_TYPE=elasticsearch
    volumes:
      - ./config/promtail-local-config.yaml.yml:/etc/promtail/config.yml
    command:
      - --log-level=debug
      - --es.server-urls=[ELASTICSEARCH-IP]:9200
```

### How to use clymene-ingester Docker-compose
```yaml
version: '2'
services:
  clymene-ingester:
    image: bourbonkk/clymene-ingester:latest
    ports:
      - "15694:15694"
    environment:
      # - STORAGE_TYPE=elasticsearch,prometheus   # use composite writer
      - STORAGE_TYPE=elasticsearch
    command:
      - --log-level=debug
      - --kafka.consumer.brokers=[KAFKA-IP]:9092
      - --es.server-urls=http://[ELASTICSEARCH-IP]:9200
#      - --prometheus.remote.url=http://prometheus:9090/api/v1/write
```


### How to use clymene-gateway Docker-compose
```yaml
version: '2'
services:
  clymene-ingester:
    image: bourbonkk/clymene-gateway:latest
    ports:
      - "15610:15610"
    environment:
      - STORAGE_TYPE=elasticsearch
    command:
      - --log-level=debug
      - --es.server-urls=http://[ELASTICSEARCH-IP]:9200
```
