
- [reference](https://kubernetes.io/docs/tasks/tools/install-minikube/)
- [docs](https://minikube.sigs.k8s.io/docs/)

- a tool that runs a single-node Kubernetes cluster in a virtual machine on your personal computer.
  
- Install [Minikube](https://github.com/kubernetes/minikube)
    - Install kubectl
        - Make sure you have kubectl installed.
    - Install a Hypervisor
        - If you do not already have a hypervisor installed, install one of these now:
            - HyperKit
            - VirtualBox
            - VMware Fusion
    - Install Minikube
        - brew install minikube
    - Confirm Installation
        - minikube start --driver=<driver_name>
        - minikube status
