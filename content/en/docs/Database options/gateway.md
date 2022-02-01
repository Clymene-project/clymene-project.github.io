---
categories: ["Examples"]
tags: ["test", "sample", "docs"]
title: "clymene-gateway Options"
linkTitle: "clymene-gateway Options"
date: 2017-01-18
description: >
  The Clymene Gateway is an optional service that can receive metric data from the agent through gRPC, HTTP communication
---
### clymene-gateway Options

```
--gateway.grpc.discovery.min-peers int   Max number of collectors to which the agent will try to connect at any given time (default 3)
--gateway.grpc.host-port string          Comma-separated string representing host:port of a static list of gateways to connect to directly (default "localhost:15610")
--gateway.grpc.retry.max uint            Sets the maximum number of retries for a call (default 3)
--gateway.grpc.tls.ca string             Path to a TLS CA (Certification Authority) file used to verify the remote server(s) (by default will use the system truststore)
--gateway.grpc.tls.cert string           Path to a TLS Certificate file, used to identify this process to the remote server(s)
--gateway.grpc.tls.enabled               Enable TLS when talking to the remote server(s)
--gateway.grpc.tls.key string            Path to a TLS Private Key file, used to identify this process to the remote server(s)
--gateway.grpc.tls.server-name string    Override the TLS server name we expect in the certificate of the remote server(s)
--gateway.grpc.tls.skip-host-verify      (insecure) Skip server's certificate chain and host name verification
--gateway.http.max-err-msg-len int       Maximum length of error message (default 256)
--gateway.http.timeout duration          Time out when doing remote write(sec, default 10 sec) (default 10s)
--gateway.http.url string                the clymene-gateway remote write HTTP receiver endpoint(/api/metrics) (default "http://localhost:15611/api/metrics")
--gateway.http.user.agent string         User-Agent in request header (default "Clymene/")
--gateway.service-type string            gateway service type(grpc or http) (default "grpc")
```

