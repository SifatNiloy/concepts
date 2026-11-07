# Level 3: Prometheus & Exporters (Metric Collection)

While `docker stats` shows what is happening *right now*, it cannot tell you:
* *"Did our CPU spike yesterday at 3:00 AM?"*
* *"Is memory slowly increasing by 5MB every hour (a memory leak)?"*

For history over time, you need **Prometheus**.

---

## 📈 How Prometheus Works: The Pull Model

Prometheus is a **Time-Series Database (TSDB)**.

Unlike traditional monitoring tools where apps push data to a server, Prometheus uses a **Scrape (Pull) Model**:
1. Every 15 seconds, Prometheus sends an HTTP GET request to your servers and containers.
2. The targets respond with a plain-text list of metrics:
   ```text
   node_cpu_seconds_total 1452.3
   node_memory_MemFree_bytes 104857600
   http_requests_total{status="200"} 5420
   ```
3. Prometheus stores these timestamped numbers in its database.

---

## 🔌 What is an "Exporter"?

Most software (like Linux itself or Postgres) doesn't natively speak Prometheus format. An **Exporter** is a small adapter container that translates:

* **Node Exporter:** Translates Linux OS hardware stats (CPU, RAM, Disk, Network) into Prometheus metrics.
* **cAdvisor (Container Advisor):** Built by Google to translate all Docker container stats into Prometheus metrics.
* **Postgres Exporter:** Translates database query speeds and connection pool counts into Prometheus metrics.

---

## 📝 Step 1: The Prometheus Config (`prometheus.yml`)

Create `prometheus.yml`:

```yaml
global:
  scrape_interval: 15s # Scrape targets every 15 seconds

scrape_jobs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']

  - job_name: 'cadvisor'
    static_configs:
      - targets: ['cadvisor:8080']
```

---

## 🐳 Step 2: The Monitoring `docker-compose.yml`

```yaml
services:
  prometheus:
    image: prom/prometheus:latest
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
      - prometheus_data:/prometheus

  cadvisor:
    image: gcr.io/cadvisor/cadvisor:latest
    ports:
      - "8080:8080"
    volumes:
      - /:/rootfs:ro
      - /var/run:/var/run:ro
      - /sys:/sys:ro
      - /var/lib/docker/:/var/lib/docker:ro

volumes:
  prometheus_data:
```

---

## 🚀 Run and Explore

```powershell
docker compose up -d
```

Open:
👉 **`http://localhost:9090`**

In the query bar, type:
```text
container_memory_usage_bytes
```
Click **Execute** $\rightarrow$ switch to the **Graph** tab. You are now graphing live container memory over time!

---

## ✅ Level 3 Checklist

- [ ] I understand Prometheus's pull (scrape) model.
- [ ] I understand what an Exporter (like cAdvisor or Node Exporter) does.
- [ ] I can write PromQL queries to look up container metrics.

👉 Open [level-4-real-world.md](file:///d:/CODES/concepts/5-monitoring-and-logging/level-4-real-world.md) to turn these numbers into stunning **Grafana** dashboards!
