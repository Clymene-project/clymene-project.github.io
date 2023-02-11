---
categories: ["Examples"]
tags: ["test", "sample", "docs"]
title: "Prometheus Options"
linkTitle: "Prometheus Options"
date: 2017-01-18
description: >
  https://prometheus.io/
---
### Prometheus Options

```
--prometheus.remote.kafka.encoding string   Encoding of metric ("json" or "protobuf") sent to kafka. (default "protobuf")
--prometheus.remote.max.err.msg.len int     Maximum length of error message (default 256)
--prometheus.remote.timeout duration        Time out when doing remote write(sec, default 10 sec) (default 10s)
--prometheus.remote.url string              the prometheus remote write receiver endpoint(/api/v1/write) (default "http://localhost:9090/api/v1/write")
--prometheus.remote.user.agent string       User-Agent in request header (default "Prometheus/")
```

