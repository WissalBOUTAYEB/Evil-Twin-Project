# Evil Twin Attack Simulation & Monitoring with ELK

> Hands‑on simulation of an Evil Twin (rogue Wi‑Fi AP) attack with real‑time monitoring and visualization using the ELK stack. This repository documents setup, workflow, artifacts, dashboards and ethical constraints for a controlled lab exercise.

---

## Overview

This project demonstrates how an attacker can deploy a rogue Wi‑Fi access point (Evil Twin), present a fake captive portal to harvest credentials, and perform Man‑in‑the‑Middle interception of traffic. All captured artifacts (credentials, network traffic, handshakes, logs) are collected and analyzed in near real‑time using the ELK Stack (Filebeat → Logstash → Elasticsearch → Kibana).

> **Important:** This project is intended for controlled, ethical, and legal lab environments only. Do NOT deploy against networks, devices, or people without explicit authorization.

---

## Objectives

* Create a malicious Wi‑Fi access point that mimics a legitimate SSID.
* Serve a fake captive portal to harvest user credentials for demonstration/education.
* Intercept and capture network traffic (including Wi‑Fi handshakes) using MITM techniques.
* Ingest, parse, index and visualize intercepted data in ELK for monitoring and analysis.

---

## Workflow

1. **Rogue AP setup**

   * Configure `hostapd` to advertise the chosen SSID and manage wireless settings.
   * Configure `dnsmasq` to provide DHCP leases and spoof DNS responses so clients are redirected to the captive portal.

2. **Phishing/captive portal deployment**

   * Host a simple login page (Apache + PHP) that mimics the target portal. Log form submissions to `creds.txt` and structured JSON (`capture.json`) for ingestion.

3. **MITM & packet capture**

   * Use `Bettercap` or `Ettercap` to perform ARP spoofing and active interception of client traffic.
   * Capture network traffic and WPA/WPA2 handshakes into `.pcap` files for offline forensic analysis.

4. **Ingest & parse**

   * Ship logs and artifacts to `Filebeat` / `Logstash`.
   * Parse captured artifacts (credentials, JSON capture files, pcap metadata) and enrich them with fields suitable for indexing.

5. **Store & visualize**

   * Index events in `Elasticsearch`.
   * Create Kibana dashboards to visualize credential submissions, suspicious connections, top talkers, ARP anomalies, handshake captures and timelines.

6. **Analysis & reporting**

   * Use Kibana dashboards to investigate incident timelines, extract indicators of compromise and produce mitigation recommendations.

---

## Tools & Technologies

* **Rogue AP & network services:** `hostapd`, `dnsmasq`
* **Web/portal hosting:** `Apache`, `PHP` (or any lightweight web server)
* **MITM / sniffing / injection:** `Bettercap`, `Ettercap`, `tcpdump`, `aircrack-ng` (for handshake cracking where allowed)
* **ELK pipeline:** `Filebeat`, `Logstash`, `Elasticsearch`, `Kibana`
* **Artifacts / storage:** `creds.txt`, `capture.json`, `*.pcap`
* **OS / environment:** Kali / Debian based lab VM(s) or isolated test network

---

## Key Results

* Successfully simulated a realistic Evil Twin attack in a controlled lab.
* Captured user‑submitted credentials through a fake captive portal.
* Intercepted and saved network traffic and Wi‑Fi handshakes for offline analysis.
* Implemented an ELK pipeline that ingests and visualizes attack artifacts in near real‑time.
* Built Kibana dashboards to detect suspicious activity (credential submissions, ARP anomalies, unusual flows).


## Installation & Quick Start (Lab)

1. Provision two isolated VMs or network namespaces: one attacker machine (with wifi adapter supporting AP mode) and one victim/test client.
2. Install required tooling on attacker VM (`hostapd`, `dnsmasq`, `apache2`, `php`, `bettercap`/`ettercap`, `tcpdump`, `filebeat`, `logstash`, `elasticsearch`, `kibana`).
3. Configure `hostapd` + `dnsmasq` and start the rogue AP.
4. Deploy the captive portal and ensure `creds.txt` / `capture.json` receive form submissions.
5. Run `bettercap`/`ettercap` to launch ARP spoofing and packet capture.
6. Start Filebeat/Logstash to push logs to Elasticsearch and open Kibana to view dashboards.



## Future Improvements

* Add Kibana alerts / Watcher or integrate with a SIEM to trigger notifications on suspicious events.
* Enrich logs with threat intelligence (IP reputation, geolocation).
* Implement automated playbooks for containment and remediation.
* Test scale by simulating multiple rogue APs and concurrent clients.
* Explore ML/behavioral anomaly detection for stealthier rogue AP detection.

