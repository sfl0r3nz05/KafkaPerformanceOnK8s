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

   > **Note:** For environments based on Minikube using NodePort.

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
	- The IP to be used can be seen in the figure below left.

	   ```console
	   configurationOverrides:
	     "advertised.listeners": EXTERNAL://3.94.152.36:31090
	     ...
	     "listener.security.protocol.map": |-
	       PLAINTEXT:PLAINTEXT,EXTERNAL:PLAINTEXT
	   ```

   > **Note:** For environments based on K8s using LoadBalancer.

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
	- The IP to be used can be seen in the figure below left.

	   ```console
	   configurationOverrides:
	     "advertised.listeners": EXTERNAL://10.5.0.102:31090
	     ...
	     "listener.security.protocol.map": |-
	       PLAINTEXT:PLAINTEXT,EXTERNAL:PLAINTEXT
	   ```

	| <img src="https://user-images.githubusercontent.com/6643905/216343545-64a965c2-f79c-4c1e-8778-a23f0aaa1733.png"  width=60% height=60%> | <img src="https://user-images.githubusercontent.com/6643905/216343551-ef47bf13-0033-46c6-9062-c1cd7c7cc7d8.png"  width=60% height=60%> |
	|-------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|
	| **Service type:** NodePort                                                                                        | **Service type:** LoadBalancer                                                                                    |

3. Create namespace

   ```console
   kubectl create namespace kafka
   ```

4. Update the `kafka` `helm repository` including the `zookeeper` `helm chart` inside the `kafka` folder:

   > **Note:** Zookeeper is included in the Kafka helmchart in step number 3. The details of the requirement can be found in the `requirements.yaml` file placed in `~/PerformanceNetworkToolOnK8s/charts/kafka/requirements.yaml`.

   ```console
   cd ~/PerformanceNetworkToolOnK8s/charts/kafka
   helm dependency update
   ```

5. Install Kafka and Zookeeper cluster from helm chart

   ```console
   helm install kafka . -n kafka
   ```

6. Exposure of the cluster to internal connections must be done once the cluster has been deployed.

	> **Note:** For environments based on Minikube using `NodePort`.

   - For the following Kafka cluster deployed:

	```console
	NAME                    READY   STATUS    RESTARTS        AGE
	pod/kafka-0             1/1     Running   2 (4h24m ago)   6h32m
	pod/kafka-zookeeper-0   1/1     Running   1 (4h25m ago)   6h32m

	NAME                               TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
	service/kafka                      ClusterIP   10.102.186.115   <none>        9092/TCP                     6h32m
	service/kafka-0-external           NodePort    10.106.175.163   <none>        19092:31090/TCP              6h32m
	service/kafka-headless             ClusterIP   None             <none>        9092/TCP                     6h32m
	service/kafka-zookeeper            ClusterIP   10.104.249.3     <none>        2181/TCP                     6h32m
	service/kafka-zookeeper-headless   ClusterIP   None             <none>        2181/TCP,3888/TCP,2888/TCP   6h32m

	NAME                               READY   AGE
	statefulset.apps/kafka             1/1     6h32m
	statefulset.apps/kafka-zookeeper   1/1     6h32m
	```

	- Enable `service/kafka` as `NodePort` to expose internal cluster:

	```console
	kubectl edit service/kafka -n kafka
	```

	- The service is now exposed:

	```console
	NAME                               TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
	service/kafka                      NodePort    10.102.186.115   <none>        9092:30537/TCP               6h38m
	```

7. It is now possible to perform both internal and external connection tests. For this, refer to section [Performance test based on a Docker Tool](../ToolBasedOnDocker/README.md).
