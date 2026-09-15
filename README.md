# Enterprise SOC & Detection Engineering Lab (Wazuh SIEM)

A practical Security Operations Center (SOC) monitoring and detection engineering lab deployed in a virtualized network environment. This project demonstrates centralized telemetry ingestion, attack emulation mapped to MITRE ATT&CK, and custom SIEM rule engineering.

## Architecture & Components
* **SIEM / EDR Server:** Wazuh All-in-One Manager (Ubuntu Server / OpenSearch Indexer)
* **Monitored Endpoint:** Ubuntu Linux Agent with Auditd & File Integrity Monitoring (FIM)
* **Networking:** Segmented virtual network with real-time log forwarding over port 1514
* **Hypervisor:** VirtualBox

## Detection Engineering & Use Cases
1. **Binary Drop & Persistence (MITRE ATT&CK T1037):**
   * *Telemetry:* Real-time File Integrity Monitoring (`<directories check_all="yes" realtime="yes">/bin,/usr/bin</directories>`).
   * *Detection Rule:* Custom rule `100002` (Level 12) monitoring anomalous writes and unauthorized executables inside `/bin/`.
2. **Adversary Emulation:**
   * Automated command execution mimicking persistence techniques and privilege modifications.
   * Log analysis confirming alert triggers in `/var/ossec/logs/alerts/alerts.json` and Wazuh UI.

## Repository Contents
* `rules/local_rules.xml`: Production-ready custom XML detection rules.
* `simulations/test_attacks.sh`: Scripts used to test and trigger alerts.
* `docs/screenshots/`: Visual evidence of alert triage and event dashboards.

## Verification & Triage
* Alerts successfully ingested and mapped inside the Wazuh Dashboard with zero CPU regression.
* Validated through `wazuh-logtest` and live adversary emulation.
