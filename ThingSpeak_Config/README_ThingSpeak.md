# ThingSpeak Cloud Configuration

## ECOGUARD-IoT V7

This document describes the cloud telemetry configuration implemented in the ECOGUARD-IoT autonomous environmental monitoring system using the ThingSpeak IoT platform.

---

# Channel Information

| Parameter              | Value                         |
| ---------------------- | ----------------------------- |
| Platform               | ThingSpeak                    |
| Channel Name           | Sistema Robotico Automatizado |
| Channel ID             | 3347447                       |
| Access Type            | Public                        |
| Communication Protocol | HTTP REST                     |
| Embedded Controller    | ESP32-WROOM Dual Core         |

---

# Channel Description

Environmental monitoring and robotic telemetry system based on ESP32 for real-time acquisition of temperature, humidity, air quality, obstacle distance, operational modes, and intelligent alerts.

---

# Configured Fields

| Field   | Variable             | Sensor / Function            |
| ------- | -------------------- | ---------------------------- |
| Field 1 | temperatura DHT22    | Temperature monitoring       |
| Field 2 | humedad DHT22        | Relative humidity monitoring |
| Field 3 | MQ135                | Air quality / gas monitoring |
| Field 4 | distancia ultrasónic | HC-SR04 obstacle detection   |
| Field 5 | modo del robot       | Manual / Autonomous mode     |
| Field 6 | ALERTA DEL ROBOT     | Intelligent alert system     |

---

# Implemented IoT Functions

The ThingSpeak platform was configured to provide:

* Real-time telemetry monitoring
* Historical environmental data storage
* Dynamic chart visualization
* Remote cloud supervision
* Environmental variable analysis
* Wireless IoT communication using ESP32 WiFi

---

# Data Acquisition Rate

| Parameter                 | Value         |
| ------------------------- | ------------- |
| Telemetry Update Interval | 15 seconds    |
| Communication Type        | WiFi 2.4 GHz  |
| Cloud Protocol            | HTTP REST API |

---

# Communication Example

```http
GET https://api.thingspeak.com/update?api_key=YOUR_API_KEY&field1=30
```

---

# Environmental Variables

| Variable    | Unit |
| ----------- | ---- |
| Temperature | °C   |
| Humidity    | %    |
| Air Quality | PPM  |
| Distance    | cm   |

---

# Dashboard Features

The implemented dashboard includes:

* Temperature charts
* Humidity charts
* Air quality monitoring
* Obstacle distance visualization
* Historical telemetry
* Remote environmental supervision

---

# Security Recommendations

For security purposes, private API keys and WiFi credentials are not included in this repository.

Replace all sensitive information using:

```cpp
YOUR_API_KEY
YOUR_WIFI_SSID
YOUR_WIFI_PASSWORD
```

---

# Integrated Technologies

* ESP32-WROOM
* ThingSpeak IoT
* WiFi Communication
* HTTP REST API
* Environmental Sensors
* Flutter Mobile Application
* Embedded Systems

---

# Author

**Reibeiro Velásquez Flórez**
UNAD – Telecommunications Engineering
ECOGUARD-IoT Project
