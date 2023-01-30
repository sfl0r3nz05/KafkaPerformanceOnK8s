# Performance Evaluation of Kafka deployed on K8s

> **Note:** This project is under development 🛠 and mantained by [Santiago Figueroa](sfigueroa@ceit.es).

## Prerequisites

1. Deploy the cluster:
   1. [Test cluster installation (*for demo purposes*)](./documentation/minikube.md)
   2. [Deployment of K8s Cluster on LXC based on Ansible (*for production purposes*)](https://github.com/sfl0r3nz05/BaseImageClusterK8sOnLXC.git)

2. Install requirements:
   1. [Helm chart installation](./documentation/install_helm.md)
   2. [Docker installation](https://docs.docker.com/engine/install/ubuntu/)
   3. [Manage Docker as a non-root user](https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user)

## Install Kafka on deployed clusters

1. [Installation based on a self Helm Chart](./documentation/InstallSelfHelmChart.md)
2. [Installation based on Confluent Helm Chart](./documentation/InstallConfluentHelmChart.md)

## Performance Kafka Test

1. [Performance test based on a Docker Tool](./ToolBasedOnDocker/README.md)
   - [Demonstration video using NodePort as service type](https://youtu.be/vysTHRjRQr4)
   - [Demonstration video using LoadBalancer as service type](https://youtu.be/0MjyktT1ouQ)
2. [Performance test based on K8s Jobs](./ToolsBasedOnK8s/README.md)

## To do

1. Testing a deployment based on cp-helm-charts with the Docker tool.
2. Improve the demonstration (videos published on youtube) of Docker-based performance tests for both NodePort and LoadBalancer service types.
3. Create an image to better understanding of the performance test based on K8s Jobs.
4. Create a demonstration video for the performance test based on K8s Jobs.
