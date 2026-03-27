
# 🚀 Project: Centralized Logging System with ELK Stack & Docker (Minimal Hardware)

This repository contains the configuration and documentation for a production-ready centralized logging system using the **ELK Stack (Elasticsearch, Logstash, Kibana)** deployed via **Docker Compose**. This project is specifically optimized to run on distributed environments with minimal hardware specifications.

---

## 📖 Deep Dive: Full Technical Writeup
For a step-by-step guide, technical challenges, and deep configuration insights, please read my featured article on Medium:
👉 **[Building a Centralized Logging System with ELK Stack & Docker on Minimal Hardware](https://medium.com/@indofrick/building-a-centralized-logging-system-with-elk-stack-docker-on-minimal-hardware-72ee2b960ccb)**

---

## 🏗️ Architecture Overview
The project is deployed across two Virtual Machines (VMware) running **Ubuntu Server 24.04** with limited resources (**2 vCPUs & 2GB RAM** each):

### 1. **ELK Node (Monitoring Server)**
* **Engine:** Docker & Docker Compose.
* **Elasticsearch:** Indexed storage with tuned JVM Heap size for low-RAM stability.
* **Logstash:** Real-time data processing and **Grok Filtering**.
* **Kibana:** Visualization and security auditing dashboard.

### 2. **Client Node (Production Server)**
* **Services:** Nginx Web Server & OpenSSH Server.
* **Shippers:** * **Filebeat:** For Nginx access/error logs and System/SSH auth logs.
    * **Metricbeat:** For real-time CPU, RAM, and Network performance metrics.

---

## ✨ Key Features
* **Centralized Log Ingestion:** Real-time streaming of Nginx and System logs.
* **Log Transformation:** Advanced Logstash Grok filters to parse raw text into structured data (Client IP, User, Status Code, etc.).
* **Security Observability:** Real-time tracking of unauthorized SSH access attempts (Failed logins/Brute force).
* **System Health Monitoring:** Comprehensive dashboard for resource usage across the infrastructure.
* **Dockerized Deployment:** "Infrastructure as Code" approach for easy replication.

---

## 🛠️ Quick Start

### ELK Server Setup
1. Clone this repository:
   ```bash
   git clone https://github.com/yourusername/elk-monitoring-project.git
   cd elk-monitoring-project
   ```
2. Start the stack:
   ```bash
   sudo docker compose up -d
   ```
3. Ensure ports `5044` (Logstash), `9200` (Elasticsearch), and `5601` (Kibana) are reachable.

### Client Server Setup
1. Install Filebeat & Metricbeat.
2. Enable necessary modules:
   ```bash
   sudo filebeat modules enable nginx system
   sudo metricbeat modules enable nginx system
   ```
3. Configure `output.logstash` in the `.yml` config files to point to your ELK Server IP.
4. Verify the connection:
   ```bash
   sudo filebeat test output
   ```

---

## 📊 Kibana Configuration
1. Access Kibana at `http://<YOUR-ELK-IP>:5601`.
2. Navigate to **Stack Management > Kibana > Data Views**.
3. Create Data Views for:
    * `filebeat-*`
    * `metricbeat-*`
    * `logstash-*` (if using custom Logstash indices).
4. Head to the **Discover** tab to explore your live logs.

---

## 🛡️ Troubleshooting
This project addresses common pitfalls in minimal ELK setups, including:
* **Data Path Locking:** Resolving `filebeat.lock` issues during output migration.
* **Grok Parsing Failures:** Custom patterns to handle Ubuntu 24.04 ISO8601 timestamps and PAM authentication variations.
* **JVM Tuning:** Optimizing memory limits to prevent out-of-memory (OOM) kills on 2GB RAM.

---

## 🤝 Contributing
Contributions, issues, and feature requests are welcome! If you have optimized Grok filters or Docker configurations for low-spec hardware, feel free to open a Pull Request.

---
