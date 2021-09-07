## Fraud detection pipeline, based on Kafka.
This project was created for study purposes. There are several Kafka brokers (+ Zookeeper)
one generator and one detector service. Generator service generates data to input Kafka topic (partition=2, replication=2).
Detector consumes messages from input topic, apples fraud-detection algorithm :-) and depends on result 
sends message either to correct or fraud Kafka topics. 


#### Docker compose running
```shell
   docker-compose up 
```

#### Kubernetes running
```shell
    # skip steps for creating cluster and configuring kubectl command
    kubectl apply -f kubernetes.yaml
```

#### Message schema
```json
{
  "user_from": "David", 
  "user_to": "Denys",
  "amount": 846,
  "currency": "USD",
  "date": "2021-01-01"
} 
```

#### Fraud detector algorithm
For simplicity, all messages with amount > 900 are marker as fraud.

#### Experiments
- Remove one broker. Kafka should work well and re-balancing data and load.
- Add one more broker (in total 3). New broker should start works.
- Add one consumer (detector). Old (first consumer before adding consumes data from 2 partition. After adding,
  each consumer should consume on one partition)
- Add one more consumer (in total 3). One should be IDLE because input partition=2