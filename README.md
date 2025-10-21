# SKT_Dwsim-with-Thingsboard-dashboard
Automated Ventilation Distributed Control System (DCS)
This repository contains the full hardware and software implementation for an automated ventilation control system, developed as a project for the Distributed Control System course at Institut Teknologi Sepuluh Nopember (ITS).

this repository was done by grup 1 in Instrumentation Engineering

The system is designed to automatically manage air quality in a closed environment by monitoring real-time temperature and humidity. It controls a servo-actuated damper and a relay-driven exhaust fan based on configurable setpoints. The core controller is an ESP32-S3 , with firmware written entirely in Rust.



Data is federated to two platforms: ThingsBoard for real-time operational monitoring and InfluxDB for historical time-series data logging and analysis.

![WhatsApp Image 2025-10-14 at 12 15 30_6cb5223c](https://github.com/user-attachments/assets/fbc3c2df-36ef-43f1-8f94-a93abc230b6b)
