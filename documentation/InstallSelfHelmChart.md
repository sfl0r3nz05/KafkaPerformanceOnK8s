# Deploy Kafka based on Self Helm Chart

## Pre-requisites

- Have a K8s/minikube cluster installed. [See Pre-requsites section](../README.md/#prerequisites)

## Installation

1. Modify the values of the `value.yaml` file, placed in `~/KafkaPerformanceOnK8s/chart/kafka` in `Kafka` according to your requirements. E.g.:

   - Number of replicas:

   ```console
   replicas: 1
   ```

2. Modify the values of the `value.yaml` file, placed in `~/KafkaPerformanceOnK8s/chart/zookeeper` in `Zookeeper` according to your requirements. E.g.:

   - Number of replicas:

   ```console
   replicas: 1
   ```

> **Note:** For environments based on Minikube.

   - Enable external access:

   ```console
   external:
     enabled: true
     type: NodePort
   dns:
     useInternal: false
     useExternal: false
   ```

   - Configure the external access, modifying the external IP address. E.g.: 3.94.152.36:

   ```console
   configurationOverrides:
     "advertised.listeners": EXTERNAL://3.94.152.36:31090
   ```

   ```console
     "listener.security.protocol.map": |-
   ```

> **Note:** For environments based on K8s.

   - Enable external access:

   ```console
   external:
     enabled: true
     type: LoadBalancer
   dns:
     useInternal: false
     useExternal: false
   ```

   - Configure the external access, modifying the external IP address. E.g.: 10.5.0.102:

   ```console
   configurationOverrides:
     "advertised.listeners": EXTERNAL://10.5.0.102:31090
   ```

   ```console
     "listener.security.protocol.map": |-
   ```

3. Create namespace

   ```console
   kubectl create namespace kafka
   ```

5. Update the kafka helm repository

   > **Note:** Zookeeper is included in the Kafka helmchart in step number 3. The details of the requirement can be found in the `requirements.yaml` file placed in `~/PerformanceNetworkToolOnK8s/charts/kafka/requirements.yaml`.

   ```console
   cd ~/PerformanceNetworkToolOnK8s/charts/kafka
   helm dependency update
   ```

6. Install Kafka and Zookeeper cluster from helm chart

   ```console
   helm install kafka . -n kafka
   ```
