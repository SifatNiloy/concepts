# Level 5: ConfigMaps, Secrets & Persistent Volumes (PVC)

Applications need two things to be production-ready:
1. Environment variables and secrets.
2. Persistent hard drives so database data isn't deleted when a pod restarts.

---

## ⚙️ ConfigMaps vs Secrets

* **ConfigMap:** For non-sensitive configurations (e.g. `PORT=3000`, `NODE_ENV=production`).
* **Secret:** For passwords, API keys, and certificates (stored base64-encoded).

### Example YAML:
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  NODE_ENV: "production"
  APP_THEME: "dark"

---
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
stringData:
  DB_PASSWORD: "SuperSecretPassword123"
```

In your Deployment YAML, you inject them into your containers:
```yaml
envFrom:
  - configMapRef:
      name: app-config
  - secretRef:
      name: db-secret
```

---

## 💾 Persistent Volume Claims (PVC)

If a Postgres pod crashes, Kubernetes destroys its container. Without persistent storage, **all your database tables are lost**.

In Kubernetes, you create a **PersistentVolumeClaim (PVC)**:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: postgres-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 10Gi
```

When you mount this PVC to `/var/lib/postgresql/data` in your Postgres pod, the data is stored on a real disk outside the pod. If the pod dies and revives on a different server, Kubernetes automatically detaches the disk from the old server and plugs it into the new one!

---

## ✅ Level 5 Checklist

- [ ] I can store non-sensitive config in ConfigMaps.
- [ ] I can store credentials in Secrets.
- [ ] I understand how PVCs protect database data from pod restarts.
