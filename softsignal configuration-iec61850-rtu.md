# Case Study — New RTU Installation with IED via IEC 61850

> Site scenario: Installing a new RTU that needs to receive data from an IED (server) as a client, requiring SCD file preparation and XCEL synchronization.

---

## Network Topology

```
IED (Server)
     |
  Switch
     |
RTU (Client)
```

---

## Scenario Overview

| Item | Detail |
|------|--------|
| **Setup** | New RTU being installed at site |
| **Communication** | IEC 61850 — IED is Server, RTU is Client |
| **Data flow** | IED sends data to RTU (polling) |
| **Problem** | Existing SCD file does not have the new RTU (client) name configured |

---

## The Problem

- For IEC 61850 communication to work, the **SCD file must have both**:
  - **Server name** — IED
  - **Client name** — RTU
- Because the RTU is **newly installed**, the existing SCD file was prepared **without the RTU client name**.
- Without the client name in the SCD, the IED (server) will **not send data** to the RTU (client).

---

## The Solution — Step by Step

### Step 1 — Assign Client Name in RTU

- During RTU configuration, **assign the client name** (RTU name) inside the RTU settings.
- This name must match what will be registered in the SCD file.

---

### Step 2 — Export ICD File from RTU

- After assigning the client name, **export the ICD file** from the RTU.
- The ICD file now contains the RTU's identity and capability description.

---

### Step 3 — Prepare the SCD File

Two options:

| Option | Who does it |
|--------|-------------|
| **Option A** | Use IET tool yourself — prepare SCD file using the exported ICD |
| **Option B** | Give the ICD file to a **Hitachi engineer** — they will prepare the SCD file |

- The SCD file will now contain **both the IED (server) name and the RTU (client) name**.

---

### Step 4 — Export XCEL File (Before Importing New SCD)

> ⚠️ **Do this BEFORE importing the new SCD file — very important.**

- Export the existing **XCEL file** from the RTU tool.
- This is a backup and will be needed for synchronization in the next step.

---

### Step 5 — Import New SCD File into RTU

- Import the newly prepared **SCD file** into the RTU.
- The RTU now has the complete substation configuration with both client and server names.

---

### Step 6 — Synchronize the XCEL File

- After importing the SCD, **synchronize the XCEL file** with the new RTU/SCD configuration.
- This ensures the XCEL file is aligned with the updated SCD data.

---

### Step 7 — Edit Parameters in XCEL and Import

- Now you can **change or update parameters** in the XCEL file as needed (signal mapping, addresses, etc.).
- Once editing is done, **import the updated XCEL file** into the RTU tool (RTUTill).
- Configuration is now complete.

---

## Full Flow Diagram

```
New RTU installed at site
         |
[Assign client name (RTU name) in RTU configuration]
         |
[Export ICD file from RTU]
         |
         ├── Option A: Use IET tool → prepare SCD yourself
         └── Option B: Send ICD to Hitachi engineer → they prepare SCD
         |
[SCD file ready — contains both IED (server) + RTU (client) names]
         |
[Export XCEL file FIRST — before importing SCD]
         |
[Import new SCD file into RTU]
         |
[Synchronize XCEL file with new SCD]
         |
[Edit parameters in XCEL as needed]
         |
[Import updated XCEL into RTUTill]
         |
RTU configured — IED (server) will now send data to RTU (client) ✅
```

---

## Key Points to Remember

- IEC 61850 requires **both server and client names** in the SCD file — missing either one breaks communication.
- Always **export the XCEL file before importing a new SCD** — never skip this step.
- The **IET tool** can be used to prepare the SCD file independently, or it can be delegated to the Hitachi engineer.
- Client name assigned in RTU configuration **must exactly match** what is written in the SCD file.
- Final step is always importing the updated XCEL into **RTUTill** to complete the configuration.

---

## Role Summary

| Device | IEC 61850 Role | Action |
|--------|---------------|--------|
| IED | Server | Sends data (responds to polling) |
| RTU | Client | Receives data (polls the IED) |
| SCADA | — | Not in this loop |

---

## File Sequence Summary

```
RTU Config
    → Export ICD
        → Prepare SCD (IET tool or Hitachi engineer)
            → Export XCEL (backup before SCD import)
                → Import SCD into RTU
                    → Synchronize XCEL
                        → Edit XCEL parameters
                            → Import XCEL into RTUTill ✅
```

---

*Last updated: April 2026 | Site Configuration Case Study*