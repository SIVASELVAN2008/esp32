# ESP32 IR-Triggered Motor Control System

![Hardware](https://img.shields.io/badge/Hardware-ESP32-blue)
![Language](https://img.shields.io/badge/Language-MicroPython-yellow)
![Status](https://img.shields.io/badge/Status-Completed-success)

## 📌 Project Overview
This repository contains the MicroPython script and hardware architecture for an automated, closed-loop motor control system. Built around an ESP32 microcontroller, the system reads real-time digital inputs from an Infrared (IR) proximity sensor to actuate and control a 100 RPM DC gear motor via an L298N motor driver. 

This project demonstrates core embedded systems principles, including GPIO polling, power isolation, and hardware-software integration using MicroPython.

## 🛠️ Hardware Specifications
* **Microcontroller:** ESP32 Development Board
* **Motor Driver:** L298N Dual H-Bridge Motor Driver
* **Actuator:** 100 RPM DC Gear Motor
* **Sensor:** IR Proximity Sensor (Digital Output)
* **Power Source:** 7.4V DC Lithium-Ion (Li-ion) Battery Pack

## ⚡ System Architecture & Wiring

To ensure stable logic operation and prevent micro-controller brownouts, power routing is handled strategically:

1. **Power Distribution:** 
   * The **7.4V Li-ion battery** is connected directly to the 12V input terminal of the L298N motor driver to handle the high-current draw of the motor.
   * The **ESP32** and **IR Sensor** are powered using the onboard 5V regulator output from the L298N (with a shared common ground across all components).
2. **Logic & Control:**
   * **IR Sensor OUT** ➔ ESP32 GPIO [Insert Pin, e.g., GPIO 34]
   * **ESP32 GPIO [Insert Pins, e.g., 26 & 27]** ➔ L298N IN1 & IN2 (Motor Direction/State Control)
   * **L298N OUT1 & OUT2** ➔ 100 RPM DC Motor Terminals

## 💻 Firmware Logic
The control logic is written in MicroPython. 
* Initializes `machine.Pin` objects for input (sensor) and output (motor driver).
* Continuously polls the IR sensor state using a `while` loop.
* Actuates the 100 RPM motor when an obstacle is detected within the sensor's threshold.
* Implements basic software delays (using `time.sleep`) to prevent erratic motor toggling on edge-case sensor readings.

## 🔧 Engineering Challenges & Troubleshooting
**Issue:** High current draw from the DC motor during startup caused voltage dips, initially leading to ESP32 resets and instability.
**Solution:** Implemented power isolation. By routing the raw 7.4V from the Li-ion pack exclusively to the L298N driver and utilizing the driver's voltage regulator to supply a steady 5V to the ESP32, the logic circuit was successfully isolated from the actuator's electrical noise and voltage drops.

## 📂 Repository Structure
```text
├── main.py                # Main MicroPython control script
├── schematics/
│   └── circuit_diagram.png # Wiring and pinout diagram
└── README.md
