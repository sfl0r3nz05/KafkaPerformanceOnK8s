# Install and Deploy Minikube

1. Check and delete for previous minikube cluster deployed

    ```console
    minikube delete
    ```

2. Delete Minikube version:

    ```console
    rm -rf /usr/local/bin/minikube
    ```

3. Download actual Minikube version:

    Latest:

    ```console
    curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
    ```

    v1.24.0:

    ```console
    curl -Lo minikube https://storage.googleapis.com/minikube/releases/v1.24.0/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
    ```

4. Deployed Minikube:

    - 2GB default memory isn't always enough

    ```console
    minikube start --memory=4096 --driver=none
    ```
