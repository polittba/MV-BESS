# Mountainview Generation Station — Ovation DCS Baseline (Balance of Plant)

Source diagram: [MVGS_Baseline Architecture_Ovation_V1.png](../img-Architecture/MVGS_Baseline%20Architecture_Ovation_V1.png)

Owner: IT Enterprise Architecture
Classification: INTERNAL
Version: V1 (5/8/2026) — H. Mendel / B. Smith

## 1. Purpose

This document describes the **baseline Emerson Ovation Distributed Control
System (DCS)** that provides **Balance of Plant (BoP)** monitoring and
control at Southern California Edison's **Mountainview Generation Station
(MVGS)**. It captures the as-built network segmentation, the major server
and workstation roles, the field controllers, and the external interfaces
that surround the Ovation system today. It is intended as a reference for
IT/OT architects, controls engineers, cybersecurity, and integration teams
working on MVGS modifications (including the upcoming BESS integration).

The Ovation DCS is responsible for the BoP scope at MVGS — Heat Recovery
Steam Generators (HRSG) duct burners, the gas compressor train, water
treatment, and the supporting plant-wide control, historian, and operator
infrastructure. The combustion turbine / generator controls themselves are
provided by a separate **GE DCS (PDH)** that interfaces into the Ovation
environment through a dedicated PDH link.

## 2. Architecture at a Glance

The Ovation deployment is organized as a layered, segmented architecture:

1. **External networks** — `GRID2` and the SCE Enterprise Control Network
   (`ECN`) terminate outside the plant control envelope.
2. **Perimeter** — an `MVGS FW` firewall and a `DMZ` mediate all traffic
   between `GRID2` and the plant.
3. **Field acquisition** — serial/Ethernet gateways (MOXA) and the GE PDH
   link bring RTU, weather, and turbine data onto a `MOXA LAN`.
4. **Field LAN Router with ACL** — a controlled junction between the field
   acquisition LAN and the Ovation `ELC LAN`.
5. **ELC LAN** — Ovation External Link Controllers (ELCs), deployed in
   PRI/SEC pairs, terminate DNP3 and MODBUS sessions and present the data
   into Ovation.
6. **ROOT network** — the Ovation plant bus that connects controllers,
   operator stations, engineering and historian servers, and the control
   building infrastructure.
7. **Plant-wide control segments** — HRSG, Gas Compressor, and Water
   Treatment areas, each with their own Ovation controllers and Remote I/O.
8. **PWCS LAN** — an isolated Plant-Wide Cyber Security suite for the
   Ovation environment.

## 3. External Interfaces and Perimeter

| Zone / Device | Role |
| --- | --- |
| `GRID2` | External SCE grid-side network, brought to the MVGS firewall. |
| `MVGS FW` | Edge firewall enforcing the boundary between `GRID2` and MVGS. |
| `DMZ` | Demilitarized zone for controlled data exchange between `GRID2` and the plant. |
| `EDS` | Edge / data services workstation reachable from the DMZ side. |
| `ECN` | SCE Enterprise Control Network, the source of RTU and weather feeds. |

All north-bound integration of the Ovation environment to SCE corporate or
grid systems traverses the `MVGS FW` and `DMZ`. The PWCS LAN, ROOT, ELC
LAN, MOXA LAN, and PDH networks are all considered inside the plant
control envelope.

## 4. Field Acquisition Layer (MOXA LAN and PDH)

The `MOXA LAN` aggregates serial devices that arrive from the `ECN` via
MOXA Serial/Ethernet gateways. The `PDH` segment carries the link to the
plant's combustion turbine controls.

| Source Device | Gateway / Link | Notes |
| --- | --- | --- |
| Trimark RIG | Serial/Ethernet (MOXA) | Remote Intelligent Gateway feed. |
| Weather Station | Serial/Ethernet (MOXA) | Plant meteorological data. |
| OMNI RTU | Serial/Ethernet (MOXA) | RTU feed from ECN. |
| EMS RTU | Serial/Ethernet (MOXA) | Energy Management System RTU. |
| FOCUS RTU | Serial/Ethernet (MOXA) | FOCUS dispatch / monitoring RTU. |
| GE DCS (PDH) | PDH link | Turbine/generator DCS — *not part of Ovation*; shown as the external/legacy yellow block on the diagram. |

The MOXA LAN and the PDH link both terminate at the **Field LAN Router**,
which enforces an **Access Control List (ACL)** before any traffic is
permitted onto the `ELC LAN`.

## 5. External Link Controllers (ELC LAN)

The ELC LAN hosts the Ovation External Link Controllers that translate
between field protocols and the Ovation ROOT network. All ELCs are
deployed as **PRI/SEC (primary / secondary) redundant pairs**.

| ELC Pair | Protocol Role | Upstream Source |
| --- | --- | --- |
| ELC — RIG (PRI/SEC) | DNP3 Client | Trimark RIG |
| ELC — Weather (PRI/SEC) | MODBUS Client | Weather Station |
| ELC — EMS RTU (PRI/SEC) | DNP3 Server | EMS RTU |
| ELC — FOCUS (PRI/SEC) | DNP3 Client | FOCUS RTU |
| ELC — GenMS (PRI/SEC) | DNP3 Server | Generation Management System |
| Controller (01/51) | Ovation controller | Plant-wide / common functions |

The `(01/51)` numbering convention (and the `xx/yy` numbering used
throughout the rest of the architecture) follows Ovation's standard
**primary controller / partner controller** pairing.

## 6. Ovation ROOT Network

The `ROOT` network is the Ovation plant data highway. Connected to ROOT
are the ELC pairs, every Ovation controller, every Operator Station, the
historian and engineering infrastructure, and the control-building
servers.

### 6.1 Control Building Infrastructure

| Device | ID | Role |
| --- | --- | --- |
| GPS Clock | — | Time source for the Ovation environment. |
| Printer | — | Plant printing services. |
| Process Historian | 160 | Primary Ovation historian. |
| Engineering Station | 180 | Ovation engineering / configuration. |
| Domain Controller | 150 | Ovation Windows domain controller. |
| Database Server | 200 | Ovation database services. |
| Terminal Services Server | 245 | Remote / terminal access services. |

### 6.2 Main Control Room

| Operator Station | ID | Desk |
| --- | --- | --- |
| Operator Station | 210 / 211 | Unit 3 Desk |
| Operator Station | 220 / 221 | Unit 4 Desk |
| Operator Station | 230 | Common / additional desk |

### 6.3 Backup Control Room

| Device | ID | Role |
| --- | --- | --- |
| Process Historian | 161 | Backup historian located in the Backup Control Room. |

## 7. HRSG Duct Burner Controls

Each of the four Heat Recovery Steam Generators has a dedicated Ovation
controller pair and a local Operator Station. All connect back to the
ROOT network.

| HRSG Area | Operator Station | Controller (PRI/SEC) | Function |
| --- | --- | --- | --- |
| HRSG 3A | 212 | 38 / 88 | Duct Burner 3A |
| HRSG 3B | 213 | 39 / 89 | Duct Burner 3B |
| HRSG 4A | 222 | 48 / 98 | Duct Burner 4A |
| HRSG 4B | 223 | 49 / 99 | Duct Burner 4B |

## 8. Gas Compressor (GCF MCC Building)

The Gas Compressor train is controlled by a single Ovation controller
pair with six Remote I/O (RIO) drops, one per compressor / suction skid.

| Device | ID | Function |
| --- | --- | --- |
| Controller | 20 / 70 | Gas Compressor master controller (PRI/SEC) |
| RIO 20-1 | — | Gas Compressor Skid 1 |
| RIO 20-2 | — | Gas Compressor Skid 2 |
| RIO 20-3 | — | Gas Compressor Skid 3 |
| RIO 20-4 | — | Gas Compressor Skid 4 |
| RIO 20-5 | — | Gas Compressor Skid 5 |
| RIO 20-6 | — | Gas Compressor Suction Skid |

## 9. Water Treatment

Water Treatment is split across the Water Treatment MCC Building (process
equipment and RIO) and the Water Treatment Lab (local HMI).

| Location | Device | ID | Function |
| --- | --- | --- | --- |
| Water Treatment MCC Building | Controller | 21 / 71 | Water Treatment master controller (PRI/SEC) |
| Water Treatment MCC Building | RIO 21-1 | — | Reverse Osmosis |
| Water Treatment MCC Building | RIO 21-2 | — | Pre-Treatment |
| Tank Farm | RIO 21-3 | — | Tank Farm |
| Lime/Ash Silo | RIO 21-4 | — | Lime / Ash Silo |
| Water Treatment Lab | Operator Station | 240 | Local Water Treatment HMI |

## 10. PWCS LAN — Plant-Wide Cyber Security Suite

The PWCS LAN is a dedicated segment off the Control Building that hosts
the cybersecurity tooling for the Ovation environment.

| Device | Role |
| --- | --- |
| Cybersecurity Suite User Interface | Operator / analyst console for the cyber suite. |
| Cybersecurity Suite VM Host | Virtualization host for the cyber suite workloads. |
| Cybersecurity Suite NAS | Storage for cyber suite data (logs, baselines, artifacts). |

## 11. Conventions and Notes

- **Controller numbering** follows Ovation's `primary / partner`
  convention (e.g., `38 / 88`, `20 / 70`, `21 / 71`, `01 / 51`).
- **ELC pairs** are denoted `(PRI/SEC)` and are deployed redundantly.
- The **GE DCS (PDH)** block is shown on the diagram with a dashed yellow
  outline to indicate that it is **not part of the Ovation BoP scope** —
  it is the combustion turbine / generator DCS that interfaces to Ovation
  via the PDH link.
- The **Field LAN Router (ACL)** is the single enforced choke point
  between the MOXA LAN / PDH and the Ovation ELC LAN.
- All operator interaction with the BoP occurs from Operator Stations on
  the ROOT network (Main Control Room, HRSG areas, and the Water
  Treatment Lab); the Backup Control Room currently hosts the secondary
  historian only.
