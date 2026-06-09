# Network-Security
# Network Security & Packet Analysis

## Lab 1: DNS Protocol Verification over Local Wireless Gateway
**Objective:** Capture and analyze real-time Domain Name System (DNS) request and response handshakes over a local production gateway.

### Visual Evidence
![Captured DNS Traffic Handshake](./dns_evidence.png)

### Protocol Analysis Summary
During this exercise, the local host client machine (`192.168.1.117`) initiated an outbound web communication string, requiring a domain name resolution query for `www.canva.com`. 

* **Raw Network Capture Log:** [Download Packet Capture (.pcapng)](./dns_traffic_analysis.pcapng)

---

## Lab 2: Cleartext IoT Telemetry Sniffing (MQTT)
**Objective:** Intercept and analyze raw Message Queuing Telemetry Transport (MQTT) packet streams running over a local host system.

### Visual Evidence
![Captured MQTT Telemetry Data](./mqtt_leak_evidence.png)

### Security Risk Assessment
During host interface monitoring on the loopback adapter, core system health frames were published under the administrative `$SYS/broker/uptime` branch. 

Because the underlying pipeline lacks cryptographic transport protection (TLS), all internal device statistics and uptime metrics are fully exposed in plain text. In a production industrial setting, an adversary could exploit this visibility to map out infrastructure runtime windows and target specific service disruption exploits.
