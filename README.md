# demo-next

Next.js application — CI/CD demo with Rancher + ArgoCD

## Stack

| Layer              | Tool                                |
| ------------------ | ----------------------------------- |
| App                | Next.js                             |
| Container registry | Harbor `10.151.1.171`               |
| CI pipeline        | GitHub Actions (self-hosted runner) |
| CD                 | ArgoCD                              |
| Cluster            | RKE2 managed by Rancher             |

## CI/CD Flow

```
push to main
  └─ GitHub Actions: build → push image to Harbor
       └─ update image tag in k8s/deployment.yaml
            └─ ArgoCD detect diff → sync
                 └─ deploy to K8s cluster (demo-worker-01)
```

## Development

```bash
pnpm install
pnpm dev
```

## Docker

```bash
docker build -t demo-next .
docker run -p 3000:3000 demo-next
```

## Repository Structure

```
.
├── .github/workflows/
│   └── ci.yml           # GitHub Actions CI pipeline
├── k8s/
│   ├── namespace.yaml
│   ├── deployment.yaml  # image tag ถูก update โดย CI อัตโนมัติ
│   └── service.yaml
├── src/app/
├── public/
└── Dockerfile
```

## GitHub Actions Secrets

ต้องตั้งค่า secrets ใน GitHub repository ก่อนใช้งาน

| Secret            | Value           |
| ----------------- | --------------- |
| `HARBOR_USER`     | Harbor username |
| `HARBOR_PASSWORD` | Harbor password |

## Notes

- ใช้ self-hosted GitHub Actions runner ใน LAN เดียวกับ Harbor
- ArgoCD ติดตั้งใน K8s cluster (namespace `argocd`)
- สำหรับ demo เท่านั้น — production ควรเพิ่ม worker node และ HA
