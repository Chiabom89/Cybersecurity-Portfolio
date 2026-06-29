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

