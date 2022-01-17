---
categories: ["Examples"]
tags: ["test", "sample", "docs"]
title: "Cortex Options"
linkTitle: "Cortex Options"
date: 2017-01-18
description: >
  https://cortexmetrics.io/
---
### Cortex Options

```
--cortex.distributor.kafka.encoding string   Encoding of metric ("json" or "protobuf") sent to kafka. (default "protobuf")
--cortex.distributor.max.err.msg.len int     Maximum length of error message (default 256)
--cortex.distributor.timeout duration        Time out when doing remote write(sec, default 10 sec) (default 10s)
--cortex.distributor.url string              the cortex distributor remote write receiver endpoint(/api/v1/push) (default "http://localhost/api/v1/push")
--cortex.distributor.user.agent string       User-Agent in request header (default "Prometheus/")
```

