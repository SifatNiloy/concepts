# Level 2: Local Cluster Setup & The First Pod (100% Free)

You don't need expensive cloud clusters (like AWS EKS or GCP GKE, which cost \$75+/month just to exist).  
You can run a **real, full-featured Kubernetes cluster locally on your laptop inside Docker** for free!

---

## 🛠️ The Tool: K3d (or Minikube)

We recommend **K3d** (lightweight Kubernetes inside Docker) or **Minikube**.

### Step 1: Install `kubectl` (The Kubernetes Command-Line Tool)
`kubectl` (pronounced "cube-control") is the CLI tool you use to control any Kubernetes cluster.

In PowerShell (with `winget` or `choco`):
```powershell
winget install Kubernetes.kubectl
```
Verify:
```powershell
kubectl version --client
```

### Step 2: Install K3d (Runs K8s inside Docker):
```powershell
winget install Rancher.k3d
```

### Step 3: Create your free cluster in 30 seconds!
```powershell
k3d cluster create my-cluster --port "8080:80@loadbalancer"
```

Verify your cluster is healthy:
```powershell
kubectl get nodes
```
You will see your worker node status: `Ready`! 🎉

---

## 🥜 What is a "Pod"?

In Docker, the smallest unit is a **Container**.  
In Kubernetes, you never run containers directly. The smallest unit is a **Pod**.

* Think of a Pod like a **pea pod** containing 1 or more containers that share the same IP address and storage.
* 99% of the time, a Pod holds **exactly one container** (e.g. 1 Pod = 1 running instance of your Next.js app).

---

## 🎯 Hands-On: Run your first Pod

Create a file named `first-pod.yaml`:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-first-pod
  labels:
    app: web
spec:
  containers:
    - name: nginx-container
      image: nginx:alpine
      ports:
        - containerPort: 80
```

### Apply it to the cluster:
```powershell
kubectl apply -f first-pod.yaml
```

### Inspect your Pod:
```powershell
# See running pods:
kubectl get pods

# View detailed information:
kubectl describe pod my-first-pod

# View pod logs:
kubectl logs my-first-pod

# Delete the pod:
kubectl delete -f first-pod.yaml
```

---

## ✅ Level 2 Checklist

- [ ] I installed `kubectl` and launched a local cluster with `k3d`.
- [ ] I understand what a Pod is.
- [ ] I can deploy and inspect a Pod using `kubectl apply` and `kubectl get pods`.

👉 Open [level-3-intermediate.md](file:///d:/CODES/concepts/6-kubernetes-learning-guide/level-3-intermediate.md) to discover **Deployments & Self-Healing**!
