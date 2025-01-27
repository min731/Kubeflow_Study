# Kubelet 'Too Many Open Files'

## 문제 상황

```bash
# kube-proxy 에러
E0127 04:41:27.277291       1 run.go:72] "command failed" err="failed complete: too many open files"

# nvidia-device-plugin 에러
E0127 04:44:44.372984       1 main.go:173] failed to create FS watcher for /var/lib/kubelet/device-plugins/: too many open files
```

## 해결 단계

### 1. 시스템 파일 디스크립터 제한 설정
```bash
# sysctl.conf 파일 수정
sudo vi /etc/sysctl.conf

# 다음 내용 추가
fs.inotify.max_user_instances = 8192
fs.inotify.max_user_watches = 524288
fs.file-max = 2097152

# 설정 적용
sudo sysctl -p
```

### 2. Docker 설정 수정

```bash
# Docker daemon 설정 파일 수정
sudo vi /etc/docker/daemon.json

# 다음 내용 추가/수정 (기존 nvidia 설정 유지)
{
  "default-runtime": "nvidia",
  "runtimes": {
    "nvidia": {
      "path": "nvidia-container-runtime",
      "runtimeArgs": []
    }
  },
  "default-ulimits": {
    "nofile": {
      "Name": "nofile",
      "Hard": 524288,
      "Soft": 524288
    }
  }
}

# Docker 재시작
sudo systemctl restart docker
```

### 3. systemd 서비스 제한 설정

```bash
# Docker 서비스 limits 설정 디렉토리 생성
sudo mkdir -p /etc/systemd/system/docker.service.d/

# limits.conf 파일 생성 및 설정
sudo vi /etc/systemd/system/docker.service.d/limits.conf

# 다음 내용 추가
[Service]
LimitNOFILE=524288

# systemd 데몬 리로드
sudo systemctl daemon-reload
```

### 4. 문제가 있는 파드 재시작

```bash
# kube-proxy 재시작
kubectl delete pod -n kube-system -l k8s-app=kube-proxy

# nvidia-device-plugin 재시작
kubectl delete pod -n gpu-operator -l app=gpu-operator
```

## 확인

```bash
# 현재 file descriptor 제한 확인
ulimit -n

# 시스템 전체 제한 확인
cat /proc/sys/fs/file-max

# Docker 설정 확인
cat /etc/docker/daemon.json
```

```bash
# 모든 네임스페이스의 파드 상태 확인
kubectl get pods -A

# 특정 파드의 로그 확인
kubectl logs -n kube-system <kube-proxy-pod-name>
```