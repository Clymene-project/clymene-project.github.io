---
categories: ["Examples"]
tags: ["test", "sample", "docs"]
title: "Clymene Ingester Getting Start"
linkTitle: "Clymene Ingester"
date: 2017-01-05
description: >
  The Clymene ingester is an optional service responsible for insert time series data loaded on kafka into the database
---
The Clymene ingester is an optional service responsible for insert time series data loaded on kafka into the database  

1. Kafka message consume
2. Time-series data insert to Database(ElasticSearch, Prometheus, ETC) (Optional)

### How to setting ingester

```
--kafka.consumer.authentication string          Authentication type used to authenticate with kafka cluster. e.g. none, kerberos, tls, plaintext (default "none")
--kafka.consumer.brokers string                 The comma-separated list of kafka brokers. i.e. '127.0.0.1:9092,0.0.0:1234' (default "127.0.0.1:9092")
--kafka.consumer.client-id string               The Consumer Client ID that clymene-ingester will use (default "clymene")
--kafka.consumer.encoding string                The encoding of metrics ("json", "protobuf") consumed from kafka (default "protobuf")
--kafka.consumer.group-id string                The Consumer Group that clymene-ingester will be consuming on behalf of (default "clymene")
--kafka.consumer.kerberos.config-file string    Path to Kerberos configuration. i.e /etc/krb5.conf (default "/etc/krb5.conf")
--kafka.consumer.kerberos.keytab-file string    Path to keytab file. i.e /etc/security/kafka.keytab (default "/etc/security/kafka.keytab")
--kafka.consumer.kerberos.password string       The Kerberos password used for authenticate with KDC
--kafka.consumer.kerberos.realm string          Kerberos realm
--kafka.consumer.kerberos.service-name string   Kerberos service name (default "kafka")
--kafka.consumer.kerberos.use-keytab            Use of keytab instead of password, if this is true, keytab file will be used instead of password
--kafka.consumer.kerberos.username string       The Kerberos username used for authenticate with KDC
--kafka.consumer.plaintext.mechanism string     The plaintext Mechanism for SASL/PLAIN authentication, e.g. 'SCRAM-SHA-256' or 'SCRAM-SHA-512' or 'PLAIN' (default "PLAIN")
--kafka.consumer.plaintext.password string      The plaintext Password for SASL/PLAIN authentication
--kafka.consumer.plaintext.username string      The plaintext Username for SASL/PLAIN authentication
--kafka.consumer.protocol-version string        Kafka protocol version - must be supported by kafka server
--kafka.consumer.tls.ca string                  Path to a TLS CA (Certification Authority) file used to verify the remote server(s) (by default will use the system truststore)
--kafka.consumer.tls.cert string                Path to a TLS Certificate file, used to identify this process to the remote server(s)
--kafka.consumer.tls.enabled                    Enable TLS when talking to the remote server(s)
--kafka.consumer.tls.key string                 Path to a TLS Private Key file, used to identify this process to the remote server(s)
--kafka.consumer.tls.server-name string         Override the TLS server name we expect in the certificate of the remote server(s)
--kafka.consumer.tls.skip-host-verify           (insecure) Skip server's certificate chain and host name verification
--kafka.consumer.topic string                   The name of the kafka topic to consume from (default "clymene")
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

### Including kafka and ingester Architecture
<img src="https://user-images.githubusercontent.com/25188468/150611879-35855d02-0753-4f90-83fd-583c1e036c71.png" width="70%" height="70%" alt="architecture_v1.4.0_ingester">  

