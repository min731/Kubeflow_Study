# Setting Cluster

## Prerequisites

### Hardware Requirements
- **CPU**: AMD Ryzen 5 4600H
  - 12 Cores
- **RAM**: Kingston LV32D4S2S8HD-8
  - 8GB DDR4 3200MHz
- **GPU**: NVIDIA GeForce GTX 1650 Ti
  - 4GB VRAM
  - NVIDIA Driver version: 560.35.03 
  - CUDA Toolkit version: 12.6

### Software Requirements
- **Operating System**: Ubuntu 22.04.1
  - 6.8.0-49-generic
- **Container Runtime**: Docker
  - Docker version 27.3.1, build ce12230
- **Kubernetes**: Minikube
  - Minikube version: v1.34.0
- **Package Manager**: Helm Chart
  - Helm version: v3.16.4
- **Additional Tools**
  - Kubectl version: v1.31.1
  - Kustomize version: v5.4.2

### Alias
```bash
alias k='kubectl'
alias d='docker'
alias mk='minikube'
alias h='helm'
alias ls='ls -alFh'
```
## Install Cluster

```bash
# 1 control plane, 1 worker node 
minikube start -p "mlops" \
  --nodes="2" \
  --cpus="4" \
  --memory="2560" \
  --disk-size="50g" \
  --driver="docker" \
  --kubernetes-version="v1.31.0" \
  --gpus="all" \
  --addons="metrics-server,ingress,storage-provisioner"
```
```bash
# delete profile
minikube delete -p {{PROFILE_NAME}}
```
```bash
# get nodes
k get nodes --show-labels
```

```bash
# label node
k label node mlops-m02 node-role.kubernetes.io/control-plane-
k label node mlops-m02 node-role.kubernetes.io/master-
k label node mlops-m02 node-role.kubernetes.io/worker=worker
```

```bash
# check label
/jmlim2/kubeflow_study$ k get nodes
NAME        STATUS   ROLES           AGE     VERSION
mlops       Ready    control-plane   5m49s   v1.31.0
mlops-m02   Ready    worker          3m58s   v1.31.0
```