# Level 4: Grafana — Visual Dashboards (100% Free)

Prometheus is great at storing numbers, but its built-in graphs are plain.  
**Grafana** is the visual UI used by tech companies worldwide (Spotify, Uber, NASA) to display dashboards.

---

## 🎨 Adding Grafana to Docker Compose

Add Grafana to your monitoring stack:

```yaml
services:
  prometheus:
    image: prom/prometheus:latest
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
    networks:
      - monitor_net

  grafana:
    image: grafana/grafana:latest
    ports:
      - "3001:3000" # Map host 3001 to Grafana container 3000
    volumes:
      - grafana_data:/var/lib/grafana
    environment:
      - GF_SECURITY_ADMIN_PASSWORD=admin
    networks:
      - monitor_net

volumes:
  grafana_data:

networks:
  monitor_net:
```

---

## 🚀 Step-by-Step Setup:

1. Start the stack:
   ```powershell
   docker compose up -d
   ```
2. Open your browser to:
3. Log in with:
   * **Username:** `admin`
   * **Password:** `admin`

---

## 🔌 Connecting Prometheus to Grafana (2 Clicks):

1. Click **Connections** $\rightarrow$ **Data Sources** $\rightarrow$ **Add new data source**.
2. Select **Prometheus**.
3. Under **Prometheus server URL**, type:
   ```text
   http://prometheus:9090
   ```
   *(Notice: You use the Docker service name `prometheus`!)*
4. Click **Save & Test**. You will see: *"Successfully queried the Prometheus API"*.

---

## 🌟 The Magic: Community Dashboards (Zero Design Required!)

You don't need to build graphs by hand. The Grafana community has thousands of pre-made templates:

1. In Grafana, click **Dashboards** $\rightarrow$ **New** $\rightarrow$ **Import**.
2. Type dashboard ID **`14282`** (The famous Docker cAdvisor dashboard) or **`1860`** (Node Exporter Full).
3. Click **Load** $\rightarrow$ Select your Prometheus data source $\rightarrow$ Click **Import**.

**Boom!** You instantly have a production dashboard with:
* Real-time CPU gauges
* Per-container memory breakdown
* Network traffic charts
* Disk I/O read/write speeds

---

## ✅ Level 4 Checklist

- [ ] I can launch Grafana with Docker Compose.
- [ ] I can connect Prometheus as a data source inside Grafana.
- [ ] I know how to import community dashboards using an ID.
