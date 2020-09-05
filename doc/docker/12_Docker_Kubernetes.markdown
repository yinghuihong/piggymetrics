
- [docker desktop for mac kubernetes manual](https://docs.docker.com/docker-for-mac/#kubernetes)
    - Docker Desktop includes a standalone Kubernetes server that runs on your Mac
    - 可切换上下文，指向其它环境，如minikube、GKE（google kubernetes engine） cluster
        - $ kubectl config get-contexts
        - $ kubectl config use-context docker-desktop
    - Enable Kubernetes 勾选后，docker desktop for mac 将：
        - 安装 kubectl
        - 将 Kubernetes server 相关镜像实例化为容器
            - k8s.gcr.io/kube-proxy
            - k8s.gcr.io/kube-controller-manager
            - k8s.gcr.io/kube-apiserver
            - k8s.gcr.io/kube-scheduler
            - k8s.gcr.io/pause
            - k8s.gcr.io/coredns
            - k8s.gcr.io/etcd


    - kubectl apply -f pod.yaml
    - kubectl get pods
    - kubectl logs demo
    - kubectl delete -f pod.yaml


- [Deploy to Kubernetes](https://docs.docker.com/get-started/kube-deploy/)
    - kubectl apply -f bb.yaml
    - kubectl get deployments
    - kubectl get services
    - kubectl delete -f bb.yaml




- [other reference](https://kubernetes.io/docs/concepts/workloads/pods/)

