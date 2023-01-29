# Deploy Kafka based on Confluent Helm Chart

## Pre-requisites

- Have a K8s/minikube cluster installed. [See Pre-requsites section](../README.md/#prerequisites)

## Installation

1. Create kafka namespace

    ```console
    kubectl create namespace kafka
    ```

2. Clone the conflueninc repo:

    ```console
    git clone https://github.com/confluentinc/cp-helm-charts.git
    ```

3. Go into the directory

    ```console
    cd ./cp-helm-charts
    ```

    > **Note** If the deployed cluster is K8s the `values.yaml` file located in the root of the `cp-helm-charts` folder must be changed to [this one](./values.yaml).

4. Deploy Kafka cluster

    ```console
    helm install confluentinc . -n kafka
    ```
