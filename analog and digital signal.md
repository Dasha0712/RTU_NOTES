# Signal Theory & Instrumentation
 
---
 
## 20. Analog, Digital & Binary Signals
 
### Core Idea
 
Both analog and digital signals **vary with time** — the difference is **how** they vary.
 
---
 
### Analog Signal
 
Varies **smoothly and continuously** — can take **any value** within a range.
 
```
Value
10 |        ___
8  |      /     \
6  |    /         \
4  |  /             \
2  |/                 \____
   |_________________________ Time
```
 
Every moment in time can have a different value: 3.5, 6.7, 8.2 — any number.
 
**Examples:** Temperature, Pressure, Flow, Voltage, Current, Power measurements
 
---
 
### Digital Signal
 
Also varies with time — but **only switches between two states**: `0` and `1`.
 
```
Value
1  |  ___       ___       ___
   | |   |     |   |     |   |
0  |_|   |_____|   |_____|   |___
   |_________________________ Time
```
 
Never has intermediate values like 0.5 or 0.7.
 
**Examples:** Breaker OPEN/CLOSE, Alarm ACTIVE/INACTIVE, Switch ON/OFF
 
---
 
### Binary Signal
 
A **type of digital signal** used specifically in automation systems for equipment status:
 
- Breaker status
- Protection trip
- Alarm indication
- Equipment ON/OFF
 
Handled by **Binary Input (BIU)** and **Binary Output (BOU)** modules.
 
---
 
### Key Comparison
 
| Feature | Analog Signal | Digital / Binary Signal |
|---------|--------------|------------------------|
| Nature | Continuous | Discrete |
| Values | Infinite (any number) | Only two: 0 or 1 |
| Shape | Smooth curve | Square wave |
| Example | Temperature rising from 20°C to 85°C | Switch ON for 3 sec, OFF again |
 
---
 
### 🧠 Memory Trick
 
> **Analog = Sunrise 🌅** — the sky slowly gets brighter, infinite shades
>
> **Digital = Light Switch 💡** — fully ON or fully OFF, nothing in between
 
---
 
## 21. Signal Mapping
 
### What is Mapping?
 
Mapping means **converting one range of values into another range proportionally**.
 
A physical quantity (temperature, pressure, speed) cannot travel down a wire as a number — so it is **converted into a current or voltage signal**.
 
```
Physical Quantity → Sensor → Electrical Signal → RTU/PLC Module → SCADA
```
 
---
 
### Example 1 — Temperature → 4–20 mA
 
Sensor measures: `0°C to 100°C`
Transmitter converts to: `4 mA to 20 mA`
 
```
mA
20 |                        ✓
18 |                   ___/
16 |              ___/
14 |         ___/
12 |    ___/  ← 50°C = 12mA
10 |___/
4  |✓
   |_________________________ Temperature
   0    25    50    75    100°C
```
 
**Formula:**
```
mA = 4 + ((Temperature − MinTemp) / (MaxTemp − MinTemp)) × 16
```
> The 16 = 20mA − 4mA (total usable range)
 
**Example Calculations:**
 
| Temperature | Calculation | Result |
|-------------|-------------|--------|
| 0°C | Minimum | 4 mA |
| 25°C | 4 + (25/100) × 16 | 8 mA |
| 50°C | 4 + (50/100) × 16 | 12 mA |
| 75°C | 4 + (75/100) × 16 | 16 mA |
| 100°C | Maximum | 20 mA |
 
---
 
### Example 2 — Bipolar Mapping (−5V to +5V)
 
Used when measurements go in **two directions** (e.g., motor forward/reverse).
 
```
−1000 RPM  →  −5V   (full reverse)
    0 RPM  →   0V   (stopped)
+1000 RPM  →  +5V   (full forward)
```
 
**Formula:**
```
Voltage = (RPM / MaxRPM) × 5V
```
 
| RPM | Voltage |
|-----|---------|
| −1000 | −5V |
| −500 | −2.5V |
| 0 | 0V |
| +500 | +2.5V |
| +1000 | +5V |
 
---
 
### 🧠 Memory Trick
 
> Think of mapping as a **percentage**:
> - 0% of range = minimum signal (4mA or −5V)
> - 50% of range = middle signal (12mA or 0V)
> - 100% of range = maximum signal (20mA or +5V)
 
---
 
## 22. Transducer, Sensor & Transmitter
 
### Definitions
 
| Device | What it does |
|--------|--------------|
| **Sensor** | Only **detects** the physical change |
| **Transducer** | **Detects AND converts** energy from one form to another |
| **Transmitter** | Takes the signal and **sends it as 4–20 mA** over long distance |
 
> **Transducer = Sensor + Transmitter combined**
 
---
 
### Energy Conversion Examples
 
| Transducer | Input Energy | Output Energy |
|------------|--------------|---------------|
| Microphone | Sound | Electrical |
| Speaker | Electrical | Sound |
| Thermocouple | Heat | Electrical voltage |
| Solar Cell | Light | Electrical |
| Electric Motor | Electrical | Mechanical |
| Generator | Mechanical | Electrical |
| Pressure Sensor | Mechanical pressure | Electrical |
| LED | Electrical | Light |
 
---
 
### Key Distinction
 
```
Sensor:
  RTD detects temperature as resistance change → does NOT convert to electrical signal
 
Transducer:
  Thermocouple detects heat AND produces mV voltage → detects AND converts
 
Transmitter:
  Takes the mV → outputs 4–20 mA for long-distance wire transmission
```
 
> Every **Transducer** can be called a Sensor.
> But **not every Sensor is a Transducer**.
 
---
 
### Signal Flow in Automation
 
```
Physical Quantity (Temperature / Pressure / Speed)
              ↓
       Sensor / Transducer
              ↓
  Electrical Signal (4–20mA / 0–10V / −5V to +5V)
              ↓
       RTU / PLC Input Module
              ↓
      Converted to Digital Value
              ↓
           SCADA
```
 
---
 
### 🧠 Memory Trick
 
> Imagine you speak Tamil and your friend speaks only English.
> You need a **translator** in between.
>
> **Transducer = Translator**
> It takes one language (heat, pressure, sound) and translates it to electrical signal — so the PLC can understand it.
 
---
 
## 23. Current Signal Types
 
| Signal Type | Range | Wire Break Detection | Use Case |
|-------------|-------|----------------------|----------|
| **Unipolar** | 0–20 mA | ❌ No | Simple positive measurements |
| **Bipolar** | −20 to +20 mA | ❌ No | Bidirectional measurements (forward/reverse) |
| **Live Zero** | 4–20 mA | ✅ Yes (0mA = fault) | Standard industrial measurement |
| **Extended (4–40 mA)** | 4–40 mA | ✅ Yes (0mA = fault) | High precision measurement |
 
---
 
### Why 4–40 mA instead of 4–20 mA?
 
```
4–20 mA:   4mA ──────────── 20mA    (16 mA total span)
4–40 mA:   4mA ──────────────────── 40mA    (36 mA total span)
```
 
More range = **more precision** = easier to detect small changes in the real value.
 
| Measurement | With 4–20 mA | With 4–40 mA |
|-------------|-------------|-------------|
| 0°C | 4 mA | 4 mA |
| 50°C | 12 mA | 22 mA |
| 100°C | 20 mA | 40 mA |
| Change per 1°C | 0.16 mA | 0.36 mA |
 
> 0.36 mA per degree is **easier to detect** than 0.16 mA — better resolution.
 
---
 
## 24. Live Zero — Full Explanation
 
### Why it is called "Live Zero"
 
```
LIVE  +  ZERO
  ↓         ↓
Alive    Zero point
= "The zero point is alive — it carries a signal"
```
 
---
 
### The Problem with Dead Zero (0 mA systems)
 
In old systems, minimum signal = `0 mA`.
 
| Condition | Signal |
|-----------|--------|
| Wire broken | 0 mA |
| Sensor dead | 0 mA |
| Power failed | 0 mA |
| Actual minimum value | 0 mA |
 
> All four situations look identical to the PLC. It **cannot tell the difference**.
> This is called **Dead Zero** — the zero point is silent.
 
---
 
### The Solution — Live Zero (4 mA system)
 
Minimum signal is shifted to **4 mA**.
 
| Signal | Meaning |
|--------|---------|
| 0 mA | ❌ FAULT — wire broken or sensor dead |
| 4 mA | ✅ ALIVE — reading minimum value |
| 20 mA | ✅ ALIVE — reading maximum value |
 
---
 
### Visual Comparison
 
**Dead Zero:**
```
0mA ─────────────────── 20mA
↑
Also means wire broken!
Cannot distinguish ❌
```
 
**Live Zero:**
```
0mA    4mA ──────────── 20mA
↑       ↑
DEAD   ALIVE (minimum value)
        Zero is now living — it has a signal! ✅
```
 
---
 
### 🧠 Real Life Analogy
 
> Imagine a **security guard calling base every hour**:
>
> **Dead Zero system:**
> - Guard calls = "All clear"
> - Guard missing = No call
> - Radio broken = Also no call
> - Base **cannot tell if guard is safe or radio is broken** ❌
>
> **Live Zero system:**
> - Guard calls every hour = "All clear" (4 mA = alive)
> - No call at all = Something is WRONG (0 mA = dead)
> - Base **immediately knows something is wrong** ✅
 
---
 
### One Line to Never Forget
 
> **"Live Zero means — even at ZERO measurement, the signal is still ALIVE with 4 mA."**
>
> `0 mA = Dead`
> `4 mA = Live Zero` ← zero measurement, but the system is still alive
 
---
 
*Last updated: April 2026 | Personal RTU & Instrumentation Study Notes*
 