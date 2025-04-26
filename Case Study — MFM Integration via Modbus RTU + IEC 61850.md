# Case Study — MFM Integration via Modbus RTU + IEC 61850

> Site scenario: Configuring RTU to collect MFM signals via Modbus and forward them to SCADA via IEC 61850, without breaking an existing SCL file.

---

## Scenario Overview

| Item | Detail |
|------|--------|
| **Existing condition** | SCL file already created and in use at site |
| **New requirement** | Customer wants all MFM (Multi-Function Meter) signals configured in RTU |
| **Protocol — Field side** | Modbus RTU (RTU = Master, all MFMs = Slaves) |
| **Protocol — SCADA side** | IEC 61850 (RTU = Server, SCADA = Client) |

---

## The Problem

- An SCL file is already created and active.
- If a **new SCL file is created separately**, you **cannot load two SCL files** into:
  - The RTU
  - The CET tool of MicroSCADA
- This would break the existing configuration.

---

## The Solution — Step by Step

### Step 1 — Configure the RTU

- Add all MFM signals in the RTU configuration.
- Set up the **Modbus RTU communication** (RTU as Master, MFMs as Slaves).
- Map all MFM signals into the RTU signal list.

---

### Step 2 — Export ICD File from RTU

- After configuring, **export the ICD file** from the RTU.
- The ICD file contains the updated RTU capability description including the new MFM signals.

---

### Step 3 — Create SCD File Using IET Dongle

- Use the **IET dongle** to open the ICD file.
- Generate the **SCD (Substation Configuration Description)** file from it.
- The SCD file merges all IED descriptions and communication bindings into one file.

---

### Step 4 — Import SCD File into RTU

- Import the newly generated SCD file **back into the RTU**.
- This updates the RTU with the full substation configuration.

---

### Step 5 — Export ICD File from RTU Again

- After importing the SCD, **export the ICD file from the RTU once more**.
- This final ICD reflects the complete and updated configuration.

---

### Step 6 — Put New ICD into CET Tool (MicroSCADA)

- Load the new ICD file into the **CET tool of MicroSCADA**.
- Now both sides (RTU Server and SCADA Client) have the **same file**.
- Data sent from the **RTU (Server)** will be correctly received by **SCADA (Client)**.

---

## Flow Diagram

```
MFM Devices (Slaves)
       |
  Modbus RTU
       |
    RTU (Master)
       |
  [Configure RTU — add all MFM signals]
       |
  [Export ICD from RTU]
       |
  [IET Dongle — generate SCD file]
       |
  [Import SCD into RTU]
       |
  [Export ICD from RTU again]
       |
  [Load ICD into CET Tool — MicroSCADA]
       |
  IEC 61850
       |
  SCADA (Client)
```

---

## Key Points to Remember

- Never create a **separate new SCL file** — you cannot use two SCL files simultaneously in RTU or CET tool.
- The correct approach is to **update the existing configuration** by going through the ICD → SCD → ICD cycle.
- The **IET dongle** is required to generate the SCD file — make sure it is available on site.
- After the final ICD is loaded into the CET tool, both RTU and SCADA will be **in sync** — same file, same signals.

---

## Protocol Summary

| Side | Protocol | RTU Role |
|------|----------|----------|
| Field (MFM to RTU) | Modbus RTU | Master |
| SCADA (RTU to SCADA) | IEC 61850 | Server |

---

*Last updated: April 2026 | Site Configuration Case Study*