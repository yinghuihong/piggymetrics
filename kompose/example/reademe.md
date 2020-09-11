- [kompose](https://kompose.io/)

- wget https://raw.githubusercontent.com/kubernetes/kompose/master/examples/docker-compose-v3.yaml -O docker-compose.yaml

- Install Kompose on Linux or macOS
        
        # Linux
        curl -L https://github.com/kubernetes/kompose/releases/download/v1.19.0/kompose-linux-amd64 -o kompose
        
        # macOS
        curl -L https://github.com/kubernetes/kompose/releases/download/v1.19.0/kompose-darwin-amd64 -o kompose
        
        chmod +x kompose
        sudo mv ./kompose /usr/local/bin/kompose

    - brew install kompose
        
- Install minikube
    - brew install minikube
        
- https://kompose.io/getting-started/
    - Start minikube:
        - minikube start
    - Convert your Docker Compose file to Kubernetes:
        - kompose convert
    - Alternatively, you can convert and deploy directly to Kubernetes with kompose up
        - kompose up
            - 遇到错误：FATA Error while deploying application: Get http://localhost:8080/api: dial tcp [::1]:8080: connect: connection refused 
                - 解决方案[https://github.com/kubernetes/kompose/issues/1201]：配置api-server地址为环境变量
                    - kubectl cluster-info
                        - Kubernetes master is running at https://192.168.99.104:8443
                    - vim .bash_profile
                        - export KUBERNETES_MASTER=https://192.168.99.104:8443
            - 遇到错误：FATA Error while deploying application: the server could not find the requested resource 
                - apps/v1 下才有 Deloyment，手动修改
            - spec下添加selector配置
    - Access the newly deployed service:
        - minikube service frontend
    - Otherwise, use kubectl to see what IP the service is using:
        - kubectl describe svc frontend