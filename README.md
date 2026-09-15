# Enterprise SOC Telemetry & Detection Lab

An end-to-end Security Operations Center (SOC) logging and detection engineering pipeline deployed on an isolated Ubuntu Linux environment using Docker, Elasticsearch, Kibana, and Filebeat.

## Architecture Overview
* **Hypervisor / Host:** VirtualBox on Apple Silicon ARM64 macOS
* **Operating System:** Ubuntu 24.04 LTS (Isolated NAT/Host networking)
* **SIEM & Analytics Engine:** Elasticsearch 8.15 & Kibana (Containerized via Docker)
* **Log Shipper / Forwarder:** Elastic Filebeat (Native ARM64 `filestream` inputs)
* **Monitored Telemetry:** Linux PAM authentication events (`/var/log/auth.log`) and system diagnostics (`/var/log/syslog`)

## Engineering & Deployment Workflow
1. **Containerized SIEM Orchestration:** Configured single-node Elasticsearch and Kibana instances via Docker Compose, tuning kernel memory parameters (`vm.max_map_count=262144`) for high-throughput indexing.
2. **Telemetry Ingestion Pipeline:** Configured Filebeat system log shippers to forward raw authorization streams directly into Elasticsearch on port `9200`.
3. **Adversary Emulation:** Simulated unauthorized SSH brute-force enumeration and unauthorized root privilege escalation (`su` authentication failure) to generate anomalous security telemetry.
4. **Detection Triage & Analysis:** Created Kibana Data Views (`filebeat-*`) and filtered KQL search queries (`pam_unix(su:auth): authentication failure`) to validate real-time incident detection.

## Attack Simulation
```bash
# 1. SSH Brute-Force Emulation
for u in admin root test evil_user; do
  ssh -o StrictHostKeyChecking=no -o ConnectTimeout=1 $u@localhost
done

# 2. Unauthorized Privilege Escalation
su - root -c "whoami"
