# Overview

Distributed IoT system for elevator condition monitoring using two ESP32 IoT boards as nodes. Detects anomalies at the edge, classifies severity in the cloud, and visualizes in real-time web dashboard. KTH IK1332 project
Problem solved: Resource-constrained embedded ML + reliable edge-cloud pipeline for safety-critical elevator data.

<img width="1640" height="1258" alt="image" src="https://github.com/user-attachments/assets/f61b83fe-04cf-4651-89d4-0274d8aeca2b" />

# Architecture

```
Sensing Node (ESP32) ──BLE──> Gateway Node (ESP32) ──WiFi──> Firebase RTDB
  │ Sensors + Edge ML                │ Queue/ACK buffer                  │
  │ Anomaly deviation ──────────────> │ Forwards JSON ───────────────────> │ onAnomalyCreated (severity)
  └──────────────────────────────────┘                                  │ Updates baselines / Writes /cloud
                                                                        │
Web Interface (React/TS) ──RTDB Listeners ──────────────────────────────> Enriched data + real-time updates
```
# Features

- Edge anomaly detection (Gaussian models, online adaptation).

- Cloud severity classification ("info"/"warning"/"critical" + risk score 0-100).

- BLE client-server with queues (loss mitigation).

- Floor detection via pressure + K-means.

- Real-time dashboard (travel log, status, anomaly history).

- Adaptive ML baselines per elevator/type.

Tech Stack

```
| Layer   | Tech                                                  |
| ------- | ----------------------------------------------------- |
| Sensing | ESP32-S3, BMP581 pressure, ICM-20948 IMU, Arduino/C++ |
| Comms   | BLE (client-server), WiFi                             |
| Cloud   | Firebase Realtime DB + Cloud Functions (Node 22/TS)   |
| Web     | React + TypeScript + Vite                             |
| ML      | Edge: statistical models; Cloud: adaptive thresholds  |
```

# Quick Start

## Prerequisites

- Arduino IDE + ESP32 board package.

- Firebase project (Realtime DB + Functions).

- Node.js 22 (for cloud/web).

# Repositories
```
IK1332-Group-3/
├── cloud-functions/     # Firebase Functions (API + severity ML)
├── gateway-node/        # ESP32 BLE server + Firebase writer
├── sensing-node/        # ESP32 sensors + edge ML + BLE client
├── web-interface/       # React dashboard
├── data-collection/     # Training data/scripts
```

# Installation

## To clone any repo:

```
git clone https://github.com/Elevator-IoT-monitoring/[REPO NAME]
```
1. Sensing node
   ```
      cd sensing-node
      # Flash via Arduino IDE (see repo README)
      # Sensors: BMP581/IMU via I²C
   ```

2. Gateway Node
    ```
    cd gateway-node
    # Flash via Arduino IDE
    # Configure WiFi + Firebase credentials
    ```
3. Cloud Functions
    ```
    cd cloud-functions
    npm install
    npm run emu  # Test locally
    npm run deploy
    ```
4. Web Interface
   ```
   cd web-interface
   npm install
   npm run dev  # http://localhost:5173
   ```
**Full pipeline test** : Sensor → BLE → Gateway → Firebase → Cloud ML → Web (live).

  
