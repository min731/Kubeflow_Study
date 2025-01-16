# Install NVIDIA GPU Operator

## Prerequisites
- **NVIDIA Container Toolkit**: Required
- **NVIDIA Driver**: 560.35.03
- **CUDA Version**: 12.6
- **Helm**: v3.16.4

### Installation Steps

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