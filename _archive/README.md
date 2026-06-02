# demo-next

Next.js application for Container Platform (OKD / OpenShift)

## Getting Started

### Development

```bash
pnpm install
pnpm dev
```

### Production

```bash
pnpm build
pnpm start
```

### Docker

```bash
docker build -t demo-next .
docker run -p 3000:3000 demo-next
```

Open [http://localhost:3000](http://localhost:3000)

## OKD Deployment

Deploy ผ่าน YAML โดยใช้ไฟล์ `k8s/demo-next-okd.yaml` ซึ่งประกอบด้วย
ImageStream, BuildConfig, Deployment, Service และ Route

```
Administrator → + → Import YAML → วาง YAML → Create
```

## Notes

### pnpm-workspace.yaml

ไฟล์ `pnpm-workspace.yaml` ถูกลบออกจาก repo นี้ เนื่องจากแค่การมีอยู่ของไฟล์
ทำให้ pnpm ตีความ repo เป็น monorepo workspace ทันที ส่งผลให้ทุก pnpm command
error ว่า "packages field missing or empty" แม้จะไม่ได้ใช้งาน workspace จริงๆ

> A workspace must have a `pnpm-workspace.yaml` file in its root.
> — [pnpm Workspaces](https://pnpm.io/workspaces)

หาก pnpm เวอร์ชันใหม่แจ้งเตือน ignored build scripts ของ `sharp` หรือ `unrs-resolver`
ให้เพิ่มใน `package.json` แทน:

```json
"pnpm": {
  "ignoredBuiltDependencies": ["sharp", "unrs-resolver"]
}
```

อ้างอิง: [pnpm-workspace.yaml](https://pnpm.io/pnpm-workspace_yaml) · [pnpm Settings](https://pnpm.io/settings) · [Issue #8968](https://github.com/pnpm/pnpm/issues/8968)
