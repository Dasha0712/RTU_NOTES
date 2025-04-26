# RTU Documentation — Field & Configuration Notes

> Personal documentation based on hands-on learning and site experience.

---

## 1. What is an RTU?

An **RTU (Remote Terminal Unit)** is a protocol converter and data transmitter used in industrial automation and SCADA systems.

> **RTU NETWORK TREE**  HOST Activity means **data send** and SUB Activity means **data receive**

---

## 2. Modules in RTU

The most common modules found in an RTU are:

- Power Supply Unit (PSU)
- Communication Module Unit (CMU)
- Bus Communication Unit (BCU)
- Analog Input Unit (AIU)
- Analog Output Unit (AOU)
- Binary Input Unit (BIU)
- Binary Output Unit (BOU)

---

## 3. Cabinets and Rack Types

- Customer Specification Cabinet
- Swing Frame Rack
- MPR Rack

> **Note:** Chords (cables) should be arranged based on slot addresses, which must match both the physical device and the scheme.

---

## 4. RTU500 Series Types

| Model  | Notes |
|--------|-------|
| RTU560 | Most commonly used; highly reliable |
| RTU530 | Newer model |

---

## 5. LED Indicators on RTU

| LED State | Meaning |
|-----------|---------|
| Red LED — Blinking | Warning |
| Red LED — Continuously Glowing | Alarm (something is wrong) |

> **Remember:** All alarms are events, but not all events are alarms.
> **RS485(THREE WIRE) to ethernet cable(choosing three wire):** choose greenwhite and orange wire.(if you choose wrong wire -RTU webinterface - inoperable error will show - makesure rx tx light is glowing)

---

## 6. RTU Licensing

### License Types
- HMI
- PLC
- Basic

Licensing is based on the **number of data points (signals)** that can be configured.

### RTU530 — Digital License
- Generate the digital license using the device's serial number.
- A PDF guide exists for generating and regenerating the license for new or existing devices.

---

## 7. RTU Files

| File | Extension | Description |
|------|-----------|-------------|
| RTU Project File | `.rtu` | Main project file |
| RTU Configuration Description | `.rcd` | Generated from the `.rtu` file |

### Key Rules
- Always work with the `.rcd` file.
- Whenever changes are made, generate a new `.rcd` file with an incremented revision version.
- Only the `.rcd` file is uploaded to the RTU.
- When exporting RTU file + license, the output is always named `***config.rcd`.
- Every RTU has a **base file** and a **config file**. If no other option works, reset to the base `.rcd` file.

---

## 8. Common Problems & Troubleshooting

### Problem 1 — Data Point License Slots

- **Scenario A:** 5000 total data points, four slots, each slot licensed for 5000 DP → **No problem.**
- **Scenario B:** Two slots (main + standby), each with upstream/downstream, each licensed for 5000 DP → **Problem.** (License count conflict due to slot pairing.)

---

### Problem 2 — Checking Binary Input/Output Health

#### Binary Input (Live Condition)
- If input is confirmed sent from the field device but is not being received:
  - **Check the quality of data.**
  - In the data hierarchy model, these three fields are mandatory:
    1. Quality
    2. Timestamp
    3. Magnitude / Status

#### Binary Output (Live Condition)
- Place multimeter's positive terminal at the end going to the field device.
- Place the other terminal at the negative of the same circuit.
- *(Refer to circuit diagram for clarity.)*

#### Binary Output (Off Condition)
- After issuing a make contact command, perform a **continuity test** — continuity should flow.

---

## 9. IMD01 Card

- Accepts **R, Y, B, N** inputs for both current and voltage.
- Automatically calculates:
  - Power Factor
  - Reactive Power
  - Apparent Power

---

## 10. Sending Data to Remote Control Center (RCC)

| Parameter | Rule |
|-----------|------|
| IOA Address | Must be **different** for each object |
| ASDU Address | Must be the **same** across devices |

---

## 11. Buffer vs. Unbuffered Signals

| Signal Type | Mode | Reason |
|-------------|------|--------|
| Measured / Analog values | Unbuffered | Buffering fills storage rapidly |
| Protection signals, Trip signals | Buffered | Critical events must be preserved |

---

## 12. Time Synchronization in RTU

RTU time can be synchronized in three ways:

1. **SNTP Client** — from a GPS server
2. **Remote Control Center (RCC)** — RCC pushes time to RTU
3. **RTC (Real Time Clock)** — onboard clock

---

## 13. RTU Web Interface — Login Credentials

| Mode | Username | Password |
|------|----------|----------|
| Default Mode | Default / Root | Default / Root |
| Engineer Mode | Admin | Admin |

---

## 14. CAN Bus

> Always ensure the CAN bus wire is connected to the **Bus Communication Unit (BCU)**.||
> Twist the wire like pc keyboard **adding extended module to main module using communication wires**

---

## 15. Firmware Upgrade

- Upgrade **one board at a time** (single CMR/CMU unit).
- When prompted: *"Are you going to upgrade firmware to all CMR or single board at a time?"* → **Choose: Single board.**

---

## 16. Ethernet Port LED Indicators

| LED Color | Meaning |
|-----------|---------|
| Green | Communication established |
| Orange | Data is being transferred |

---

## 17. Modbus — Value Not Coming Correctly

- If Modbus values are incorrect or missing → **Check the data type(little endian & big endian) in the settings.**

---

## 18. Invalid (IV) Mode in RTU

If **IV (Invalid)** mode is displayed on RTU hardware, troubleshoot in this order:

1. **Physical connection** — Check if the chord is properly fitted in the rack.
2. **Configuration** — Check if rack address is correctly configured for the RTU modules.
3. **Hardware failure** — If steps 1 and 2 are confirmed correct, the module may be faulty.

---

## 19. WebInterface: Default IP - 169.254.0.10


## 20. Site Checklist

- [ ] Store backup file in **three separate locations** immediately upon arrival — this is critical.
- [ ] For commissioning sites: verify **BOM (Bill of Materials)** to ensure all items are available.
- [ ] **Send a mail/email** documenting everything — it can be a lifesaver later.
- [ ] Check **IED Scout** to confirm the device is appearing and communicating.

---

*Last updated: April 2026*