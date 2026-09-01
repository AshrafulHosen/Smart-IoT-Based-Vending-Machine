<div align="center">

# 🚀 Smart IoT-Based Vending Machine

### ESP32 • MQTT • HiveMQ Cloud • Android • IR Sensors • Motor Control

<p>
  <img src="https://img.shields.io/badge/ESP32-Microcontroller-blue?style=for-the-badge&logo=espressif" alt="ESP32">
  <img src="https://img.shields.io/badge/MQTT-Protocol-purple?style=for-the-badge" alt="MQTT">
  <img src="https://img.shields.io/badge/HiveMQ-Cloud-orange?style=for-the-badge" alt="HiveMQ">
  <img src="https://img.shields.io/badge/Arduino-IDE-00979D?style=for-the-badge&logo=arduino" alt="Arduino">
  <img src="https://img.shields.io/badge/IoT-Project-success?style=for-the-badge" alt="IoT">
</p>

<p>
  <img src="https://img.shields.io/github/repo-size/AshrafulHosen/Smart-IoT-Based-Vending-Machine?style=flat-square">
  <img src="https://img.shields.io/github/last-commit/AshrafulHosen/Smart-IoT-Based-Vending-Machine?style=flat-square">
  <img src="https://img.shields.io/github/stars/AshrafulHosen/Smart-IoT-Based-Vending-Machine?style=flat-square">
</p>

### 🌐 A low-cost, Internet-connected vending machine controlled remotely through an Android application using MQTT.

</div>

---

# 📌 Table of Contents

- [📖 Overview](#-overview)
- [🎯 Objectives](#-objectives)
- [✨ Features](#-features)
- [🏗️ System Architecture](#️-system-architecture)
- [🔌 Hardware Components](#-hardware-components)
- [📍 Pin Configuration](#-pin-configuration)
- [🔧 Complete Wiring Diagram](#-complete-wiring-diagram)
- [⚡ Power Connection](#-power-connection)
- [📡 MQTT Communication](#-mqtt-communication)
- [🔄 How the System Works](#-how-the-system-works)
- [🧠 Dispensing Logic](#-dispensing-logic)
- [📨 MQTT Commands](#-mqtt-commands)
- [💻 Software Stack](#-software-stack)
- [📂 Repository Structure](#-repository-structure)
- [🚀 Installation & Setup](#-installation--setup)
- [🔐 Security](#-security)
- [🛠️ Troubleshooting](#️-troubleshooting)
- [🌟 Advantages](#-advantages)
- [🔮 Future Improvements](#-future-improvements)
- [📄 Documentation](#-documentation)
- [👨‍💻 Project Information](#-project-information)

---

# 📖 Overview

The **Smart IoT-Based Vending Machine** is an IoT-enabled vending system built around the **ESP32 microcontroller**.

Unlike traditional vending machines that depend on physical coin mechanisms, this system allows users to select products remotely through an **Android application**.

The order is transmitted through the Internet using the **MQTT protocol**. **HiveMQ Cloud** acts as the MQTT broker between the Android application and the ESP32.

The ESP32 receives the order, verifies product availability, controls the dispensing motors, monitors IR sensors to confirm successful delivery, updates the inventory, and sends machine status back to the application in real time.

### 🔥 Core Concept

```text
Android Application
        │
        │ MQTT Command
        ▼
   HiveMQ Cloud
   MQTT Broker
        │
        │ MQTT
        ▼
      ESP32
        │
        ▼
  Motor Driver
        │
   ┌────┴────┐
   ▼         ▼
Motor 1    Motor 2
   │         │
   ▼         ▼
Product A  Product B
   │         │
   ▼         ▼
IR Sensor  IR Sensor
   │         │
   └────┬────┘
        ▼
      ESP32
        │
        │ MQTT Status/Event
        ▼
   HiveMQ Cloud
        │
        ▼
Android Application
```
---

## 🎯 Objectives

- 🏗️ Design a low-cost and smart IoT-based vending machine.
- 📱 Allow users to select and purchase products through an Android application.
- 🌐 Connect the vending machine to the Internet using ESP32 Wi-Fi.
- 📡 Use MQTT for reliable communication between the Android application and vending machine.
- ⚙️ Automatically control product dispensing using N20 geared motors.
- 🔍 Use IR sensors to detect and confirm successful product dispensing.
- 📦 Automatically update product inventory after successful dispensing.
- 🚨 Detect dispensing failures using sensor feedback and timeout mechanisms.
- 📊 Provide real-time vending machine status and event updates.
- 🔐 Enable secure communication between the ESP32 and MQTT cloud broker.

---

## ✨ Features

| Feature | Description |
|---|---|
| 📱 **Android Application** | Allows users to select products and place orders remotely. |
| 🌐 **IoT Connectivity** | ESP32 connects the vending machine to the Internet through Wi-Fi. |
| 📡 **MQTT Communication** | Enables lightweight and real-time communication between the app and ESP32. |
| ☁️ **HiveMQ Cloud** | Acts as the MQTT broker for communication between the application and vending machine. |
| ⚙️ **Automatic Dispensing** | N20 geared motors automatically dispense selected products. |
| 🔍 **IR Product Detection** | IR sensors confirm whether a product has been successfully dispensed. |
| 📦 **Inventory Management** | Product stock is automatically updated after successful dispensing. |
| 🔄 **Dispensing Queue** | Multiple products can be placed in a queue and dispensed sequentially. |
| 🚨 **Failure Detection** | The system detects dispensing failures using IR feedback and timeout control. |
| 📊 **Real-Time Status** | Machine status and dispensing events are published through MQTT. |
| 🔐 **Secure Communication** | MQTT communication can use TLS for secure cloud connectivity. |
| 🧩 **Expandable Design** | The system can be extended with additional product slots and motors. |

---

## 🏗️ System Architecture

The system consists of an **Android application**, **HiveMQ Cloud MQTT broker**, **ESP32 controller**, **TB6612FNG motor driver**, **N20 geared motors**, and **IR sensors**.

### 🔄 Overall Architecture

```text
                         👤 USER
                           │
                           ▼
                  ┌─────────────────┐
                  │  📱 Android App │
                  │                 │
                  │ Product Select  │
                  │ Order / Status  │
                  └────────┬────────┘
                           │
                           │ MQTT Command
                           ▼
                  ┌─────────────────┐
                  │ ☁️ HiveMQ Cloud │
                  │  MQTT Broker    │
                  └────────┬────────┘
                           │
                           │ MQTT
                           ▼
                  ┌─────────────────┐
                  │    🧠 ESP32     │
                  │                 │
                  │ Wi-Fi + Control │
                  │ Order Processing│
                  └───────┬─────────┘
                          │
             ┌────────────┴────────────┐
             │                         │
             ▼                         ▼
    ┌─────────────────┐       ┌─────────────────┐
    │ ⚙️ TB6612FNG    │       │ 🔍 IR Sensors   │
    │  Motor Driver   │       │                 │
    └────────┬────────┘       │ IR 1 → Product A│
             │                │ IR 2 → Product B│
       ┌─────┴─────┐          └────────┬────────┘
       │           │                   │
       ▼           ▼                   │
  ┌─────────┐ ┌─────────┐              │
  │ Motor 1 │ │ Motor 2 │              │
  │ Product │ │ Product │              │
  │    A    │ │    B    │              │
  └────┬────┘ └────┬────┘              │
       │           │                   │
       ▼           ▼                   │
  Product A   Product B                │
       │           │                   │
       └─────┬─────┘                   │
             │                         │
             └──────────┬──────────────┘
                        │
                        ▼
                      ESP32
                        │
                        │ Status / Events
                        ▼
                 ☁️ HiveMQ Cloud
                        │
                        ▼
                  📱 Android App
```

---

