# Jupyter Web App 

## Installation Steps

### 1. Clone Specific Version
```bash
git clone https://github.com/kubeflow/manifests.git
cd manifests

# Verify version
git describe --tags
```

### 2-1. Install cert-manager

```bash
kustomize build common/cert-manager/base | kubectl apply -f -
kustomize build common/cert-manager/kubeflow-issuer/base | kubectl apply -f -
echo "Waiting for cert-manager to be ready ..."
kubectl wait --for=condition=ready pod -l 'app in (cert-manager,webhook)' --timeout=180s -n cert-manager
kubectl wait --for=jsonpath='{.subsets[0].addresses[0].targetRef.kind}'=Pod endpoints -l 'app in (cert-manager,webhook)' --timeout=180s -n cert-manager
```

```bash
k get all -n cert-manager
```

### 2-2. Install Istio

```bash
echo "Installing Istio configured with external authorization..."
kustomize build common/istio-1-24/istio-crds/base | kubectl apply -f -
kustomize build common/istio-1-24/istio-namespace/base | kubectl apply -f -
kustomize build common/istio-1-24/istio-install/overlays/oauth2-proxy | kubectl apply -f -

echo "Waiting for all Istio Pods to become ready..."
kubectl wait --for=condition=Ready pods --all -n istio-system --timeout 300s
```

```bash
k get all -n istio-system
```

### 2-3. Create the namespace.

```bash
kustomize build common/kubeflow-namespace/base | kubectl apply -f -
```

```bash
k get ns --show-labels
```

### 2-4. Kubeflow Istio Resources (*외부 접속 Gateway 생성)

```bash
kustomize build common/istio-1-24/kubeflow-istio-resources/base | kubectl apply -f -
```

### 2.5 Oauth2-proxy, Dex (인증)

```bash
# Oauth2-proxy
kustomize build common/oauth2-proxy/overlays/m2m-dex-only/ | kubectl apply -f -
kubectl wait --for=condition=ready pod -l 'app.kubernetes.io/name=oauth2-proxy' --timeout=180s -n oauth2-proxy
```

```bash
# Dex
echo "Installing Dex..."
kustomize build common/dex/overlays/oauth2-proxy | kubectl apply -f -
kubectl wait --for=condition=ready pods --all --timeout=180s -n auth
```

### 2-6. Network Policies는 생략 (개발 환경) 

### 2-7. Install the Notebook Controller

```bash
kustomize build apps/jupyter/notebook-controller/upstream/overlays/kubeflow | kubectl apply -f -
```

```bash
k get all -n kubeflow
```

### 2-8. Install the Jupyter Web App

```bash
kustomize build apps/jupyter/jupyter-web-app/upstream/overlays/istio | kubectl apply -f -
```

```bash
k get all -n kubeflow
```

### Metallb (외부 IP 할당)

```bash
# Metallb addons 활성화
mk -p mlopds addons enable metallb
mk -p mlops addons list
k get all -n metallb-system
# LoadBalancer 외부 IP 범위 설정
k apply -f metallb/metallb_config.yaml
```

```bash
k get svc -n istio-system
# Service Type을 LoadBalancer로 바꾸어 외부 IP 할당
k edit svc istio-ingressgateway -n istio-system
k get svc -n istio-system
```

### 4. Check Resources

```bash
# 정렬 옵션
kubectl top nodes --sort-by=cpu     # CPU 사용량으로 정렬
kubectl top nodes --sort-by=memory  # 메모리 사용량으로 정렬

# 추가 정보 표시
kubectl top nodes --show-capacity   # 전체 용량 표시
kubectl top nodes -l key=value      # 라벨로 필터링

# 포맷팅
kubectl top nodes -o json          # JSON 형식
kubectl top nodes -o yaml          # YAML 형식
```

### 3. Use Jupyter Web App

```bash
# 브라우저 접속
http://192.168.49.3/jupyter/
# ID: user@example.com
# PASSWORD: 12341234
```

![alt text](image-3.png)
![alt text](image-5.png)
![alt text](image-6.png)
