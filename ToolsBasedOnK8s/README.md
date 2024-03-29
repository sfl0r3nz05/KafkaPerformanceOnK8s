# Performance test based on K8s Jobs

> This repo is based on the contribution proposed by [im-pratham](https://github.com/im-pratham) in this [medium article](https://towardsdev.com/performance-testing-your-kubernetes-kafka-cluster-95f6e7d8dfc5).

## Performance Tests

It is possible to create multiple scenarios considering the variable load, message size, and the concurrent instances of the clients.There are five sets of scenarios relevant to the test:

- Single producer throughput.
- Multiple producer throughput.
- Single producer and consumer.
- Multiple producers and single consumer throughput
- End to End Latency.

1. Preparing env

    ```console
    KHOST=$(kubectl get svc -n kafka -o jsonpath="{.items[1].metadata.name}")
    KPORT=$(kubectl get svc -n kafka -o jsonpath="{.items[1].spec.ports[0].port}")
    export KAFKA=$KHOST:$KPORT
    ```

2. Create kafka-client

    ```console
    kubectl apply -f kafka-client.yaml -n kafka
    ```

3. Create perf testing topic

    ```console
    kubectl exec -it kafka-client -n kafka -- kafka-topics --bootstrap-server $KAFKA --create --topic ssl-perf-test --partitions 6 --replication-factor 3 --config retention.ms=1800000 --config min.insync.replicas=2 --command-config /etc/kafka/config/producer.properties
    ```

### Test 1: Single producer throughput using TLS.

1. Start producer

    ```console
    kubectl apply -f single-producer.yaml -n kafka
    ```

2. Collect information

    ```console
    producer=$(kubectl get pods -n kafka --selector=job-name=kafka-single-producer-client --output=jsonpath='{.items[0].metadata.name}')
    ```

3. Visualize logs of producer

    ```console
    kubectl logs -f $producer -n kafka
    ```

4. Results of: [Test Single Producer](./ExperimentResults/single-producer.txt).

### Test 2: Multiple producer throughput using TLS.

1. Start producer

    ```console
    kubectl apply -f multiple-producer.yaml -n kafka
    ```

2. Collect information form 3 producers deployed

    ```console
    producer1=$(kubectl get pods -n kafka --selector=job-name=kafka-multiple-producer-client --output=jsonpath='{.items[0].metadata.name}')
    ```

    ```console
    producer2=$(kubectl get pods -n kafka --selector=job-name=kafka-multiple-producer-client --output=jsonpath='{.items[1].metadata.n$    
    ```

    ```console
    producer3=$(kubectl get pods -n kafka --selector=job-name=kafka-multiple-producer-client --output=jsonpath='{.items[2].metadata.n$    
    ```

3. Visualize logs producers

    ```console
    kubectl logs -f $producer1 -n kafka
    ```

    ```console
    kubectl logs -f $producer2 -n kafka
    ```

    ```console
    kubectl logs -f $producer3 -n kafka
    ```

4. Results of: [Multiple Producer 1](./ExperimentResults/multiple-producer-1.txt), [Multiple Producer 2](./ExperimentResults/multiple-producer-2.txt) and [Test Multiple Producer 3](./ExperimentResults/multiple-producer-3.txt).

### Test 3: Single producer and consumer throughput.

1. Start consumer

    ```console
    kubectl apply -f consumer.yaml -n kafka
    ```

2. Start producer

    ```console
    kubectl apply -f producer-fixed-single.yaml -n kafka
    ```

3. Collect information of consumer

    ```console
    consumer=$(kubectl get pods -n kafka --selector=job-name=kafka-consumer-client --output=jsonpath='{.items[*].metadata.name}')
    ```

4. Visualize information of consumer

    ```console
    kubectl logs -f $consumer -n kafka
    ```

5. Collect information of producer

    ```console
    producer=$(kubectl get pods -n kafka --selector=job-name=kafka-producer-fixed-single --output=jsonpath='{.items[*].metadata.name}')
    ```

    ```console
    kubectl logs -f $producer -n kafka
    ```

6. Visualize information of consumer

    ```console
    kubectl logs -f $producer -n kafka
    ```

7. Results of: [Single Producer](./ExperimentResults/single-producer-to-consumer.txt), [Single Consumer](./ExperimentResults/single-consumer-to-producer.txt).

### Test 4: Multiple producer and single consumer throughput.

1. Start consumer

    ```console
    kubectl apply -f consumer-fixed-single.yaml -n kafka
    ```

2. Start producer

    ```console
    kubectl apply -f producer-fixed-multiple.yaml -n kafka
    ```

3. Collect information of consumer

    ```console
    consumer=$(kubectl get pods -n kafka --selector=job-name=kafka-consumer-fixed-single --output=jsonpath='{.items[*].metadata.name}')
    ```

4. Visualize logs of consumer

    ```console
    kubectl logs -f $consumer -n kafka
    ```

5. Collect information of the 3 producers

    ```console
    producers1=$(kubectl get pods -n kafka --selector=job-name=kafka-producer-fixed-multiple --output=jsonpath='{.items[0].metadata.name}')
    ```

    ```console
    producers2=$(kubectl get pods -n kafka --selector=job-name=kafka-producer-fixed-multiple --output=jsonpath='{.items[1].metadata.name}')
    ```

    ```console
    producers3=$(kubectl get pods -n kafka --selector=job-name=kafka-producer-fixed-multiple --output=jsonpath='{.items[2].metadata.name}')
    ```

6. Visualize logs of producers

    ```console
    kubectl logs -f $producers1 -n kafka
    ```

    ```console
    kubectl logs -f $producers2 -n kafka
    ```

    ```console
    kubectl logs -f $producers3 -n kafka
    ```

7. Results of: [Single Consumer](./ExperimentResults/single-consumer-from-multiple-producer.txt), [Multiple Producer 1](./ExperimentResults/multiple-producer-1-to-single-consumer.txt), [Multiple Producer 2](./ExperimentResults/multiple-producer-2-to-single-consumer.txt), [Multiple Producer 3](./ExperimentResults/multiple-producer-3-to-single-consumer.txt).

### Test 5: End to End Latency.

1. Start consumer

    ```console
    kubectl apply -f e2e.yaml -n kafka
    ```

2. Collect information of latency

    ```console
    e2e=$(kubectl get pods -n kafka --selector=job-name=kafka-e2e-client --output=jsonpath='{.items[*].metadata.name}')
    ```

3. Visualize logs for end to end communication:

    ```console
    kubectl logs -f $e2e -n kafka
    ```

4. Results of: [End to End Latency](./ExperimentResults/e2e.txt).

## Troubleshootings

When a test is re-launched, it must be verified that the configmap has not been previously created:

```console
kubectl get configmap kafka-single-producer-perf-script -n kafka
```

If it has been created, it must be deleted:

```console
kubectl delete configmap kafka-single-producer-perf-script -n kafka
```
