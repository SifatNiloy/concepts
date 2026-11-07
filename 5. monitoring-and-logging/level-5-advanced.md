# Level 5: Centralized Logging with Grafana Loki

When you have 10 containers running, running `docker logs` on each container individually is painful.  
You need **all logs from all containers streamed into a single search bar**.

Meet **Grafana Loki** (often called *"Prometheus, but for logs"*).

---

## 🪵 The Loki Architecture

```
[ Your Containers (Next.js, Postgres, Nginx) ]
                       │ (stdout / stderr)
                       ▼
                 [ Promtail ] (The Log Shipper)
                       │
                       ▼
                  [ Loki ] (The Log Database)
                       │
                       ▼
                 [ Grafana ] (The Search Bar in your browser!)
```

* **Promtail:** Gathers logs from `/var/lib/docker/containers/*` and ships them.
* **Loki:** Indexes metadata (labels like `container_name=my-app`) and stores the text compactly.
* **Grafana:** Lets you search, filter, and stream live logs.

---

## 🔍 LogQL: Searching Logs Like a Pro

Inside Grafana's **Explore** tab with Loki connected, you can search logs using **LogQL**:

```logql
# 1. View all logs from the nextjs container:
{container_name="my-nextjs-site"}

# 2. Filter for only lines containing the word "Error":
{container_name="my-nextjs-site"} |= "Error"

# 3. Filter for lines that do NOT contain 200 OK:
{container_name="nginx"} != "200"

# 4. Turn logs into a graph! (Count of errors per minute):
rate({container_name="my-nextjs-site"} |= "error" [1m])
```

---

## ✅ Level 5 Checklist

- [ ] I understand how Promtail ships container logs to Loki.
- [ ] I can filter logs by container label using LogQL.
- [ ] I can search for errors across all services from a single Grafana window.

👉 Open [level-6-pro.md](file:///d:/CODES/concepts/5-monitoring-and-logging/level-6-pro.md) to set up automated alerts to Discord or Telegram!
