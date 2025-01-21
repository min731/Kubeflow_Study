# Install NVIDIA GPU Operator

## Prerequisites
- **NVIDIA Container Toolkit**: Required
- **NVIDIA Driver**: 560.35.03
- **CUDA Version**: 12.6
- **Helm**: v3.16.4

### All Steps

- **Add NVIDIA Helm Repository**
```bash
# Add and update helm repo
h repo add nvidia https://helm.ngc.nvidia.com/nvidia
h repo update
```
- **Download Helm Chart(yamls)**
```bash
# Download helm chart
h pull nvidia/gpu-operator --untar
```

- **Add GPU Node Taint**

```bash
k taint node mlops nvidia.com/gpu=present:NoSchedule
k taint node mlops-m02 nvidia.com/gpu=present:NoSchedule
```

- **Edit Chart Values**
```yaml
# if drivier already installed
driver:
  enabled: false
# only A100, H100...
migManager:
  enabled: false
# etc
vgpuManager:
  enabled: false
vgpuDeviceManager:
  enabled: false
vfioManager:
  enabled: false
```

- **Install GPU Operator**
```bash
h search repo gpu
h install gpu-operator nvidia/gpu-operator -n gpu-operator --create-namespace
k get all -n gpu-operator

# check nodes' labels
```

- **Test GPU Pod**
```bash
# can't deploy gpu-pod without toleration
k apply -f gpu-test-pod-with-operator-1.yaml

# must deploy gpu-pod with toleration
k apply -f gpu-test-pod-with-operator-2.yaml
# check logs
k logs gpu-test-pod-with-operator-2
```

- **Apply 'Binpack' Scheduler**
```bash
# To be continued on Multi-
```