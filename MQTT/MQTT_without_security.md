# MQTT Protocol with Adrastea-I

## Introduction
The Adrastea-I module supports the MQTT protocol for lightweight messaging between devices. MQTT is ideal for scenarios where bandwidth is limited or connectivity is intermittent.

### Key Features
- **Protocol**: MQTT
- **Transport**: TCP
- **Security**: No security (for this setup)
- **Broker**: [HiveMQ Public Broker](http://www.hivemq.com)

## Setup Instructions

### 1. Enabling MQTT Event Notification
To receive MQTT event notifications from the Adrastea-I module, use the following command:

```bash
AT%MQTTEV="all",1

