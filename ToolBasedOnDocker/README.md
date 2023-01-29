# Performance test based on a Docker Tool

| ![NodePort](https://user-images.githubusercontent.com/6643905/215356134-656ad456-dec0-4682-8ca1-5f3fcf8f196d.PNG) | ![LoadBalancer](https://user-images.githubusercontent.com/6643905/215358725-68f9d3a8-2f3e-441f-8e47-928d72e2669c.PNG) |
|-------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| **Service type:** NodePort                                                                                        | **Service type:** LoadBalancer                                                                                    |

1. Create a topic. E.g:

   ```console   
   docker run -it --rm confluentinc/cp-kafka:latest kafka-topics --create --topic ssl-perf-test --partitions 16 --replication-factor 3 --config retention.ms=86400000 --config min.insync.replicas=2 --bootstrap-server 172.31.26.62:32721
   ```

2. Test the Producer Performance. E.g:

   ```console
   docker run -it --rm confluentinc/cp-kafka:latest kafka-producer-perf-test --topic ssl-perf-test --throughput -1 --num-records 3000000 --record-size 1024 --producer-props acks=all bootstrap.servers=172.31.26.62:32721
   ```

3. Test the Consumer Performance. E.g:

   ```console
   docker run -it --rm confluentinc/cp-kafka:latest  kafka-consumer-perf-test --topic ssl-perf-test --broker-list 172.31.26.62:32721 --messages 3000000
   ```
