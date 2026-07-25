# 🚀 Edge Service K8s Manifests

이 디렉토리는 `edge-service`의 EKS 배포를 위한 Kustomize 기반 매니페스트를 포함하고 있습니다.
(이 파일은 AntiGravity가 Phase 3 작전 중 생성한 DX 뼈대입니다.)

## 📁 구조 요약
- `base/`: 어떤 환경이든 변하지 않는 인프라 뼈대 (Deployment, Service, TargetGroupBinding 등)
- `overlays/prod/`: 운영(Prod) 환경을 위한 특화 패치 및 리소스 (Spot 유예 셧다운 패치, Zero-Trust NetworkPolicy, HPA 등)

## 🛠️ 검증 및 빌드 테스트 가이드
Kustomize 빌드 결과(최종 YAML 병합본)를 확인하려면 아래 명령어를 사용하세요.

```bash
# Prod 오버레이의 매니페스트 병합 결과 가상 확인
kubectl kustomize localy-manifests/workloads/edge-service/overlays/prod

# 렌더링된 매니페스트를 직접 파일로 추출하려면
kubectl kustomize localy-manifests/workloads/edge-service/overlays/prod > edge-service-prod-render.yaml
```

## 🚨 주의 사항 (AntiGravity Bridge)
- **Zero-Trust Egress:** Redis 포트(6379)는 명시적으로 차단되었습니다. 새로운 의존성이 추가될 경우 반드시 `network-policy.yaml`의 Egress 화이트리스트에 추가해야 합니다.
- **ALB 연동:** `target-group-binding.yaml`에 존재하는 더미 ARN은 반드시 AWS 인프라(Terraform 등) 생성 이후 실제 ARN으로 교체되어야 합니다.
