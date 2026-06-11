# Fintech Detection Lab

## Overview

This project documents the complete build process of a fintech-focused detection engineering lab.

The goal is to simulate realistic fintech attack scenarios such as:

- Credential Stuffing
- Insider Data Exfiltration
- API Abuse
- Privilege Escalation

and build detection content that demonstrates hands-on security engineering skills using:

- Elastic SIEM
- Elasticsearch
- Kibana
- Fleet Server
- Elastic Agent
- Kali Linux
- Ubuntu

---

# Lab Architecture

Three VMs running on VirtualBox connected through a Host-Only network.

| VM | IP Address | Specs | Role |
|----|------------|--------|------|
| Kali Linux | 192.168.56.10 | 2GB RAM / 20GB Disk | Attacker |
| Ubuntu Victim | 192.168.56.20 | 2GB RAM / 20GB Disk | Application Server |
| Elastic SIEM | 192.168.56.30 | 6GB RAM / 50GB Disk | Elasticsearch + Kibana + Fleet |

---

## Architecture Diagram

![Fintech Detection Lab Architecture](./docs/architecture.png)

### Current Environment

```text
Windows Host (16GB RAM)
│
├── Kali Linux (192.168.56.10)
│   ├── 2GB RAM / 20GB Disk
│   ├── Hydra
│   └── Python attack scripts
│
├── Ubuntu Victim (192.168.56.20)
│   ├── 2GB RAM / 20GB Disk
│   ├── Elastic Agent
│   └── Flask API (Week 2)
│
└── Elastic SIEM (192.168.56.30)
    ├── Elasticsearch :9200
    ├── Kibana :5601
    └── Fleet Server :8220
```

### Log Flow

```text
Ubuntu Victim
      │
      ▼
Elastic Agent
      │
      ▼
Fleet Server (:8220)
      │
      ▼
Elasticsearch (:9200)
      │
      ▼
Kibana (:5601)
      │
      ▼
Windows Browser
```

### Week 1 Status

✅ Host-only network operational

✅ Elasticsearch healthy

✅ Kibana accessible

✅ Fleet Server enrolled

✅ Elastic Agent connected

✅ 1,679 log events successfully ingested
# Network Layout

```text
Windows Host
│
├── Kali Linux (192.168.56.10)
│
├── Ubuntu Victim (192.168.56.20)
│      └── Elastic Agent
│
└── Elastic SIEM (192.168.56.30)
       ├── Elasticsearch
       ├── Kibana
       └── Fleet Server
```

---

# Week 1 Objectives

- Create isolated virtual lab
- Configure networking
- Deploy Elastic Stack
- Configure Fleet Server
- Enroll Elastic Agent
- Verify log ingestion
- Document every issue encountered

---

# Build Steps

## Step 1 – Create Host-Only Network

VirtualBox:

```
File
→ Tools
→ Network Manager
→ Host-only Networks
→ Create
```

Configuration:

```
IP Address: 192.168.56.1
Netmask: 255.255.255.0
DHCP: Disabled
```

---

## Step 2 – Create VMs

### Kali Linux

```
RAM: 2GB
Disk: 20GB
IP: 192.168.56.10
```

### Ubuntu Victim

```
RAM: 2GB
Disk: 20GB
IP: 192.168.56.20
```

### Elastic SIEM

```
RAM: 6GB
Disk: 50GB
IP: 192.168.56.30
```

Each VM uses:

- Adapter 1: NAT
- Adapter 2: Host-Only Network

---

## Step 3 – Configure Static IP

Edit:

```bash
sudo nano /etc/netplan/00-installer-config.yaml
```

Apply:

```bash
sudo netplan apply
```

---

## Step 4 – Install Elastic Stack

```bash
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -

echo "deb https://artifacts.elastic.co/packages/8.x/apt stable main" \
| sudo tee /etc/apt/sources.list.d/elastic-8.x.list

sudo apt update

sudo apt install elasticsearch kibana -y
```

---

## Step 5 – Configure Kibana

Edit:

```bash
sudo nano /etc/kibana/kibana.yml
```

Set:

```yaml
server.port: 5601
server.host: "192.168.56.30"
```

Restart:

```bash
sudo systemctl restart kibana
```

---

## Step 6 – Elasticsearch Memory Fix

Problem:

```
Exit Code 137
OOM Kill
```

Fix:

```bash
sudo bash -c 'echo -e "-Xms512m\n-Xmx512m" \
> /etc/elasticsearch/jvm.options.d/heap.options'
```

Increase VM RAM:

```
4GB → 6GB
```

Restart Elasticsearch.

---

## Step 7 – Fleet Server Setup

Fleet Host:

```text
https://192.168.56.30:8220
```

Configure:

```
Kibana
→ Management
→ Fleet
→ Add Fleet Server
```

---

## Step 8 – Configure Elasticsearch Network Binding

Edit:

```bash
sudo nano /etc/elasticsearch/elasticsearch.yml
```

Set:

```yaml
network.host: 192.168.56.30
http.port: 9200
transport.host: 192.168.56.30
```

Restart:

```bash
sudo systemctl restart elasticsearch
```

---

## Step 9 – Install Elastic Agent

On Ubuntu Victim:

```bash
sudo ./elastic-agent install \
--url=https://192.168.56.30:8220 \
--enrollment-token=<TOKEN> \
--insecure
```

---

## Common Issues Encountered

| Issue | Resolution |
|---------|-----------|
| Host-only adapter missing | Enable Adapter 2 in VirtualBox |
| Kibana timeout | Set server.host |
| Elasticsearch OOM | Reduce heap + increase RAM |
| Enrollment token failure | Ensure Elasticsearch is running |
| Wrong Elastic password | Reset password |
| Fleet Server using NAT IP | Configure network.host |
| Fleet installed on wrong VM | Reinstall correctly |
| SSL certificate error | Use --insecure |
| No logs visible | Clear filters and select logs-* |

---

# Validation

Completed successfully:

- [x] Elastic Stack running
- [x] Kibana accessible
- [x] Fleet Server healthy
- [x] Elastic Agent enrolled
- [x] Logs visible in Discover
- [x] Lab networking functional

---

# Week 2 Roadmap

## Credential Stuffing Detection

- Deploy Flask fintech API
- Generate brute-force traffic
- Create Sigma rules
- Validate detections

## Future Scenarios

- Data Exfiltration
- Insider Threat
- API Abuse
- Privilege Escalation
- Lateral Movement

---

# Skills Demonstrated

- Linux Administration
- Elastic Stack
- Detection Engineering
- Log Analysis
- Fleet Management
- Network Troubleshooting
- SIEM Deployment
- Security Lab Design

---

## Status

Week 1 Complete ✅

Next milestone:

Credential Stuffing Detection Engineering Lab
