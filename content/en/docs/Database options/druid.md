---
categories: ["Examples"]
tags: ["test", "sample", "docs"]
title: "Druid usage"
linkTitle: "Druid usage"
date: 2023-01-23
description: >
    https://druid.apache.org/docs/latest/tutorials/tutorial-kafka.html
---

### Environment value setting

```
STORAGE_TYPE=kafka
```

### arguments setting example

```
--kafka.producer.brokers=BROKER_IP:9092
--kafka.producer.encoding=json
--kafka.producer.flatten-for-druid
```


### 1. druid Streaming setting
![image](https://user-images.githubusercontent.com/25188468/214283285-1c01f945-ac8d-4a2d-806a-1f440e065f26.png)  
![image](https://user-images.githubusercontent.com/25188468/214283446-c82c9773-5158-4e26-b2a6-93584a64cf62.png)
![image](https://user-images.githubusercontent.com/25188468/214283549-a59a23a5-9ece-4d11-ab4e-04c8c40be0d3.png)  


### 2. Tune setting - Use earliest offset : true
![image](https://user-images.githubusercontent.com/25188468/214284118-0463dbd2-a082-4c00-b025-e1723f23c25d.png)


### 3. Check Ingest and Datasources menu
![image](https://user-images.githubusercontent.com/25188468/214284410-1a15ae03-0330-4a1a-addb-5029243a36ec.png)  
![image](https://user-images.githubusercontent.com/25188468/214284518-bf7146cc-c99a-4ddd-9914-92a95bf3cf64.png)

### 4. query execution
![image](https://user-images.githubusercontent.com/25188468/214284893-2a444d50-144e-4716-9972-2c87d759bdd0.png)