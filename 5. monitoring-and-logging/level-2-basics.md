# Level 2: Quick Monitoring with `docker stats` & Portainer

Before installing complex metric pipelines, let's explore the free tools you already have available right now.

---

## ⚡ The Built-in Tool: `docker stats`

Open PowerShell while your `my-nextjs-docker-app` containers are running and type:

```powershell
docker stats
```

You will see a live updating terminal dashboard:

```text
CONTAINER ID   NAME                 CPU %     MEM USAGE / LIMIT     MEM %     NET I/O           BLOCK I/O
032fc5de3edd   ...-db-1             0.12%     28.4MiB / 7.64GiB     0.36%     1.2kB / 0B        12.4MB / 0B
3d07d4daf3bd   my-nextjs-site       0.05%     44.2MiB / 7.64GiB     0.56%     4.5kB / 2.1kB     8.1MB / 0B
```

* **CPU %:** How hard your processor is working for this container.
* **MEM USAGE:** Exactly how many Megabytes of RAM the container is using.
* **NET I/O:** How much data has been sent and received over the network.

*(Press `Ctrl + C` to exit)*

---

## 🖥️ Portainer: The Beautiful Free Web Dashboard for Docker

If you prefer a rich graphical interface in your browser instead of PowerShell commands, **Portainer** is the #1 tool in the world.

### Launch Portainer in 1 command:
```powershell
docker run -d -p 9000:9000 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
```

Now open your browser:
👉 **`http://localhost:9000`**

Set a password, and you get:
* A visual dashboard of all running containers, images, volumes, and networks.
* 1-click start, stop, restart, and delete buttons.
* Live CPU/RAM graphs and built-in interactive container terminals right inside your browser!

---

## ✅ Level 2 Checklist

- [ ] I can inspect real-time CPU & RAM usage in terminal using `docker stats`.
- [ ] I can run Portainer and navigate my Docker environment through a web UI.

👉 Open [level-3-intermediate.md](file:///d:/CODES/concepts/5-monitoring-and-logging/level-3-intermediate.md) to set up **Prometheus**!
