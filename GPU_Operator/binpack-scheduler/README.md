# Apply 'Binpack' Scheduler

### All Steps

- **Apply Manifests**
```bash
k apply -f configmap.yaml
k apply -f serviceaccount.yaml
k apply -f rbac.yaml

# deployment toleration 추가
k apply -f deployment.yaml
```

- **Test 'Binpack' Scheduler**
```bash
# GPU Pod들이 GPU를 더많이 가동중인 노드에 배포되는지 확인
# spec.schedulerName 필드가 추가
k apply -f gpu-pod-with-binpack-scheduler-1.yaml
k apply -f gpu-pod-with-binpack-scheduler-2.yaml
```

## References
- [Configure Multiple Schedulers](https://kubernetes.io/docs/tasks/extend-kubernetes/configure-multiple-schedulers/)