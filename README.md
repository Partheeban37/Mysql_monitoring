
---

# 📈 MySQL Monitoring with Prometheus & Grafana (Docker + Host MariaDB)

This project sets up a monitoring stack for a **MariaDB/MySQL instance running on the host** using:

- **Prometheus** (metrics collection)
- **mysqld-exporter** (MySQL metrics exporter)
- **Grafana** (visualization)
- **Docker Compose** (container orchestration)

---

## 📂 Project Structure

```

task1/
├── grafana/
│   └── provisioning/
│       └── datasources/
│           └── datasource.yml
├── my.cnf
├── mysql-db/
├── prometheus.yml
└── prometheus-mysql.yml

````

---

## ✅ Prerequisites

- Docker & Docker Compose installed
- MariaDB/MySQL running on the host machine (outside Docker)
- MariaDB must be configured to allow remote connections

---

## ⚙️ Setup Steps

### 1. **Allow MariaDB Remote Access**

- Set `bind-address = 0.0.0.0` in your MariaDB config (e.g. `/etc/mysql/mariadb.conf.d/50-server.cnf`)
- Restart MariaDB:
  ```bash
  sudo systemctl restart mariadb


* Grant remote access to `root`:

  ```sql
  GRANT ALL ON *.* TO 'root'@'%' IDENTIFIED BY 'yourpassword';
  FLUSH PRIVILEGES;
  ```

---

### 2. **Get Host IP (for Docker Access)**

Run:

```bash
ip -4 addr show docker0 | grep -oP '(?<=inet\s)\d+(\.\d+){3}'
```

Use this IP (e.g., `172.17.0.1`) in `my.cnf` under the `host` field.

---

### 3. **Start Monitoring Stack**

From the project root:

```bash
docker-compose -f prometheus-mysql.yml up -d
```

---

### 4. **Verify Connections**

* Visit Prometheus: [http://localhost:9090/targets](http://localhost:9090/targets)

  * Check that `mysql-exporter:9104` is **UP**
* Visit Grafana: [http://localhost:3000](http://localhost:3000)

  * Login: `admin / your-password`
  * Prometheus is pre-configured as a data source

---

### 5. **Import Dashboard in Grafana**

1. Go to **+ ➝ Import**
2. Use dashboard ID:

   * `7362` → MySQL Overview
   * `14057` → MySQL Exporter Full Stats

---

## 🚀 Done!

You should now see real-time MySQL metrics in Grafana coming from your host-based MariaDB.

---

## 🧾 Credentials Used (example)

> Change these as needed

* **MariaDB root user**: `root`
* **MariaDB password**: `mr.looser37`
* **Grafana admin**: `admin / mr.looser37`

---
<img width="1920" height="1080" alt="Screenshot from 2025-08-05 16-46-21" src="https://github.com/user-attachments/assets/13f039eb-ed9c-41d5-9bb6-665d07830b53" />
