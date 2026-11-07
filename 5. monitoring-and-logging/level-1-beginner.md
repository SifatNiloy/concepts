# Level 1: The Three Pillars of Observability

When an application slows down or crashes in production, how do you diagnose it?  
In modern software engineering, we use **Observability**.

---

## 🏛️ The Three Pillars

```                                                      
┌────────────────────────────────────────────────────────────────────────┐
│                        OBSERVABILITY TRINITY                            │
│                                                                         │
│   1. METRICS (The Vital Signs) ──▶ "How much RAM is container X using?" │
│   2. LOGS (The Event Journal)  ──▶ "Error: database connection timeout" │
│   3. TRACES (The Journey Map)  ──▶ "User request took 340ms across 4 API calls" │
└────────────────────────────────────────────────────────────────────────┘
```

### 1. Metrics (Numbers over Time)
* Examples: CPU % used, Memory MB used, HTTP requests per second, error rate %.
* Handled by: **Prometheus**, **Node Exporter**.
* Great for: Graphs, dials, and triggering alerts when RAM > 90%.

### 2. Logs (Text messages written by programs)
* Examples: `console.log("User logged in")`, `stderr: Database connection failed`.
* Handled by: **Grafana Loki**, **Promtail**.
* Great for: Debugging specific bugs and seeing the exact stack trace.

### 3. Traces (Tracking a single request through microservices)
* Tracks a request as it hops from Frontend $\rightarrow$ Backend $\rightarrow$ Database $\rightarrow$ Stripe API.
* Handled by: **OpenTelemetry**, **Jaeger**.

---

## 🩺 The Hospital Analogy

Think of your cloud server like a patient in an ICU:
* **Metrics** are the heart-rate monitor and blood pressure cuff (continuous numbers).
* **Logs** are the doctor's written notes in the patient's chart.
* **Traces** are the medical tests showing how medicine moved through the body.

---

## ✅ Level 1 Checklist

- [ ] I understand the difference between Metrics, Logs, and Traces.
- [ ] I know why relying only on manual `docker logs` fails when running dozens of containers.

👉 Open [level-2-basics.md](file:///d:/CODES/concepts/5-monitoring-and-logging/level-2-basics.md) to monitor containers instantly with `docker stats` and **Portainer**!
