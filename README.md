# SKT_Dwsim-with-Thingsboard-dashboard
Automated Ventilation Distributed Control System (DCS)
This repository contains the full hardware and software implementation for an automated ventilation control system, developed as a project for the Distributed Control System course at Institut Teknologi Sepuluh Nopember (ITS).

this repository was done by grup 1 in Instrumentation Engineering

The system is designed to automatically manage air quality in a closed environment by monitoring real-time temperature and humidity. It controls a servo-actuated damper and a relay-driven exhaust fan based on configurable setpoints. The core controller is an ESP32-S3 , with firmware written entirely in Rust.

---

Data is federated to two platforms: ThingsBoard for real-time operational monitoring and InfluxDB for historical time-series data logging and analysis.

Key Features

Robust Sensing: Uses an industrial SHT20 temperature/humidity sensor communicating via the Modbus RTU (RS485) protocol for high noise immunity.




Dual Actuator Control:


Exhaust Fan: Controlled by a 5V Relay module via a digital GPIO pin.




Ventilation Damper: Controlled by a servo motor using the ESP32's LEDC (PWM) peripheral.



Modern Firmware: The ESP32-S3 firmware is built in Rust on top of the esp-idf framework, ensuring memory safety and high performance.

Dual-Platform Data Sinks:


ThingsBoard (MQTT): Publishes real-time telemetry (temp, humidity) and actuator status (relay state, servo position) via MQTT for a live operational dashboard.






InfluxDB (HTTP): Directly writes data from the ESP32 to an InfluxDB Cloud bucket using the HTTP v2 API and Line Protocol for long-term storage.






Digital Twin Integration: Includes a Python utility script that bridges data between InfluxDB and the DWSIM chemical process simulator, allowing for comparison between live field data and a simulated model.
Dashboards & Application
1. ThingsBoard (Operational Dashboard)
   
![WhatsApp Image 2025-10-14 at 12 15 30_6cb5223c](https://github.com/user-attachments/assets/fbc3c2df-36ef-43f1-8f94-a93abc230b6b)

This dashboard provides a live, high-level view for an operator. It displays the current temperature and humidity, the fan's On/Off status, and the servo's rotational position as a gauge.

2. InfluxDB (Time-Series Data)
![WhatsApp Image 2025-10-14 at 12 15 31_fa9127b7](https://github.com/user-attachments/assets/ca6a1f9a-cb82-4592-9736-c401cf16657a)

InfluxDB serves as the system's historian. It logs all sensor and actuator data, allowing for trend analysis and historical debugging.
This dashboard shows the raw, real-time data for temperature and humidity being written to the database.

3. Python/DWSIM Bridge (Digital Twin)
![WhatsApp Image 2025-10-14 at 12 15 32_4d6b0852](https://github.com/user-attachments/assets/731a843d-287f-4783-86ed-974753c3dc10)

This Python application validates the system. It reads the latest temperature from InfluxDB, writes it as an input to a DWSIM simulation file, executes the simulation, and then pushes the simulation's four output parameters back to InfluxDB.
The plot compares the live sensor data ("Influx Temperature") with the DWSIM model's outputs ("Cold Water Outlet", "Hot Water Inlet", etc.).

System Architecture

The ESP32-S3 acts as the primary controller.

It uses its UART peripheral and a MAX485 converter to poll the SHT20 sensor over Modbus RTU (RS485).

The Rust firmware parses the Modbus response, validates the CRC, and extracts temperature and humidity values.

Control Logic:

If Temperature ≥ 27.2°C → Relay activated (Fan ON)

If Temperature > 28.5°C → Servo moved to the “open” position

A watchdog function resets the servo to 90° every 20 seconds to prevent position drift.

Data Transmission:

The ESP32 pushes data to two destinations simultaneously:

A JSON payload is sent to ThingsBoard via MQTT.

A Line Protocol string is sent via HTTP POST directly to InfluxDB.

Validation Loop:

A Python utility (running on PC) connects DWSIM and InfluxDB to provide continuous validation.

Project Context

Course: Sistem Kontrol Terdistribusi (Distributed Control System)

Institution: Institut Teknologi Sepuluh Nopember (ITS)

Faculty: Fakultas Vokasi

Department: Departemen Teknik Instrumentasi

Instructor: Ahmad Radhy, S.Si., M.Si.

Authors:

M Shofiyur Rochman (2042231031)

Maulidan Arridlo (2042231059)


