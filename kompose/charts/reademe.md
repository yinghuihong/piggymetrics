
- https://developer.aliyun.com/article/610430

- kompose不支持2.1模板版本，需对docker-compose.yml进行修改
    - 修改2.1为2
    - 不支持 condition
    - 增加 k8s server type annotation：

            labels: 
                kompose.service.type: loadbalancer 

- kompose convert -f docker-compose.yml -o piggymetrics -c
    - -c 生成 Helm charts

- [helm](https://github.com/helm/helm)
    - Helm is a tool for managing Charts. Think of it like apt/yum/homebrew for Kubernetes.
    - Charts are packages of pre-configured Kubernetes resources.
    - [Quickstart](https://helm.sh/docs/intro/quickstart/)
        - Install Helm
            - brew install helm
        - Initialize a Helm Chart Repository
            - helm repo add stable https://kubernetes-charts.storage.googleapis.com/
        - Once this is installed, you will be able to list the charts you can install:
            - helm search repo stable
        - Make sure we get the latest list of charts
            - helm repo update
        - Install an Example Chart
            - helm install stable/mysql --generate-name
        - Show a list of all deployed releases
            - helm ls
        - Uninstall a Release
            - helm uninstall mysql-1599792479 --keep-history
        - helm status mysql-1599792479

- 用 helm 一键部署 piggymetrics
    - cd kompose/charts/
    - helm install piggymetrics/ --generate-name
    - minikube dashboard