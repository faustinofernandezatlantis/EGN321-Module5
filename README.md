# Virtual Sensor Reliability Lab - Temperature Sensor

## Overview
This project simulates a virtual NTC temperature sensor connected to an Arduino Uno using Velxio. The objective is to collect real-time data, analyze sensor reliability, identify invalid or suspicious noise, and design a data filtering rule.

## Sensor Details
* **Sensor Type:** NTC Temperature Sensor (TMP36 behavior)
* **Measurement:** Temperature in Degrees Celsius (°C)
* **Sampling Rate:** 1 sample per second (1 Hz)

## Collection Method
Data was collected by running the `sketch.ino` simulation in Velxio for 101 consecutive seconds. During the test, ambient temperature adjustments, extreme value spikes, and forced sensor disconnections were introduced to observe normal and abnormal system behavior.

## Dataset Structure
The dataset is saved in `sensor_readings.csv` with the following columns:
* `sample_number`: Sequential integer identifier for each reading.
* `temperature_c`: Calculated temperature in degrees Celsius (°C).
* `status`: Classification label assigned by firmware (`OK`, `OUT_OF_RANGE`, `MISSING`).

## Files Included
* `sensor_readings.csv` - Raw sensor output (101 readings).
* `sketch.ino` - Arduino C++ source code.
* `README.md` - Documentation and setup information.