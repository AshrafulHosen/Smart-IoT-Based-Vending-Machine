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
- [⚡ Power Connection](#-power-connection)
- [📡 MQTT Communication](#-mqtt-communication)
- [🔄 How the System Works](#-how-the-system-works)
- [🧠 Dispensing Logic](#-dispensing-logic)
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

## 🔌 Hardware Components

- **ESP32 Microcontroller**: Serves as the main control unit, managing Wi-Fi connectivity, MQTT processing, motor operations, and IR sensor inputs.
- **TB6612FNG Motor Driver**: Interface module driving DC motors with directional and PWM speed control.
- **N20 12V Geared Motors (x2)**: High-torque miniature DC motors designed to rotate vending spirals.
- **IR Obstacle Avoidance Sensors (x2)**: Positioned at dispensing chutes to detect falling items via beam interruptions.
- **12V Power Supply**: External power source dedicated to motor operation.

---

## 📍 Pin Configuration

| ESP32 Pin | Connected Component | Function |
|---|---|---|
| **GPIO 18** | TB6612FNG `AIN1` | Motor A Direction Control 1 |
| **GPIO 19** | TB6612FNG `AIN2` | Motor A Direction Control 2 |
| **GPIO 5** | TB6612FNG `PWMA` | Motor A Speed Control (PWM) |
| **GPIO 16** | TB6612FNG `BIN1` | Motor B Direction Control 1 |
| **GPIO 17** | TB6612FNG `BIN2` | Motor B Direction Control 2 |
| **GPIO 23** | TB6612FNG `PWMB` | Motor B Speed Control (PWM) |
| **GPIO 21** | TB6612FNG `STBY` | Motor Driver Enable (Standby Control) |
| **GPIO 4** | IR Sensor 1 `OUT` | Product A Delivery Detection Signal |
| **GPIO 27** | IR Sensor 2 `OUT` | Product B Delivery Detection Signal |
| **5V (USB)** | Driver `VCC`, IR `VCC` | Logic Supply (5V) |
| **GND** | System Ground | Common Ground Reference |

---

## ⚡ Power Connection

- **ESP32 Logic**: Powered via 5V USB cable or a regulated 5V source[cite: 1].
- **Motor Power**: Driven by a separate **12V DC power supply** connected to the `VM` terminal of the TB6612FNG driver.
- **Common Ground**: All `GND` lines (ESP32, TB6612FNG, IR Sensors, and 12V supply) are tied together to establish a shared signal reference.

---

## 📡 MQTT Communication

The machine uses **HiveMQ Cloud** as the central broker. Data payload formats rely on structured JSON.

### MQTT Topics
- **Command Topic**: Receives product order JSON messages from the mobile application.
- **Status Topic**: Publishes system events, inventory counts, and dispensing outcomes back to the app.

### Example Payload
```json
{
  "command": "dispense",
  "a": 2,
  "b": 1
}
```

---

## 🔄 How the System Works

1. **Order Dispatch**: The user places an order on the Android app, sending an MQTT message containing product counts via HiveMQ Cloud.
2. **Order Processing**: The ESP32 parses the incoming JSON, verifies current inventory stock, and queues requested items.
3. **Motor Activation**: The ESP32 triggers the appropriate motor through the TB6612FNG motor driver.
4. **IR Sensor Detection**: As the item falls down the chute, it breaks the IR sensor beam, toggling its pin state from `HIGH` to `LOW`.
5. **Completion & Updates**: The ESP32 immediately halts the motor upon detection, decrements internal inventory, and publishes updated status data over MQTT.

---

## 🧠 Dispensing Logic

- **Queueing System**: Multi-item requests are broken down into sequential steps (e.g., `[Product A, Product A, Product B]`).
- **IR Sensing Interruption**: Motors run continuous rotation until `digitalRead(IR_PIN) == LOW` is met.
- **Timeout Protection**: Includes failure detection; if an item fails to interrupt the IR sensor within a specified period, the motor turns off automatically to avoid hardware burnout or jammed operations.

---

## 💻 Software Stack

- **Firmware Framework**: Arduino IDE / ESP32 Core
- **Libraries**:
  - `WiFi.h`: Handles Wi-Fi network initialization.
  - `PubSubClient.h`: Manages MQTT broker client connections and subscriptions.
  - `ArduinoJson.h`: Parses JSON order payloads and serializes status messages.
- **Cloud Infrastructure**: HiveMQ Cloud Broker
- **Mobile Application**: Native Android Application

---

## 📂 Repository Structure

```text
Smart-IoT-Based-Vending-Machine/
├── firmware/
│   └── vending_machine.ino                                    # ESP32 source code
├── Documentation/
│   └── Smart_IoT_Vending_Machine_Documentation.pdf            # Full project report                     
└── README.md                                                  # Project documentation overview
```

---

## 🚀 Installation & Setup

1. **Hardware Assembly**: Wire components according to the [Pin Configuration](#-pin-configuration) table.
2. **Setup Arduino IDE**:
   - Install ESP32 board support packages.
   - Install dependent libraries: `PubSubClient` and `ArduinoJson`.
3. **Firmware Configuration**: Open the firmware project file and update network parameters:
   ```cpp
   #define WIFI_SSID "YOUR_WIFI_NAME"
   #define WIFI_PASSWORD "YOUR_WIFI_PASSWORD"
   #define MQTT_BROKER "YOUR_HIVEMQ_HOST"
   #define MQTT_USER "YOUR_MQTT_USERNAME"
   #define MQTT_PASS "YOUR_MQTT_PASSWORD"
   ```
4. **Upload Firmware**: Flash the code to your ESP32 board over USB.
5. **Android App Connection**: Configure the Android app to target your HiveMQ credentials and command topics.

---

## 🔐 Security

- Supports encrypted **MQTT over TLS** connections on port 8883.
- Utilizes HiveMQ Cloud user authentication credentials (`Username` / `Password`) for authorized network interaction.

---

## 🛠️ Troubleshooting

- **Motor Fails to Turn**: Check external 12V DC power availability and confirm that `STBY` (GPIO 21) is driven HIGH.
- **IR Detection Fails**: Ensure direct optical alignment between IR LEDs and photodiode receivers. Adjust onboard potentiometer sensitivity if necessary.
- **MQTT Connectivity Issues**: Verify Wi-Fi networks have active Internet connections and double-check HiveMQ host credentials.

---

## 🌟 Advantages

- Eliminates cash/coin processing hardware costs and manual emptying needs.
- Remote inventory tracking and real-time operational feedback.
- High scalability; system layout easily expands to support more slots.
- Reliable dispensing feedback through IR sensors prevents over-dispensing.

---

## 🔮 Future Improvements

- Integration of mobile payment gateways (e.g., bKash, Nagad, Rocket).
- Dynamic QR code creation directly on display screens.
- Expanded cloud database record management using Firebase.
- Dedicated web portal dashboards for remote inventory control.

---

## 📄 Documentation

A complete technical project documentation report is available in the [`Documentation/`](./Documentation/) directory.

---

## 👨‍💻 Project Information

Developed by students of the **Department of Computer Science & Engineering** at **Khulna University of Engineering & Technology (KUET)**:
|*Name*|*Roll*|
|---|---|
|Ashraful Hosen|2207042|
|Amdadul Haque|2207059|

**Repository Link**: [https://github.com/AshrafulHosen/Smart-IoT-Based-Vending-Machine](https://github.com/AshrafulHosen/Smart-IoT-Based-Vending-Machine)

---
