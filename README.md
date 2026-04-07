# Wearable-health-monitors-for-the-elderly

## Overview

This project is a wearable IoT-based health monitoring system designed for elderly care. It continuously monitors vital parameters such as heart rate and body temperature, and detects fall and faint conditions in real time.

When an abnormal condition is detected, the system triggers an alert using a buzzer and updates a web dashboard that can be accessed from a mobile phone or laptop.


## Problem Statement

Elderly individuals living alone are at high risk during medical emergencies such as falls, fainting, or abnormal vital conditions. Immediate assistance is often unavailable.



## Solution

A low-cost wearable system that monitors health parameters and detects emergencies, providing real-time alerts and a local web dashboard for continuous monitoring.



## Objectives

- Monitor heart rate and body temperature  
- Detect fall and faint conditions  
- Provide real-time alerts using buzzer  
- Display live data on a web dashboard  
- Enable monitoring without internet  



## Components Used

- ESP32 (Sensor and Processing Unit)  
- ESP8266 NodeMCU (Web Server)  
- MAX30102 (Heart Rate Sensor)  
- DS18B20 (Temperature Sensor)  
- MPU6050 (Accelerometer for fall detection)  
- OLED Display (SSD1306)  
- Buzzer  
- Push Button (Manual Emergency Trigger)  
- Battery Power Supply  



## System Architecture

The system consists of two main modules:

1. ESP32 (Sensor Unit)  
   - Reads sensor data  
   - Processes conditions  
   - Sends data via WiFi  

2. ESP8266 (Web Server)  
   - Receives data  
   - Hosts dashboard  
   - Displays real-time information  

Data Flow:

ESP32 → WiFi → ESP8266 → Web Dashboard → User Device  

- System Architecture: [docs/working.md]


## Working

- Sensors collect real-time data  
- ESP32 processes and checks conditions  
- Emergency triggers if abnormal condition is detected  
- Buzzer alert is activated  
- Data is sent to ESP8266 via WiFi  
- ESP8266 updates the dashboard  
- User monitors through browser  

Detailed working: [docs/detailworking.md]



## Web Dashboard

The ESP8266 creates a local WiFi network:

SSID: HealthMonitor  
Password: 12345678  

Steps to access:

1. Connect mobile or laptop to WiFi  
2. Open browser  
3. Enter: http://192.168.4.1  

The dashboard displays:

- Heart rate  
- Temperature  
- Fall detection status  
- Faint detection status  
- Emergency alert status  



## Hardware Connections

Detailed pin connections:  
[docs/connections.md](docs/connections.md)



## Project Preview





## Results

<img width="1600" height="718" alt="image" src="https://github.com/user-attachments/assets/7ea5351b-a572-40d2-a2d2-c5edb3504c5d" />




## Features

- Real-time health monitoring  
- Fall detection using accelerometer  
- Faint detection using heart rate data  
- Manual emergency trigger  
- Buzzer alert system  
- Local web dashboard without internet  
- Portable and low-cost design  



## Observations

- Accurate readings require proper sensor placement  
- System works without internet using local WiFi  
- Alerts trigger correctly during abnormal conditions  



## Future Improvements

- GPS tracking integration  
- Mobile application development  
- Cloud data storage  
- AI-based health prediction  



## Documentation

- Hardware Connections: docs/connections.md  
- System Working: docs/working.md  
- System Architecture: docs/architecture.md  

---

## Author

Uday Kumar  
Electronics and Communication Engineering Student
