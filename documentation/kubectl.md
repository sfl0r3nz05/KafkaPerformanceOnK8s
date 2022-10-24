# Install Kubectl

1. Install `kubectl`

    ```console
    curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/ kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
    ```

    ```console
    chmod +x ./kubectl 
    ```

    ```console
    sudo mv ./kubectl /usr/local/bin/kubectl 
    ```