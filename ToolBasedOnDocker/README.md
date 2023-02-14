# Performance test based on a Docker Tool

## Architecture

- Performance tests may be performed for the following architectures.

| ![NodePort](https://user-images.githubusercontent.com/6643905/215356134-656ad456-dec0-4682-8ca1-5f3fcf8f196d.PNG) | ![LoadBalancer](https://user-images.githubusercontent.com/6643905/218860678-08ff540e-f3d6-44ba-8ae5-a3ca28570b7b.png) |
|-------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| **Service type:** NodePort                                                                                        | **Service type:** LoadBalancer                                                                                    |

## Basic commands

- In each case, the IP addresses and ports of the `bootstrap-server` must be substituted according to each architecture.

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

## Recommended performance tests

1. Throughput variation for a single publisher. E,g. Variation of Throughput from 1000 to 40000:

   ```console
   docker run -it --rm confluentinc/cp-kafka:latest kafka-producer-perf-test --topic ssl-perf-test --throughput 1000 --num-records 3000 --record-size 1024 --producer-props acks=all bootstrap.servers=10.98.6.161:31090

   docker run -it --rm confluentinc/cp-kafka:latest kafka-producer-perf-test --topic ssl-perf-test --throughput 5000 --num-records 3000 --record-size 1024 --producer-props acks=all bootstrap.servers=10.98.6.161:31090

   docker run -it --rm confluentinc/cp-kafka:latest kafka-producer-perf-test --topic ssl-perf-test --throughput 40000 --num-records 3000 --record-size 1024 --producer-props acks=all bootstrap.servers=10.98.6.161:31090
   ```

2. Variation in the number of messages (--messages) sent in correspondence with the number of records consumed (--num-records). E.g.:

   ```console
   docker run -it --rm confluentinc/cp-kafka:latest kafka-consumer-perf-test --topic ssl-perf-test --broker-list 10.98.6.161:31090 --messages 1000

   docker run -it --rm confluentinc/cp-kafka:latest kafka-producer-perf-test --topic ssl-perf-test --throughput 40000 --num-records 1000 --record-size 1024 --producer-props acks=all bootstrap.servers=10.98.6.161:31090
   ```
