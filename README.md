# IoT-Based Predictive Maintenance System

An end-to-end IoT-based predictive maintenance system for detecting and classifying bearing faults using real-time vibration data.

## Overview

The system collects machine telemetry from an ESP32 connected to vibration, temperature, and current sensors. The data is securely transmitted to Microsoft Azure, processed through a machine learning inference pipeline, and visualized through a React monitoring dashboard.

**ESP32 → MQTT → Azure IoT Hub → Azure Functions → Azure Blob Storage → FastAPI → ML Models → React Dashboard**

## Machine Learning

The system uses a two-stage machine learning approach:

* **LSTM Autoencoder** for anomaly detection based on vibration signals.
* **1D CNN** for classifying detected faults into:

  * Inner Race Fault
  * Outer Race Fault
  * Rolling Element Fault

A K-consecutive-anomaly mechanism is used to confirm faults and reduce false alarms.

## Real-Time Pipeline

The implemented project includes:

* ESP32-based sensor data acquisition
* Secure MQTT communication
* Azure IoT Hub ingestion
* Event Hub-triggered Azure Function
* Azure Blob Storage for telemetry and model artifacts
* FastAPI inference service
* Dockerized deployment on Azure Container Apps
* React + Vite monitoring dashboard
* Real-time telemetry and prediction visualization

The dashboard displays machine status, health score, sensor readings, reconstruction error, anomaly threshold, fault type, severity, and fault detection timestamps.

## MODEL.ipynb

`MODEL.ipynb` focuses specifically on the machine learning model and its evaluation across three categories:

### 1. Own Dataset

The model is tested on the custom dataset collected from the physical test rig under normal and faulty conditions. The reconstruction error remains below the threshold during normal operation and rises when faulty data is introduced.

The detected fault patterns are evaluated for inner race, outer race, and rolling element faults. Since the custom dataset is not a true run-to-failure dataset, **Dynamic Time Warping (DTW)** is used to align the detected fault patterns with the NASA IMS run-to-failure timeline to estimate their corresponding position relative to failure.

### 2. NASA IMS Dataset

The same model is evaluated on the NASA IMS run-to-failure dataset to validate its behavior on real degradation data with known failure points. The evaluation includes inner race, rolling element, outer race, and healthy bearing conditions, with fault confirmation measured relative to the actual failure point.

### 3. Real-Time Validation

The model is finally tested using live vibration data from the ESP32 and physical test rig. The complete real-time pipeline processes the streamed data, performs anomaly detection using the LSTM Autoencoder, and classifies confirmed faults using the CNN.

## Tech Stack

**Hardware:** ESP32, ADXL345, MAX6675, SCT-013

**Cloud:** Microsoft Azure IoT Hub, Azure Functions, Azure Blob Storage, Azure Container Apps, Azure Container Registry

**Backend:** FastAPI, Python, Docker

**ML:** TensorFlow/Keras, PyTorch, Scikit-learn, NumPy, SciPy, Pandas

**Frontend:** React, Vite

**Communication:** MQTT over TLS

## Where to Start

* **To understand the machine learning model and its evaluation:** start with `MODEL.ipynb`.
* **To run the complete real-time pipeline:** run the `predictive-maintenance-dashboard` project.
* The Project Report explains the complete Project in detail from start till end.
