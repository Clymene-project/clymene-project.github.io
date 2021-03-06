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
--admin.http.host-ports string              The host:ports (e.g. 127.0.0.1:15690 or :15690) for the admin server, including health check, /metrics, etc. (default ":15690")
--gateway.grpc-server.host-port string      The host:port (e.g. 127.0.0.1:15610 or :15610) of the gateway's GRPC server (default ":15610")
--gateway.grpc.tls.cert string              Path to a TLS Certificate file, used to identify this server to clients
--gateway.grpc.tls.client-ca string         Path to a TLS CA (Certification Authority) file used to verify certificates presented by clients (if unset, all clients are permitted)
--gateway.grpc.tls.enabled                  Enable TLS on the server
--gateway.grpc.tls.key string               Path to a TLS Private Key file, used to identify this server to clients
--log-level string                          Minimal allowed log Level. For more levels see https://github.com/uber-go/zap (default "info")
--prometheus.remote.kafka.encoding string   Encoding of metric ("json" or "protobuf") sent to kafka. (default "protobuf")
--prometheus.remote.max.err.msg.len int     Maximum length of error message (default 256)
--prometheus.remote.timeout duration        Time out when doing remote write(sec, default 10 sec) (default 10s)
--prometheus.remote.url string              the prometheus remote write receiver endpoint(/api/v1/write) (default "http://localhost:9090/api/v1/write")
--prometheus.remote.user.agent string       User-Agent in request header (default "Prometheus/")
```

