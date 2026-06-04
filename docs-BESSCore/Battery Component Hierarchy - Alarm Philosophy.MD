# Battery Component Hierarchy — Alarm Philosophy

## Reference System: 4MW / 12MWh BESS (2 × PCS @ 1–4 MW, 4 × BMS @ 1 MW)

> This document defines the alarm philosophy for each component in the Battery Component Hierarchy. It establishes severity classifications, operator response expectations, and system actions for all events, alarms, faults, and key statuses.

---

## Severity Level Definitions

| Severity | Color Code | Description | Required Operator Action | System Action |
|---|---|---|---|---|
| **Info** | Blue | Normal operational event; no issue present | Log / acknowledge at convenience | Log to historian |
| **Warning** | Yellow | Approaching a limit or degraded condition; system still operational | Investigate within shift | Log, notify EMS/SCADA |
| **Alarm** | Orange | Limit exceeded or abnormal condition requiring prompt attention | Investigate and respond within minutes | Log, annunciate, notify EMS/SCADA |
| **Fault** | Red | Protective action triggered; equipment may have tripped or shutdown | Immediate investigation required before restart | Log, annunciate, initiate protective trip or derate |
| **Critical** | Red / Flashing | Safety-critical condition; risk to personnel, equipment, or grid | Immediate response; follow emergency procedure | Log, initiate E-Stop or full system trip, notify authorities if required |

---

## General Alarm Philosophy Principles

1. **Nuisance alarm suppression** — Alarms shall be suppressed when the associated equipment is in a mode where the alarm is not meaningful (e.g., temperature alarms suppressed during planned offline cooling maintenance).
2. **Alarm shelving** — Operators may shelve a Warning-level alarm for a defined period (max 8 hours) with a documented reason. Alarms cannot be shelved indefinitely.
3. **Alarm flood prevention** — In a cascade event, root-cause alarms shall be annunciated first. Consequential alarms shall be grouped or suppressed until the root cause is cleared.
4. **First-out indication** — Fault conditions shall capture and display the first fault in a sequence to support root-cause analysis.
5. **Return-to-normal events** — All alarms shall generate a return-to-normal (RTN) event log entry when the condition clears.
6. **Acknowledgement** — Alarms require explicit operator acknowledgement. Auto-acknowledgement is not permitted for Fault or Critical severities.
7. **Latching faults** — Fault and Critical severity conditions are latching; they require operator acknowledgement and manual reset before equipment can be returned to service.

---

## Component Alarm Tables

### Grid

| Signal / Condition | Severity | Threshold / Trigger | System Action | Operator Action |
|---|---|---|---|---|
| Grid connected | Info | Utility voltage present, breaker closed | Log | None required |
| Grid disconnected | Info | Planned disconnection via EMS command | Log | None required |
| Islanding detected | **Critical** | Anti-islanding relay pickup | Trip grid interconnect breaker immediately | Verify islanding, follow reconnection procedure |
| Under-voltage warning | **Warning** | Voltage < 90% nominal | Log, notify EMS | Investigate grid conditions |
| Over-voltage warning | **Warning** | Voltage > 110% nominal | Log, notify EMS | Investigate grid conditions |
| Under-voltage trip | **Fault** | Voltage < 80% nominal for > 2 s | Trip ESS from grid | Verify grid, await stable voltage before reconnect |
| Over-voltage trip | **Fault** | Voltage > 120% nominal | Trip ESS from grid | Verify grid, await stable voltage before reconnect |
| Under-frequency warning | **Warning** | Frequency < 59.3 Hz | Log, notify EMS | Investigate |
| Over-frequency warning | **Warning** | Frequency > 60.5 Hz | Log, notify EMS | Investigate |
| Under/over frequency trip | **Fault** | Outside IEEE 1547 ride-through limits | Trip ESS from grid | Verify frequency restored, follow reconnect procedure |
| Phase imbalance warning | **Warning** | Imbalance > 2% | Log, notify EMS | Investigate source of imbalance |
| Reverse power flow | **Alarm** | Unexpected reverse flow detected | Notify EMS | Investigate metering and dispatch setpoints |
| Loss of Grid (LOG) | **Critical** | Grid voltage lost unexpectedly | Trip ESS, initiate island detection | Follow emergency reconnection procedure |

---

### Transformer

| Signal / Condition | Severity | Threshold / Trigger | System Action | Operator Action |
|---|---|---|---|---|
| Tap change | Info | Tap position changed | Log | None required |
| Cooling stage change | Info | Cooling fan/pump stage on or off | Log | None required |
| High winding temperature — warning | **Warning** | Winding temp > 90°C (or vendor limit) | Log, notify | Schedule inspection |
| High winding temperature — trip | **Fault** | Winding temp > 105°C (or vendor limit) | Trip transformer | Do not re-energize until cooled and inspected |
| Low oil level | **Alarm** | Oil level below minimum mark | Log, notify | Schedule oil top-up; inspect for leaks |
| Cooling fan failure | **Alarm** | Fan feedback failure | Log, notify, derate load if possible | Replace fan, monitor temperature closely |
| Buchholz gas accumulation | **Alarm** | Gas detected in conservator | Log, notify | Sample gas, investigate internal fault |
| Buchholz trip (oil surge) | **Critical** | Oil surge relay pickup | Trip transformer immediately | Do not re-energize; conduct internal inspection |
| Differential protection trip | **Critical** | Differential relay pickup | Trip transformer immediately | Do not re-energize; investigate internal fault |
| Overcurrent trip | **Fault** | Overcurrent relay pickup | Trip transformer | Investigate fault source before re-energizing |

---

### Switchgear / Grid Interconnect Protection

| Signal / Condition | Severity | Threshold / Trigger | System Action | Operator Action |
|---|---|---|---|---|
| Breaker closed | Info | Close command executed successfully | Log | None required |
| Breaker opened | Info | Open command executed successfully | Log | None required |
| Permissive granted | Info | Interlock logic satisfied | Log | None required |
| Permissive denied | **Alarm** | Interlock condition not met | Log, annunciate | Investigate interlock condition |
| Reclose initiated | Info | Auto-reclose sequence started | Log | Monitor outcome |
| Breaker failure to close | **Fault** | Close command issued, no feedback within timeout | Log, annunciate | Inspect breaker mechanism, check control power |
| Breaker failure to open | **Critical** | Trip command issued, no feedback within timeout | Backup trip initiated | Emergency: manually isolate upstream |
| Protection relay pickup — phase OC | **Alarm** | Overcurrent relay element picked up (not yet tripped) | Log, annunciate | Investigate load / fault conditions |
| Overcurrent trip | **Fault** | Overcurrent relay trip | Open breaker | Identify and clear fault before reclose |
| Ground fault trip | **Fault** | Ground fault relay trip | Open breaker | Identify and clear ground fault |
| Differential trip | **Critical** | Differential relay trip | Open breaker immediately | Do not reclose; investigate internally |
| Under/over voltage trip | **Fault** | Voltage relay trip | Open breaker | Verify grid stability before reclose |
| Frequency trip | **Fault** | Frequency relay trip | Open breaker | Verify frequency stable before reclose |
| Anti-islanding trip | **Critical** | Anti-islanding relay trip | Open breaker immediately | Verify islanding condition cleared |

---

### Energy Storage System (ESS) — Top Level

| Signal / Condition | Severity | Threshold / Trigger | System Action | Operator Action |
|---|---|---|---|---|
| Mode transition (Standby / Charge / Discharge) | Info | EMS command or automatic transition | Log | None required |
| Dispatch command received | Info | EMS setpoint change | Log | None required |
| E-Stop activated | **Critical** | Physical E-Stop button pressed | Immediate full system trip, open all contactors | Investigate cause; physical reset required before restart |
| E-Stop reset | Info | E-Stop reset authorized and executed | Log | Verify system ready before restart |
| System curtailment | **Warning** | EMS or protection-initiated output limit | Log, notify EMS | Investigate cause of curtailment |
| SoC high limit warning | **Warning** | System SoC > 95% | Reduce charge setpoint | Verify EMS is managing charge termination |
| SoC low limit warning | **Warning** | System SoC < 5% | Reduce discharge setpoint | Verify EMS is managing discharge termination |
| Communication loss to EMS | **Alarm** | No EMS heartbeat for > 60 s | Enter failsafe mode (hold or ramp to zero) | Restore communications; verify failsafe behavior |
| EMS communication timeout — trip | **Fault** | No EMS heartbeat for > 5 min | Trip PCS to safe state | Restore comms; investigate network fault |
| Master contactor failure | **Fault** | Contactor command/feedback mismatch | Annunciate, isolate string | Inspect contactor; do not re-energize until cleared |
| Interlock chain fault | **Fault** | Interlock sequence failure | Block restart | Identify failed interlock; correct before restart |

---

### Power Conversion System (PCS) / Inverter

| Signal / Condition | Severity | Threshold / Trigger | System Action | Operator Action |
|---|---|---|---|---|
| Pre-charge start / complete | Info | Sequence step completed | Log | None required |
| Grid sync achieved | Info | PLL locked to grid | Log | None required |
| Setpoint change | Info | Active/reactive power setpoint updated | Log | None required |
| Derating active | **Warning** | Thermal or voltage derate applied | Log, notify EMS | Investigate derate cause; check cooling |
| AC over/under voltage warning | **Warning** | AC voltage ± 5–10% nominal | Log, notify | Monitor grid conditions |
| DC over/under voltage warning | **Warning** | DC voltage approaching limits | Log, notify | Verify battery SoC and string health |
| Over-temperature warning | **Warning** | Internal temp > 80% of trip threshold | Log, notify, begin derating | Check cooling system; reduce load if needed |
| Fan degraded | **Alarm** | Fan feedback below expected RPM | Log, annunciate, derate | Schedule fan replacement |
| Ground fault warning | **Alarm** | Insulation resistance low on AC side | Log, annunciate | Investigate wiring; do not ignore |
| AC overcurrent fault | **Fault** | AC current > trip threshold | Trip PCS | Identify fault source before restart |
| DC overcurrent fault | **Fault** | DC current > trip threshold | Trip PCS | Identify fault source before restart |
| Over-temperature shutdown | **Fault** | Internal temp > trip threshold | Trip PCS | Do not restart until cooled + root cause found |
| IGBT / gate driver fault | **Fault** | Gate driver feedback fault | Trip PCS | Requires vendor inspection before restart |
| Pre-charge timeout fault | **Fault** | Pre-charge not complete within timeout | Abort pre-charge, isolate | Check DC bus for fault or short |
| Ground fault trip | **Critical** | Ground fault relay trip | Trip PCS, open contactors | Locate and clear ground fault before restart |
| DC bus voltage out of range | **Fault** | DC bus below min or above max | Trip PCS | Check battery string health and contactors |
| Internal communication fault | **Fault** | Internal CAN/fiber comm failure | Trip PCS | Requires vendor investigation |

---

### DC Bus

| Signal / Condition | Severity | Threshold / Trigger | System Action | Operator Action |
|---|---|---|---|---|
| String contactor closed | Info | Contactor close command successful | Log | None required |
| String contactor opened | Info | Contactor open command successful | Log | None required |
| IMD test initiated | Info | Insulation monitoring device test started | Log | None required |
| DC bus voltage high warning | **Warning** | Voltage > 95% of max rating | Log, notify | Verify charging setpoint and string behavior |
| DC bus voltage low warning | **Warning** | Voltage < min operating threshold + 5% | Log, notify | Verify SoC and string health |
| Insulation resistance low warning | **Warning** | IR < 100 kΩ (typical threshold) | Log, annunciate | Investigate wiring; schedule inspection |
| String contactor fail-to-operate | **Alarm** | Command/feedback mismatch | Log, annunciate | Inspect contactor and control power |
| DC bus overvoltage fault | **Fault** | Voltage > absolute max | Trip PCS and open contactors | Do not re-energize; investigate overvoltage source |
| DC bus undervoltage fault | **Fault** | Voltage < absolute min | Trip PCS | Check battery string health and contactors |
| Insulation fault (ground fault) | **Critical** | IR below critical threshold | Trip PCS, open all DC contactors | Locate ground fault before any re-energization |
| String contactor welded | **Critical** | Contactor cannot open on command | Isolate from both sides | Do not operate; requires physical replacement |

---

### Battery (Containerized Unit / Master BMS)

| Signal / Condition | Severity | Threshold / Trigger | System Action | Operator Action |
|---|---|---|---|---|
| Balancing started / stopped | Info | Cell balancing routine | Log | None required |
| Contactor open / close | Info | Normal operation | Log | None required |
| SoC high threshold crossed | **Warning** | SoC > 95% | Reduce charge rate | Verify EMS charge termination |
| SoC low threshold crossed | **Warning** | SoC < 5% | Reduce discharge rate | Verify EMS discharge management |
| Maintenance mode entered | Info | Operator-initiated | Log | Ensure appropriate lockout/tagout |
| Pack overvoltage warning | **Warning** | Pack voltage > warning threshold | Derate charge | Investigate cell balance and charging rate |
| Pack undervoltage warning | **Warning** | Pack voltage < warning threshold | Derate discharge | Investigate SoC and load |
| High temperature warning | **Warning** | Max cell temp > 40°C (LFP typical) | Derate, increase cooling | Check HVAC, reduce load |
| Cell imbalance warning | **Warning** | Max–min cell voltage delta > 50 mV | Log, initiate balancing | Monitor; if persistent, schedule inspection |
| Current limit derate | **Warning** | BMS-imposed current limit active | Derate setpoint | Investigate cause of limit |
| Pack overvoltage fault | **Fault** | Pack voltage > hard limit | Open main contactor, trip | Do not re-energize; investigate cell/charging fault |
| Pack undervoltage fault | **Fault** | Pack voltage < hard limit | Open main contactor, trip | Investigate; check for overdischarge |
| Overtemperature fault | **Fault** | Max cell temp > 55°C (LFP typical) | Open contactor, notify FACP | Investigate thermal event; inspect cells |
| Overcurrent fault | **Fault** | Current > BMS hard limit | Open contactor | Investigate fault source on DC bus |
| Contactor weld fault | **Critical** | Contactor cannot open | Annunciate immediately | Do not operate; physically isolate |
| BMS communication loss | **Fault** | No BMS heartbeat to EMS/SCADA | Enter safe state | Restore comms; do not operate blind |
| Cell voltage out of range | **Critical** | Any cell below absolute min or above absolute max | Open contactor, annunciate | Inspect cells; potential for thermal runaway — follow fire procedure |

---

### Battery Rack / String

| Signal / Condition | Severity | Threshold / Trigger | System Action | Operator Action |
|---|---|---|---|---|
| Rack contactor open / close | Info | Normal operation | Log | None required |
| Rack isolated from string | **Warning** | Manual or automatic isolation | Log, notify | Investigate reason; reduce available capacity |
| Rack returned to service | Info | Rack re-paralleled to string | Log | Verify rack health before returning |
| Rack overvoltage warning | **Warning** | Rack voltage > warning threshold | Derate | Investigate |
| Rack undervoltage warning | **Warning** | Rack voltage < warning threshold | Derate | Investigate |
| High temperature warning | **Warning** | Rack max temp > 40°C | Derate, increase cooling | Check HVAC |
| Fuse blown alarm | **Alarm** | Fuse continuity lost | Isolate rack, annunciate | Replace fuse after investigation |
| Rack communication loss | **Alarm** | No heartbeat from rack BMS | Isolate rack from dispatch | Investigate communication link |
| Rack overvoltage fault | **Fault** | Rack voltage > hard limit | Open rack contactor | Investigate before restoring |
| Rack undervoltage fault | **Fault** | Rack voltage < hard limit | Open rack contactor | Investigate before restoring |
| Rack overtemperature fault | **Fault** | Rack max temp > 55°C | Open rack contactor, notify FACP | Thermal inspection required |
| Rack overcurrent fault | **Fault** | Rack current > hard limit | Open rack contactor | Identify fault source |
| Fuse open fault | **Fault** | Fuse open confirmed | Isolate rack | Replace fuse; investigate overcurrent cause |
| Rack BMS communication fault | **Fault** | Persistent comm failure | Isolate rack from dispatch | Physical inspection of BMS hardware |

---

### Battery Module

| Signal / Condition | Severity | Threshold / Trigger | System Action | Operator Action |
|---|---|---|---|---|
| Cell balancing started / stopped | Info | Routine balancing | Log | None required |
| Module SoC limit reached | Info | Charge or discharge SoC boundary | Log | None required |
| Cell voltage high warning (any cell) | **Warning** | Any cell > 3.55V (LFP typical) | Reduce charge rate | Monitor; inspect if persistent |
| Cell voltage low warning (any cell) | **Warning** | Any cell < 2.80V (LFP typical) | Reduce discharge rate | Monitor; inspect if persistent |
| Cell temperature high warning | **Warning** | Any cell temp > 40°C | Derate, increase cooling | Check HVAC and cell condition |
| Cell imbalance warning | **Warning** | Max–min cell voltage delta > 50 mV within module | Initiate balancing | Monitor; persistent delta indicates cell degradation |
| Cell overvoltage fault | **Fault** | Any cell > 3.65V (LFP hard limit) | Open rack contactor | Investigate charging fault; inspect cells |
| Cell undervoltage fault | **Fault** | Any cell < 2.50V (LFP hard limit) | Open rack contactor | Investigate overdischarge; inspect cells |
| Cell overtemperature fault | **Fault** | Any cell temp > 55°C | Open rack contactor, notify FACP | Thermal inspection; potential runaway risk |
| Module internal communication fault | **Fault** | Module BMS comms failure | Isolate module/rack | Inspect BMS hardware and wiring |

---

### Battery Cell

| Signal / Condition | Severity | Threshold / Trigger | System Action | Operator Action |
|---|---|---|---|---|
| Cell voltage (normal) | Info | Within 2.80V – 3.55V (LFP) | Log to historian | None required |
| Cell entered balancing | Info | Passive balancing circuit active | Log | None required |
| Cell voltage delta warning | **Warning** | Cell voltage deviates > 30 mV from pack average | Log, initiate balancing | Monitor trend; flag for next maintenance |
| Cell overvoltage warning | **Warning** | Cell > 3.55V | Reduce charge rate | Monitor |
| Cell undervoltage warning | **Warning** | Cell < 2.80V | Reduce discharge rate | Monitor |
| Cell overtemperature warning | **Warning** | Cell temp > 40°C | Derate, notify cooling | Investigate heat source |
| Cell overvoltage fault | **Fault** | Cell > 3.65V | Open rack contactor | Inspect cell; do not recharge until cleared |
| Cell undervoltage fault | **Fault** | Cell < 2.50V | Open rack contactor | Inspect for overdischarge damage |
| Cell overtemperature fault | **Fault** | Cell temp > 55°C | Open rack contactor, alert FACP | High risk of thermal runaway — follow fire emergency procedure |
| Cell open-circuit fault | **Fault** | Cell voltage reads 0V or disconnected | Isolate module | Inspect cell and connections |
| Cell short-circuit detected | **Critical** | Anomalous current with cell voltage collapse | Open all contactors, alert FACP | Evacuate if required; follow fire emergency procedure |

---

### Cooling System (HVAC)

| Signal / Condition | Severity | Threshold / Trigger | System Action | Operator Action |
|---|---|---|---|---|
| Setpoint change | Info | Temperature setpoint updated | Log | None required |
| Mode change | Info | Cooling / Heating / Standby transition | Log | None required |
| Compressor stage on / off | Info | Normal staging | Log | None required |
| Filter maintenance due | **Warning** | Filter differential pressure > threshold or timer expired | Log, notify | Schedule filter replacement |
| High return air temperature warning | **Warning** | Return air > 35°C | Increase cooling output, notify EMS to derate | Inspect airflow and loading; check for blockages |
| Low supply temperature warning | **Warning** | Supply air below setpoint by > 5°C | Log, notify | Investigate over-cooling; check setpoint |
| Compressor fault warning | **Alarm** | Compressor error code without shutdown | Log, annunciate | Schedule service call |
| Filter differential pressure high | **Alarm** | Filter DP > high limit | Log, annunciate | Replace filter immediately |
| Sensor failure | **Alarm** | Temperature sensor out of range or open circuit | Log, annunciate, use redundant sensor | Replace sensor |
| Compressor fault — hard shutdown | **Fault** | Compressor protection trip | Annunciate, notify BMS to derate | Dispatch technician; monitor battery temps closely |
| Fan motor fault | **Fault** | Fan motor protection trip | Annunciate, notify BMS to derate | Dispatch technician; monitor battery temps |
| Loss of cooling | **Critical** | Battery area temperature exceeds BMS derate threshold with no cooling recovery | Full derate/trip of ESS | Immediate inspection; risk of overtemperature fault cascade |

---

### Fire Alarm Control Panel (FACP)

| Signal / Condition | Severity | Threshold / Trigger | System Action | Operator Action |
|---|---|---|---|---|
| Panel status normal | Info | All zones normal | Log | None required |
| Walk-test mode entered / exited | Info | Operator-initiated maintenance mode | Log | Ensure suppression is armed on exit |
| Supervisory: gate valve off | **Warning** | Suppression system isolation valve closed | Log, annunciate | Investigate; restore valve to normal position |
| Trouble: device fault / wiring fault | **Warning** | Detector or wiring fault code | Log, annunciate | Inspect and repair device or wiring |
| AC power loss | **Alarm** | Mains power to FACP lost | Switch to battery backup, annunciate | Restore AC power; monitor backup battery level |
| Battery backup low | **Alarm** | FACP internal battery below 80% | Log, annunciate | Replace FACP backup battery |
| Detector open / short circuit | **Alarm** | Wiring fault on detector zone | Log, annunciate, zone supervisory | Inspect and repair |
| Pre-alarm (smoke / heat) | **Alarm** | Detector reading elevated but below alarm threshold | Log, annunciate, notify EMS | Investigate area; prepare for potential alarm |
| Smoke detector alarm | **Critical** | Detector threshold exceeded | Annunciate, initiate pre-discharge sequence, notify EMS to trip ESS | Evacuate ESS enclosure; follow fire emergency procedure |
| Heat detector alarm | **Critical** | Heat detector threshold exceeded | Annunciate, initiate pre-discharge sequence, notify EMS to trip ESS | Evacuate ESS enclosure; follow fire emergency procedure |
| Manual pull station activated | **Critical** | Manual station pulled | Initiate suppression sequence, trip ESS | Evacuate; verify suppression discharge |
| Communication fault to suppression panel | **Fault** | No comms to suppression controller | Annunciate | Inspect network link; verify suppression armed locally |
| Zone isolation fault | **Fault** | Zone isolated without authorization | Log, annunciate | Restore zone; document reason |
| FACP battery backup fail | **Fault** | FACP battery fully depleted | Annunciate | Replace immediately; panel life-safety function at risk |

---

### Fire Suppression Devices (FM-200 / Aerosol)

| Signal / Condition | Severity | Threshold / Trigger | System Action | Operator Action |
|---|---|---|---|---|
| System armed | Info | Normal operational state | Log | None required |
| System disarmed | **Warning** | Suppression disabled by operator | Log, annunciate | Document reason; re-arm as soon as practical |
| Abort switch active | **Warning** | Operator held abort during pre-discharge | Log, annunciate | Investigate alarm cause; re-arm after confirmation |
| Low agent pressure warning | **Warning** | Cylinder pressure below low warning threshold | Log, notify | Schedule cylinder recharge or replacement |
| Pre-discharge warning | **Alarm** | Pre-discharge audible/visual initiated (abort window active) | Annunciate, evacuate warning | Evacuate enclosure; confirm or abort |
| Agent cylinder low pressure fault | **Fault** | Cylinder pressure below minimum operational level | Annunciate, remove from service | Recharge or replace cylinder before arming |
| Solenoid valve fault | **Fault** | Valve actuator feedback failure | Annunciate | Inspect valve; re-arm only after confirmed operational |
| Discharge detection failure | **Fault** | Suppression discharge not confirmed after command | Annunciate | Verify physical discharge; re-arm if failed to discharge |
| System disabled / bypassed | **Fault** | System in bypass beyond authorized period | Annunciate persistently | Restore system; obtain documented authorization for continued bypass |
| Suppression agent discharged | **Critical** | Confirmed discharge event | Log, notify emergency services, trip ESS if not already tripped | Follow post-discharge procedure: ventilate, inspect, recharge, return to service |

---

## Severity Summary by Component

| Component | Info | Warning | Alarm | Fault | Critical |
|---|---|---|---|---|---|
| Grid | 2 | 5 | 1 | 4 | 2 |
| Transformer | 2 | 2 | 2 | 2 | 2 |
| Switchgear | 4 | 1 | 1 | 5 | 3 |
| ESS (Top Level) | 3 | 3 | 1 | 3 | 2 |
| PCS / Inverter | 3 | 4 | 1 | 7 | 1 |
| DC Bus | 2 | 3 | 1 | 3 | 2 |
| Battery (Master BMS) | 3 | 6 | 0 | 5 | 3 |
| Battery Rack | 3 | 4 | 2 | 5 | 0 |
| Battery Module | 2 | 4 | 0 | 4 | 0 |
| Battery Cell | 2 | 4 | 0 | 5 | 1 |
| Cooling System | 3 | 3 | 3 | 2 | 1 |
| FACP | 3 | 3 | 4 | 3 | 4 |
| Fire Suppression Devices | 1 | 3 | 1 | 4 | 1 |

---

## Cross-Component Critical Condition Summary

These conditions require coordinated multi-system response:

| Condition | Components Involved | Coordinated Action |
|---|---|---|
| **Thermal Runaway** | Cell → Module → Rack → Battery → FACP → Fire Suppression | BMS trip → ESS trip → FACP alarm → suppression discharge → emergency services notification |
| **Loss of Cooling** | HVAC → BMS → PCS → ESS | HVAC fault → BMS derate → PCS derate → ESS trip if temperature continues rising |
| **DC Ground Fault** | DC Bus → PCS → Battery | IMD trip → PCS trip → open all DC contactors → lockout before investigation |
| **Loss of Grid** | Grid → Switchgear → ESS | Anti-islanding trip → open grid breaker → ESS hold/shutdown depending on mode |
| **E-Stop Activation** | ESS → PCS → all Battery contactors | Immediate trip of all power paths; physical reset required |
| **Fire Alarm** | FACP → ESS → PCS → all contactors | ESS trip → pre-discharge warning → suppression discharge → emergency services |
