### create project on GCE (kubernetes-kafka-assignment name as example)

### create cluster

```
gcloud container clusters create kafka-cluster \
  --zone us-central1-b \
  --cluster-version 1.6.2 \
  --num-nodes 3 --machine-type n1-standard-2 \
  --node-labels=cluster=kafka-cluster 
```


### get credentials for local kubectl

```
gcloud container clusters get-credentials kafka-cluster \
    --zone us-central1-b --project kubernetes-kafka-assignment
```

### create services

```
 kubectl create -f zookeeper.yaml
 kubectl create -f kafka.yaml
 kubectl create -f test.yaml

```

### wait 2-3 min, check status with 

```
kubectl get pods
kubectl get services
```

### test kafka cluster

```
# create topic
kubectl exec test -- ./bin/kafka-topics.sh --zookeeper zookeeper:2181 --topic test --create --partitions 1 --replication-factor 2

# open searate terminal window and run consumer
 kubectl exec -ti test -- ./bin/kafka-console-consumer.sh --zookeeper zookeeper:2181 --topic test --from-beginning
 
# in separate terminal run producer
kubectl exec -ti test -- ./bin/kafka-console-producer.sh --broker-list kafka:9092 --topic test

# type something in producer command line, check out consumer terminal


## Notes

For Zookeeper gcr.io/google_samples/k8szk:v1 image was used
For Kafka solsson/kafka-persistent:0.10.1

Note: in real life project those images should be moved as sources to project repo to mainaine them and fix, as long it's not images from kafka or zookeper developer but 3rdparty ones.