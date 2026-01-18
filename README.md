# SC-200 Lab: Red vs. Blue SOC Simulation
**Author:** Sunil Tiwari | **Status:** Phase 0 (Setup)

## Project Overview
This repository documents my preparation for the Microsoft Security Operations Analyst (SC-200) exam.
It simulates a "Live Fire" cyber range using Docker on ARM64 architecture.

## Architecture
- **Host:** NVIDIA DGX Spark (ARM64)
- **Victim:** Ubuntu 22.04 (Systemd enabled + Azure Arc)
- **Attacker:** Kali Linux
- **SIEM:** Microsoft Sentinel

## SC-200 Lab Roadmap
- [x] **Phase 0:** SC-200 Exam Awareness & Lab Setup
- [x] **Phase 1:** Lab Infrastructure & Telemetry Foundation
- [ ] **Phase 2:** Microsoft Sentinel Deployment & Configuration
- [ ] **Phase 3:** Microsoft Defender Integration
- [ ] **Phase 4:** Threat Simulation (Red Team)
- [ ] **Phase 5:** KQL & Threat Hunting
- [ ] **Phase 6:** Analytics Rules & Detections
- [ ] **Phase 7:** Incident Investigation & Response
- [ ] **Phase 8:** Automation & SOAR
- [ ] **Phase 9:** Workbooks, Visualization & Reporting
- [ ] **Phase 10:** Capstone & Portfolio

---
### Phase 0: SC-200 Exam Awareness & Lab Setup
**Status:** ✅ Completed | **Date:** Jan 17, 2026

**Objective:**
Initialize a professional "DevSecOps" workspace to simulate real-world Security Operations Center (SOC) engineering practices, aligned with SC-200 certification requirements.

**Key Actions:**
- **Version Control:** Established a Git repository (`sc200-portfolio`) to track Infrastructure-as-Code (IaC) changes.
- **Environment Prep:** Validated Docker and Git engines on the host machine (NVIDIA DGX Spark / ARM64).
- **Security Hygiene:** Configured `.gitignore` to prevent sensitive log leakage into the public repository.
- **Exam Mapping:** Created a "Master Roadmap" mapping lab phases directly to Microsoft Security Operations Analyst (SC-200) domains.

**Outcome:**
A structured, version-controlled foundation ready for infrastructure deployment.

---

### Phase 1: Lab Infrastructure & Telemetry Foundation
**Status:** ✅ Completed | **Date:** Jan 17, 2026

**Objective:**
Deploy an isolated "Red vs. Blue" cyber range using Docker containers that mimics an on-premise corporate environment, ensuring compatibility with Azure agents on ARM64 hardware.

**Architecture Implemented:**
- **Infrastructure-as-Code:** Wrote and deployed a `docker-compose.yml` file optimized for ARM64.
- **The Victim (Blue Team):** Ubuntu 22.04 container configured with `privileged` mode and cgroup mapping to support **Systemd (PID 1)**.
    - *Why Systemd?* Critical prerequisite for the Azure Connected Machine Agent (Arc).
- **The Attacker (Red Team):** Kali Linux container deployed on the shared `cyber-range` network for future threat simulation.
- **Data Persistence:** Mapped local volumes to `/var/log` to ensure log retention across container restarts.

**Verification:**
Validated Systemd functionality inside the victim container to ensure agent compatibility:
```bash
docker exec -it victim-linux ps 1
```

# Result: /lib/systemd/systemd (Success)

**Outcome:**
A fully functional, isolated cyber range capable of supporting enterprise cloud agents without polluting the host OS.

---

### Phase 2: Microsoft Sentinel Deployment & Configuration
**Status:** ✅ Completed | **Date:** Jan 17, 2026

**Objective:**
Establish a hybrid cloud SOC architecture by connecting a local ARM64 "On-Premise" asset to Microsoft Sentinel using Azure Arc and the Azure Monitor Agent (AMA).

**Architecture Implemented:**
- **Hybrid Bridge:** Onboarded local Docker container (`linux-server-01`) to Azure via **Azure Arc** (Script-based deployment).
- **SIEM Core:** Deployed **Log Analytics Workspace** (`law-sc200-cyber-range`) and enabled **Microsoft Sentinel**.
- **Data Pipeline:** Configured a **Data Collection Rule (DCR)** to manage granular log ingestion.
    - **Target:** Linux Syslog
    - **Facilities:** `auth`, `authpriv`, `syslog`, `user`
    - **Severity:** `LOG_INFO` (Tuned to exclude debug noise and optimize ingestion costs).

**Verification:**
Validated end-to-end connectivity using KQL to query the Azure Monitor Agent heartbeat.
```kusto
Heartbeat
| where Category == "Azure Monitor Agent"
| project TimeGenerated, Computer, Category, OSType

```
**Outcome:**
Successfully achieved real-time log ingestion from an external ARM64 node into Azure Sentinel, establishing the foundation for threat detection.
