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



## Lab Scenario: Network Traffic Dissection & Payload Inspection

### 1. Incident Overview & Triage Baseline
A network traffic capture (`iot_traffic_anomaly.pcap`) was pulled from the internal medical logistics network segment to investigate unauthorized data transmissions between deployed IoT endpoints and the central gateway interface.

- **Analysis Tooling:** Command-Line `tshark` Protocol Engine
- **Target Network Capture File:** `iot_traffic_anomaly.pcap`
- **Identified Endpoints:** 
  * `10.0.0.20` (Standard IoT Telemetry Client)
  * `10.0.0.99` (Anomalous Malicious Actor Node)

### 2. Deep Packet Dissection Matrix
By running raw hexadecimal and ASCII packet layout extractions via `tshark -r iot_traffic_anomaly.pcap -x`, cleartext network payloads were uncovered moving across unencrypted TCP channels:

* **Packet 1 (`10.0.0.20 -> 10.0.0.1`):** Transmitting routine sensor telemetry data: `testC22.5`.
* **Packet 2 (`10.0.0.99 -> 10.0.0.1`):** Injecting an unauthorized administrative command payload: `testCMD:SHUTDOWN_COOL`.

### 3. Engineering Countermeasures & Mitigation Strategy
* **Enforce Transport Layer Security (TLS):** Transition all raw unencrypted TCP socket pathways to encrypted application profiles (such as MQTT over TLS) to negate packet sniffing and injection vulnerabilities.
* **Network Segmentation Controls:** Deploy explicit Access Control Lists (ACLs) to structurally isolate edge nodes, completely preventing unauthorized internal node-to-node visibility or malicious command injection probes.
