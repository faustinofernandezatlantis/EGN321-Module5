
---

## 1. Short Analysis Report

### Sensor and Measurement

* **Sensor:** NTC Temperature Sensor (simulating TMP36 behavior).


* **Unit of Measurement:** Degrees Celsius (°C).



### Simulation Platform

* **Platform:** Velxio (Arduino Uno with NTC sensor circuit connected to analog input A0).



### Collection Method

* A total of **101 continuous samples** were collected at 1-second intervals via serial communication from the Arduino board in Velxio.



### Observed Range & Data Statistics

* **Minimum Value (Min):** -32.9 °C (Samples 53 and 54)


* **Maximum Value (Max):** 438.3 °C (Samples 56 through 59)


* **Typical / Baseline Value:** ~20.4 °C during steady-state operation


* **Overall Observed Range:** 471.2 °C (from -32.9 °C to 438.3 °C)



### Normal Variation

During normal operation without external thermal changes (Samples 1 to 37), the sensor exhibited perfect stability at 20.4 °C with 0 °C of random fluctuation. During smooth environmental transitions (Samples 65–70 or 75–80), readings varied gradually between 1.8 °C and 41.4 °C.

---

## 2. Identification of Suspicious or Invalid Readings

1. **Extreme / Impossible High Values (> 100 °C):**
* *Samples 41–43 (172.9 °C) and Samples 56–60 (434.4 °C – 438.3 °C)*.


* *Why investigate:* These values exceed the physical operating environment of the sensor and project scope. They represent sudden simulation artifact spikes or abrupt potentiometer adjustments in Velxio.




2. **Negative Values / `MISSING` State (< 0 °C):**
* *Samples 45–55 (down to -32.9 °C) and Samples 71–74 / 94–98*.


* *Why investigate:* These represent readings where the voltage dropped below the standard 0.5 V minimum operating threshold, which in a physical circuit would indicate a disconnected wire, floating pin, or complete sensor failure.




3. **Physically Impossible Deltas:**
* *Sample 55 (-15.8 °C) to Sample 56 (438.3 °C)*.


* *Why investigate:* A thermal jump of over 450 °C within a single 1-second sample is physically impossible due to thermal inertia in real hardware.



---

## 3. Proposed Rejection / Filtering Rule

To clean unreliable input data, a **Two-Tiered Filtering Rule (Range + Rate of Change)** is proposed for implementation in Python:

1. **Absolute Bounds Filter:**
* Reject any reading outside the supported temperature operating range of **0 °C to 50 °C**.


* Automatically flag any value < 0 °C (`MISSING`) or > 50 °C (`OUT_OF_RANGE`) as invalid.




2. **Rate of Change (Delta) Filter:**
* Reject any reading that deviates by more than **±5.0 °C** from the immediately preceding valid reading.



---

## 4. Cost of the Rule (Tradeoffs)

Every filtering rule introduces potential tradeoffs and risks:

1. **Rejection of Real Emergency Events:** If a genuine thermal emergency occurs (e.g., a sudden rapid fire raising ambient temperature from 25 °C to 65 °C within seconds), the **Bounds Filter** will reject valid high temperatures (> 50 °C) and the **Delta Filter** will discard the fast rate of change (> 5 °C/s), delaying critical alerts.
2. **Masking Hardware Degradation:** Automatically filtering or discarding negative readings and out-of-range spikes can create a false sense that the environment is stable, hiding the fact that the underlying physical wiring or sensor hardware is intermittently failing.
3. **False Positives During Cold/Warm Starts:** If a sensor is rapidly moved between extreme environments (such as taking equipment out of cold storage), valid steep gradients will be wrongfully rejected as noise.

---

