
- [Learning Environment](https://kubernetes.io/docs/setup/learning-environment/)
    - [Minikube]()
    - [Kind](https://kind.sigs.k8s.io/)
    
- [Production environment](https://kubernetes.io/docs/setup/production-environment/)
    - [Container runtimes](https://kubernetes.io/docs/setup/production-environment/container-runtimes/)
        - Cgroup drivers
        - Docker 
        - CRI(Container Runtime Interface) runtime
            - [CRI-O](https://github.com/cri-o/cri-o/)
            - [Containerd](https://github.com/containerd/containerd)
            - [frakti](https://github.com/kubernetes/frakti#frakti)
    - Installing Kubernetes with deployment tools
        - Bootstrapping clusters with [kubeadm](https://github.com/kubernetes/kubeadm)
        - Installing Kubernetes with [kops](https://github.com/kubernetes/kops)
        - Installing Kubernetes with [Kubespray](https://github.com/kubernetes-sigs/kubespray)
    - Turnkey 云解决方案
        - 在阿里云上运行 Kubernetes，[ACK](https://www.alibabacloud.com/help/zh/doc-detail/86737.htm)
        - Running Kubernetes on Google Compute Engine，[GCE](https://kubernetes.io/docs/setup/production-environment/turnkey/gce/)