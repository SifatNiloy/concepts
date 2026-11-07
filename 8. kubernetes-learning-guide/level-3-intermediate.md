# Level 3: Deployments & Self-Healing (Never Run Raw Pods!)

In Level 2, we created a single Pod. But in production, **engineers never create raw Pods**.

Why? Because if a raw Pod crashes or dies, **it stays dead**.

To get self-healing and scaling, you create a **Deployment**.

---

## 🛡️ What is a Deployment?

A Deployment is a manager that supervises a **ReplicaSet**:
* You state your desired goal: *"I want exactly 3 replicas of my Next.js app running."*
* If you manually delete Pod #2, the Deployment notices instantly and spawns a new replacement Pod #2 in milliseconds!

---

## 📝 The Deployment YAML (`deployment.yaml`)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nextjs-deployment
spec:
  replicas: 3 # Run 3 identical copies!
  selector:
    matchLabels:
      app: nextjs
  template:
    metadata:
      labels:
        app: nextjs
    spec:
      containers:
        - name: nextjs
          image: nginx:alpine # (Replace with your own Next.js image!)
          ports:
            - containerPort: 80
          resources:
            limits:
              memory: "256Mi"
              cpu: "250m"
```

Apply it:
```powershell
kubectl apply -f deployment.yaml
```

Check the status:
```powershell
kubectl get pods
```
Output:
```text
NAME                                 READY   STATUS    RESTARTS   AGE
nextjs-deployment-688bb6b4f7-8j4k2   1/1     Running   0          12s
nextjs-deployment-688bb6b4f7-kd9s1   1/1     Running   0          12s
nextjs-deployment-688bb6b4f7-x29fa   1/1     Running   0          12s
```

---

## 🧪 Try to Kill One! (Witness Self-Healing)

Copy the name of one of the pods and delete it:
```powershell
kubectl delete pod nextjs-deployment-688bb6b4f7-8j4k2
```

Now immediately run:
```powershell
kubectl get pods
```
You will see that the dead pod is gone, and a **brand new Pod was already created** to keep the count at 3!

---

## 🔄 Zero-Downtime Rolling Updates

When you release version 2 of your app:
```powershell
kubectl set image deployment/nextjs-deployment nextjs=nginx:1.25-alpine
```

Kubernetes does a **Rolling Update**:
1. Starts 1 new Pod with the new image.
2. Waits until it passes health checks.
3. Terminates 1 old Pod.
4. Repeats for the remaining pods.

**Your users never experience a single millisecond of downtime!**

---

## ✅ Level 3 Checklist

- [ ] I understand why Deployments are preferred over raw Pods.
- [ ] I can scale pods by changing the `replicas:` number.
- [ ] I tested self-healing by deleting a pod.
- [ ] I understand how rolling updates achieve zero downtime.
