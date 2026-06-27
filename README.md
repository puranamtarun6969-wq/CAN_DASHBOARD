# 🚗 CAN Dashboard System Using LPC2129


# 📑 Table of Contents

* Abstract
* Objectives
* System Overview
* System Architecture
* Hardware Requirements
* Software Requirements
* Why CAN Protocol?
* Node Descriptions
* CAN Communication
* Interrupt Handling
* LCD Output
* Project Flow
* Features
* Learning Outcomes
* Future Enhancements
* Author

---

# 📖 Abstract

Modern automobiles contain multiple Electronic Control Units (ECUs) that communicate over a **Controller Area Network (CAN)** bus. CAN enables reliable, high-speed communication between vehicle subsystems while significantly reducing wiring complexity.

This project implements a simplified automotive dashboard using **three LPC2129 ARM7 microcontrollers** connected through a CAN network.

The dashboard continuously monitors:

* 🌡 Engine Temperature
* ⛽ Fuel Level
* 🚦 Indicator Status

The collected information is exchanged over the CAN bus and displayed on an LCD dashboard. Indicator control is implemented using external interrupts and LED animations.

This project demonstrates real-time embedded communication, distributed control systems, sensor interfacing, interrupt handling, and CAN protocol implementation.

---

# 🎯 Objectives

* Understand CAN Bus communication.
* Implement distributed node architecture.
* Monitor fuel level using ADC.
* Monitor engine temperature using DS18B20.
* Display vehicle information on LCD.
* Control vehicle indicators using external interrupts.
* Exchange real-time data between multiple nodes.

---

# 🚘 System Overview

The system consists of three independent nodes:

* 🖥 Main Dashboard Node
* ⛽ Fuel Monitoring Node
* 🚦 Indicator Control Node

Each node performs a dedicated task while communicating through the CAN network.

---

# 🏗 System Architecture

```text
                           CAN BUS
--------------------------------------------------------------------------------

                    +----------------------+
                    |  MAIN DASHBOARD NODE |
                    |      LPC2129         |
                    +----------+-----------+
                               |
                +--------------+--------------+
                |                             |
                |                             |
                V                             V

     +------------------+         +------------------+
     |    FUEL NODE     |         | INDICATOR NODE   |
     |     LPC2129      |         |    LPC2129       |
     +------------------+         +------------------+

--------------------------------------------------------------------------------
```

---

# 🔧 Hardware Requirements

| Component                    | Quantity |
| ---------------------------- | -------- |
| LPC2129 ARM7 Microcontroller | 3        |
| MCP2551 CAN Transceiver      | 3        |
| DS18B20 Temperature Sensor   | 1        |
| Potentiometer (Fuel Sensor)  | 1        |
| 16×2 / 20×4 LCD              | 1        |
| LEDs                         | 8        |
| Push Buttons                 | 2        |
| Crystal Oscillator           | 3        |
| Power Supply                 | 1        |

---

# 💻 Software Requirements

* Keil µVision
* Embedded C
* Flash Magic
* Proteus (Optional)
* Git

---

# 📡 Why CAN Protocol?

CAN is widely used in automotive applications because it provides:

* High Reliability
* Error Detection
* Multi-Master Communication
* Reduced Wiring
* High-Speed Communication
* Low Cost
* Real-Time Performance

---

# 🖥 Node 1 — Main Dashboard Node

## Responsibilities

* Reads engine temperature using DS18B20
* Receives fuel percentage from Fuel Node
* Detects Left and Right switch presses
* Sends indicator commands over CAN
* Displays dashboard information on LCD

### LCD Connections

| LCD Pin | LPC2129 Pin |
| ------- | ----------- |
| D0–D7   | P0.0 – P0.7 |
| RW      | P0.9        |
| EN      | P0.10       |
| RS      | P0.11       |

### Switch Connections

| Switch | Pin           |
| ------ | ------------- |
| Left   | P0.14 (EINT0) |
| Right  | P0.16 (EINT1) |

### Sensor Connection

| Sensor  | Pin   |
| ------- | ----- |
| DS18B20 | P0.15 |

### CAN Connection

| Signal | Pin   |
| ------ | ----- |
| TXD1   | P0.24 |
| RXD1   | P0.25 |

---

# 🌡 Temperature Monitoring

The DS18B20 continuously measures engine temperature.

### Advantages

* Digital Sensor
* High Accuracy
* One-Wire Communication
* No ADC Required

### Conversion

```c
Temperature = RawValue >> 4;
```

Example:

```
Raw Value = 400

Temperature = 25°C
```

---

# ⛽ Node 2 — Fuel Monitoring Node

## Responsibilities

* Read fuel sensor
* Convert ADC value to percentage
* Send fuel data through CAN

### ADC Specifications

```
Resolution : 10-bit
Range      : 0–1023
Reference  : 3.3V
```

### Fuel Calculation

```c
fuelPercent = (adcValue * 100) / 1023;
```

---

# 🚦 Node 3 — Indicator Control Node

## Responsibilities

* Receive CAN commands
* Control LED indicators
* Generate Left/Right animations

### Left Indicator

```
LED7 → LED6 → LED5 → LED4 → LED3 → LED2 → LED1 → LED0
```

Command:

```
'L'
```

### Right Indicator

```
LED0 → LED1 → LED2 → LED3 → LED4 → LED5 → LED6 → LED7
```

Command:

```
'R'
```

### OFF State

```
'O'
```

---

# 📨 CAN Communication

## Fuel Frame

```
CAN ID : 0x01
DLC    : 4
DATA1  : Fuel Percentage
```

Example

```
ID = 0x01

Fuel = 75%
```

---

## Indicator Frame

```
CAN ID : 0x11
DLC    : 1
```

Commands

```
'L' Left
'R' Right
'O' OFF
```

---

# ⚡ Interrupt Handling

| Interrupt | Pin   | Function        |
| --------- | ----- | --------------- |
| EINT0     | P0.14 | Left Indicator  |
| EINT1     | P0.16 | Right Indicator |

Flow

```text
Button Press
      ↓
External Interrupt
      ↓
Transmit CAN Message
      ↓
Indicator Node
      ↓
LED Animation
```

---

# 📟 LCD Output

```text
--------------------------------
<<< DASH BOARD >>>

Fuel : 75 %

Indicator : < >

Temp : 30 °C
--------------------------------
```

---

# 🔄 Complete Project Flow

```text
START

↓

Initialize LCD

↓

Initialize CAN

↓

Initialize ADC

↓

Initialize DS18B20

↓

Read Sensors

↓

Transmit CAN Data

↓

Receive CAN Data

↓

Update LCD

↓

Repeat Forever
```

---

# ✅ Features

* CAN-Based Communication
* Distributed Node Architecture
* Real-Time Temperature Monitoring
* Real-Time Fuel Monitoring
* LCD Dashboard Interface
* Interrupt-Based Indicator Control
* LED Indicator Animation
* Embedded C Firmware
* Automotive Dashboard Simulation

---

# 🎓 Learning Outcomes

* LPC2129 ARM7 Programming
* CAN Protocol
* Embedded C
* ADC Interfacing
* DS18B20 Interfacing
* LCD Programming
* Interrupt Handling
* Real-Time Embedded Systems
* Automotive Communication Networks

---

# 🔮 Future Enhancements

* Vehicle Speed Display
* RPM Monitoring
* OBD-II Integration
* GPS Tracking
* GSM Alerts
* Touchscreen Dashboard
* SD Card Data Logging
* Bluetooth Connectivity
* Mobile Application Integration

---

# 👨‍💻 Author

**Tarun Puranam**



---

## ⭐ If you found this project useful, consider giving it a star on GitHub!
