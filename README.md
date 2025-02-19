# 🐳 Kubeflow_Study

![image](https://github.com/user-attachments/assets/5d03e3c4-8a17-4d0a-bf37-c11bb21aa3f1)

# 📅 날짜별 기록<br>

### ✔️ 수행 기간
- 2025.01.16 ~ ing

### ✔️ 세부 진행 기록
- 25-01-16: Minikube 클러스터 생성, Istio 구축 및 예제 구현  
- 25-01-21: Helm 활용 GPU Operator 배포 및 예제 구현  
- 25-01-22: 
  - Minikube 클러스터(단일 GPU) 환경에서 Binpack 스케줄러 구현 시도(실패)  
  - Minikube Cluster 환경에서 Kubeflow 모든 컴포넌트 설치 시도(실패)  
  - 개발 컴포넌트 설치 예정  
- 25-01-25: Kubeflow Jupyter Web App 환경 구축 및 접속 (Cert-Manager, Istio, Oauth2-Proxy, Dex, Metallb)  
- 25-01-27: Kubeflow OIDC Flow 확인(Istio, Oauh2-Proxy, Dex), Trouble Shooting: kubelet 'Too Many Open Files'
- 25-01-29: Kubeflow CRD RBAC (Profile, Rolebinding) 확인, CSRF-TOKEN 미인증 설정
- 25-01-30: Kubeflow ClusterRole, Admission Webhook, Profile Controller/Access-Management 배포
- 25-02-03: Jupyter Web App 예제 구현 리소스 부족 확인 및 플랫폼 삭제, 캐시 초기화
- 25-02-08: Kubeflow Pipeline Standalone 배포 및 UI 접속, 리소스 한계로 인한 Run 불가 확인

📒 참고 자료<br>

- [GPU Operator](https://heumsi.github.io/blog/posts/setup-gpu-env-in-k8s/)
- [Kubeflow Notebooks](https://www.kubeflow.org/docs/components/notebooks/)
