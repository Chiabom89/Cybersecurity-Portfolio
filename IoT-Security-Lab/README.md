# IoT Security Lab: Deploying and Validating a Containerized MQTT Broker

## 1. Overview & Architecture
This lab demonstrates how to spin up a secure, isolated Internet of Things (IoT) messaging infrastructure using Docker. The goal was to establish a local Message Queuing Telemetry Transport (MQTT) broker, simulate a live IoT device publishing data, and monitor the stream securely inside a sandboxed environment.

* **Infrastructure Platform:** GitHub Codespaces (Cloud-hosted Linux sandbox)
* **Container Engine:** Docker
* **MQTT Broker:** Eclipse Mosquitto
* **Network Protocol:** TCP Port 1883 (Isolated local interface)

---

## 2. Infrastructure Deployment
The deployment was containerized to ensure complete isolation from the host machine. The broker was pulled and instantiated using the following Docker configuration:

```bash
# Instantiating the containerized Mosquitto Broker
docker run -d --name iot_broker -p 1883:1883 eclipse-mosquitto

docker exec -it iot_broker mosquitto_sub -t telemetry/test

docker exec -it iot_broker mosquitto_pub -t telemetry/test -m '{"device_id": "sensor_01", "status": "secure", "payload": "Lab connection successful!"}'

Connection successful!
{
  "device_id": "sensor_01", 
  "status": "secure", 
  "payload": "Lab connection successful!"
}

---

## 3. Security Hardening & Access Control Lists (ACLs)
To elevate the broker deployment from a default "open" configuration to an enterprise-ready zero-trust architecture, strict authentication and topic isolation boundaries were applied.

### Implementation Blueprint
1. **Cryptographic Credential Generation:** Used the internal container tool `mosquitto_passwd` to create an encrypted, hashed password file mapping a dedicated user:
   ```bash
   docker exec -it iot_broker mosquitto_passwd -b /mosquitto/config/password.txt sensor_admin Stunna@Pass123!

user sensor_admin
topic readwrite telemetry/#

allow_anonymous false
password_file /mosquitto/config/password.txt
acl_file /mosquitto/config/acl.conf

$ docker exec -it iot_broker mosquitto_sub -t telemetry/test
Connection Refused: not authorized

$ docker exec -it iot_broker mosquitto_sub -t telemetry/test -u sensor_admin -P Stunna@Pass123!
# System stands by securely...
secure connection verified!

$ docker exec -it iot_broker mosquitto_pub -t telemetry/test -u sensor_admin -P Stunna@Pass123! -m "secure connection verified!"

---

## 3. Container Security Hardening (ACL Verification)
To replace the vulnerable default configuration, an authenticated access control matrix was engineered directly within the runtime environment.

### Production Audit Logs
The system validation logs below verify that the broker successfully handles zero-trust verification parameters:

* **Terminal 1 (Authenticated Subscriber Connection):**
  ```bash
  $ docker exec -it iot_broker mosquitto_sub -t telemetry/test -u sensor_admin -P Stunna@Pass123!
  secure connection verified!

$ docker exec -it iot_broker mosquitto_pub -t telemetry/test -u sensor_admin -P Stunna@Pass123! -m "secure connection verified!"


