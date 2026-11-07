# Level 4: Services & Ingress (Connecting to the Outside World)

In Kubernetes, Pods are ephemeral: they die and get recreated with **brand new random internal IP addresses**.

If your backend needs to talk to the database, how can it find it if the database IP changes every restart?  
**The Answer: A Kubernetes Service.**

---

## 🧭 What is a Service?

A **Service** provides a **permanent, stable IP and DNS name** in front of a group of Pods, acting as an internal load balancer.

```
                   SERVICE (Stable IP / DNS: "nextjs-service")
                                  │
         ┌────────────────────────┼────────────────────────┐
         ▼                        ▼                        ▼
     [ Pod 1 ]                [ Pod 2 ]                [ Pod 3 ]
(IP: 10.42.0.15)         (IP: 10.42.0.18)         (IP: 10.42.0.22)
```

---

## 📝 The 3 Types of Services

1. **`ClusterIP` (Default):** Accessible **only inside the cluster**. Great for databases and backend microservices.
2. **`NodePort`:** Opens a high port (e.g. `31234`) on every physical server node.
3. **`LoadBalancer`:** Asks your cloud provider (AWS/GCP) to create a real public cloud load balancer.

---

## 🌐 Ingress: The Cluster's Reverse Proxy

Instead of creating an expensive LoadBalancer service for every single app, you use an **Ingress Controller** (like Nginx Ingress or Traefik):

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nextjs-service
spec:
  selector:
    app: nextjs
  ports:
    - port: 80
      targetPort: 80

---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: nextjs-ingress
spec:
  rules:
    - http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: nextjs-service
                port:
                  number: 80
```

Apply both with `kubectl apply -f service-and-ingress.yaml`, and your app is routed through the Ingress controller!

---

## ✅ Level 4 Checklist

- [ ] I understand why Pod IPs are unreliable and why Services are required.
- [ ] I know the difference between `ClusterIP` (internal) and `LoadBalancer` (external).
- [ ] I understand how an Ingress acts as the single entry point for all cluster domains.

👉 Open [level-5-advanced.md](file:///d:/CODES/concepts/6-kubernetes-learning-guide/level-5-advanced.md) for ConfigMaps, Secrets, and Persistent Volumes!
