# Level 6: Pro - Automated Alerting to Discord & Telegram

Nobody stares at Grafana dashboards 24 hours a day. The hallmark of a senior engineer is **setting up alerts so the system notifies you when something breaks**.

---

## 🚨 The Alert Workflow

```
1. Condition: "Is Container RAM > 85% for more than 5 minutes?"
                       │
                       ▼ (YES)
2. Alert Triggered in Grafana / Alertmanager
                       │
                       ▼
3. Webhook sends message to Discord #alerts or Telegram Bot
                       │
                       ▼
4. Your phone buzzes at 3:00 AM with the exact container name and graph link!
```

---

## 🤖 100% Free Integration: Discord Webhook

You don't need paid PagerDuty accounts. Discord webhooks are instant and free:

### Step 1: Create a Discord Webhook
1. In your personal Discord server, create a channel named `#devops-alerts`.
2. Click Channel Settings (gear icon) $\rightarrow$ **Integrations** $\rightarrow$ **Webhooks**.
3. Click **New Webhook**, name it "Grafana Bot", and click **Copy Webhook URL**.

### Step 2: Add Contact Point in Grafana
1. In Grafana, click **Alerting** $\rightarrow$ **Contact points**.
2. Click **Add contact point**.
3. Name: `Discord Devops`
4. Type: **Discord**
5. Paste your Discord Webhook URL.
6. Click **Test**. Check your Discord channel — you will instantly receive a test alert! 🔔

---

## 📊 Creating an Alert Rule

1. In Grafana, go to **Alerting** $\rightarrow$ **Alert rules** $\rightarrow$ **New alert rule**.
2. Query: `avg(container_memory_usage_bytes{name="my-nextjs-site"}) / 1024 / 1024`
3. Condition: **IS ABOVE 300** (Trigger if RAM exceeds 300 MB).
4. For: `5m` (Must stay above 300MB for 5 consecutive minutes to ignore temporary spikes).
5. Notification: Send to `Discord Devops`.

Now your infrastructure monitors itself!

---

## 🏆 You Have Mastered Full Observability!

You can now:
* Inspect live container resources via `docker stats` and Portainer.
* Collect time-series metrics with Prometheus.
* Build high-impact visual dashboards in Grafana.
* Aggregate and search multi-container logs with Loki.
* Receive automated alerts on your phone whenever systems degrade.

👉 Next: The final frontier of container orchestration:  
Proceed to: [6-kubernetes-learning-guide](file:///d:/CODES/concepts/6-kubernetes-learning-guide)
