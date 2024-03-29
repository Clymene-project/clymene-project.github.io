---
categories: ["Examples"]
tags: ["test", "sample", "docs"]
title: "Kafka Options"
linkTitle: "Kafka Options"
date: 2017-01-18
description: >
  https://kafka.apache.org/
---
### Kafka Options

```
--kafka.producer.authentication string          Authentication type used to authenticate with kafka cluster. e.g. none, kerberos, tls, plaintext (default "none")
--kafka.producer.batch-linger duration          (experimental) Time interval to wait before sending records to Kafka. Higher value reduce request to Kafka but increase latency and the possibility of data loss in case of process restart. See https://kafka.apache.org/documentation/ (default 0s)
--kafka.producer.batch-max-messages int         (experimental) Maximum number of message to batch before sending records to Kafka
--kafka.producer.batch-min-messages int         (experimental) The best-effort minimum number of messages needed to send a batch of records to Kafka. Higher value reduce request to Kafka but increase latency and the possibility of data loss in case of process restart. See https://kafka.apache.org/documentation/
--kafka.producer.batch-size int                 (experimental) Number of bytes to batch before sending records to Kafka. Higher value reduce request to Kafka but increase latency and the possibility of data loss in case of process restart. See https://kafka.apache.org/documentation/
--kafka.producer.brokers string                 The comma-separated list of kafka brokers. i.e. '127.0.0.1:9092,0.0.0:1234' (default "127.0.0.1:9092")
--kafka.producer.compression string             (experimental) Type of compression (none, gzip, snappy, lz4, zstd) to use on messages (default "none")
--kafka.producer.compression-level int          (experimental) compression level to use on messages. gzip = 1-9 (default = 6), snappy = none, lz4 = 1-17 (default = 9), zstd = -131072 - 22 (default = 3)
--kafka.producer.encoding string                Encoding of metric ("json" or "protobuf") sent to kafka. (default "protobuf")
--kafka.producer.kerberos.config-file string    Path to Kerberos configuration. i.e /etc/krb5.conf (default "/etc/krb5.conf")
--kafka.producer.kerberos.keytab-file string    Path to keytab file. i.e /etc/security/kafka.keytab (default "/etc/security/kafka.keytab")
--kafka.producer.kerberos.password string       The Kerberos password used for authenticate with KDC
--kafka.producer.kerberos.realm string          Kerberos realm
--kafka.producer.kerberos.service-name string   Kerberos service name (default "kafka")
--kafka.producer.kerberos.use-keytab            Use of keytab instead of password, if this is true, keytab file will be used instead of password
--kafka.producer.kerberos.username string       The Kerberos username used for authenticate with KDC
--kafka.producer.plaintext.mechanism string     The plaintext Mechanism for SASL/PLAIN authentication, e.g. 'SCRAM-SHA-256' or 'SCRAM-SHA-512' or 'PLAIN' (default "PLAIN")
--kafka.producer.plaintext.password string      The plaintext Password for SASL/PLAIN authentication
--kafka.producer.plaintext.username string      The plaintext Username for SASL/PLAIN authentication
--kafka.producer.protocol-version string        Kafka protocol version - must be supported by kafka server
--kafka.producer.required-acks string           (experimental) Required kafka broker acknowledgement. i.e. noack, local, all (default "local")
--kafka.producer.tls.ca string                  Path to a TLS CA (Certification Authority) file used to verify the remote server(s) (by default will use the system truststore)
--kafka.producer.tls.cert string                Path to a TLS Certificate file, used to identify this process to the remote server(s)
--kafka.producer.tls.enabled                    Enable TLS when talking to the remote server(s)
--kafka.producer.tls.key string                 Path to a TLS Private Key file, used to identify this process to the remote server(s)
--kafka.producer.tls.server-name string         Override the TLS server name we expect in the certificate of the remote server(s)
--kafka.producer.tls.skip-host-verify           (insecure) Skip server's certificate chain and host name verification
--kafka.producer.topic string                   The name of the kafka topic (default "clymene")
--kafka.producer.promtail.topic string          The name of the promtail kafka topic to consume from (default "clymene-logs")
--kafka.producer.flatten-for-druid              flattening settings for using druid.
```

