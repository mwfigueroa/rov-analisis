# ROV Analysis

Case-analysis and tracking project for a **QYSEA FIFISH V6 Expert** exposed to **saltwater**, with a localized **front thruster** failure and a planned referral to an authorized service center for repair and pressure-seal re-certification.

- Suggested repository name: `rov-analisis`
- Versión en español: `README.es.md`
- Short index: `Readme.md`

---

## 1. Current case status

| Field | Status |
|---|---|
| Unit | QYSEA FIFISH V6 Expert |
| User location | Argentina |
| Diagnosis baseline date | 2026-07-30 |
| Event | Saltwater exposure; the ROV was powered on after the incident |
| Initial moisture evidence | Condensation on the inside of the dome lens |
| Saline ingress confirmation | Pending: salt-residue test |
| Test performed | Brief bench test in air |
| Test result | 5 thrusters ran normally; 1 front thruster near the dome trembled/shook |
| Observed temperature | Casing approx. 40–45 °C near the affected area |
| Electrical safety | Li-ion battery removed and stored safely |
| Intervention status | Not re-energized, no disassembly, no repair attempted |

**Process phase:** reasoned diagnosis complete; non-destructive physical measurements pending.

---

## 2. Working hypothesis

### Primary hypothesis
Localized failure in the **front thruster closest to the dome**, most likely a **lost phase**:
- open or high-resistance phase;
- corroded connector;
- ESC channel damaged by saline corrosion.

This is consistent with saltwater exposure, condensation near the dome, motor trembling, localized heating, and only one thruster failing while the other five ran normally.

### Alternative hypothesis
Mechanical branch: obstruction or seized bearing caused by fishing line, hair, sand, crystallized salt, or shaft corrosion.

---

## 3. Project objective

Centralize the technical diagnosis, available evidence, pending measurements, official documentation, RMA emails, and the final repair/shipping/re-certification decision.

The practical goal is to reach a clear, defensible referral to an authorized service center while avoiding further damage from test power-ups, unauthorized opening, or loss of sealing.

---

## 4. Scope

### Includes
- Incident and symptom analysis.
- Separation between benign condensation and real saltwater ingress.
- Non-destructive diagnostic plan.
- Measurement log template.
- RMA request emails in English and Spanish.
- One-page English case summary.
- References to official QYSEA documentation.
- Recommended repair and shipping route from Argentina.

### Out of scope
- Pressure-hull disassembly.
- Internal ROV repair.
- Resealing without leak testing or certification.
- Handling of a damaged or salt-exposed Li-ion battery.

---

## 5. Repository structure

| File | Contents | Main use |
|---|---|---|
| `Readme.md` | Short bilingual index | Repository landing page |
| `README.es.md` | Documentación completa en español | Spanish version |
| `README.en.md` | Full documentation in English | This file |
| `diagnostico-fifish-v6-expert.md` | Full technical diagnosis, test plan, measurement template, service centers, and sources | Central analysis document |
| `case-summary-en.pdf` | One-page English case summary | RMA attachment |
| `correos-reparacion-centros.md` | English RMA emails for Europe and North America | Send to official centers |
| `correos-reparacion-centros-ES.md` | Spanish version of the emails | Spanish-speaking reference |
| `QYSEA_V6Expert_QuickStart_Manual_V2.2_EN.pdf` | Official Quick Start Manual V2.2 | Official reference |
| `QYSEA_V6Expert_QuickStart_Manual_V2.2_EN_extracted-text.txt` | Extracted Quick Start text | Quick search and citation |
| `QYSEA_V6Expert_Motors_Battery_Maintenance_Guide.pdf` | Official motors and battery maintenance guide | Official maintenance reference |

---

## 6. Key official-documentation findings

- Thrusters **must not run in air for more than 30 seconds**.
- After every dive, the ROV must be rinsed/cleaned with **fresh water**.
- The FIFISH app includes a motor cleaning routine of approximately **10 minutes**.
- The battery should be stored at approximately **50–60%** and fully charged every **90 days** if unused.
- QYSEA **does not publish a pressure-hull disassembly procedure** for the V6 Expert.
- After-sales coverage may be excluded by unauthorized modification, non-compliant disassembly/opening, or non-authorized service.

**Operational conclusion:** do not open the hull. Complete non-destructive diagnosis and refer to an authorized center.

---

## 7. Symptoms and interpretation

### Observed symptoms
1. Saltwater exposure.
2. Power-on after the incident.
3. Condensation inside the dome lens.
4. During a brief air test, only one thruster trembled: the front thruster near the dome.
5. Warm/hot casing, approx. 40–45 °C, near the affected area.

### Technical interpretation
A BLDC motor that **trembles and heats** is drawing current without producing useful rotation.

| Branch | Likely cause | Indicator |
|---|---|---|
| Mechanical | Obstruction, seized shaft, corroded bearing | Feels rough, locked, or different from the others when spun by hand |
| Electrical | Lost phase, corroded connector, damaged ESC channel | Spins freely by hand but trembles under power; unbalanced phase-to-phase resistance |

**Key point:** 5 thrusters worked normally in the same air test; air alone does not explain only one failing.

---

## 8. Condensation vs. real saltwater ingress

| Sign | Interpretation |
|---|---|
| Uniform fog that disappears when warmed | Likely benign condensation from trapped internal air |
| Droplets, runs, or pooled water | Real liquid ingress |
| White residue after drying | Salt: seawater entered |
| Green/blue residue | Copper corrosion |
| Moisture in battery bay or other compartments | Real ingress |
| Leak/humidity alarm in app | Detected real ingress |

**Pending test:** wipe accessible internal surfaces with clean cotton and check for salt residue.

---

## 9. Pending diagnostics

- [ ] Physically identify the suspect thruster: front-left or front-right.
- [ ] Choose a healthy reference thruster of the same type.
- [ ] Manual spin with power off: free, rough, or locked.
- [ ] Check for fishing line, hair, sand, or crystallized salt on the shaft.
- [ ] Measure phase-to-phase resistance: U-V, V-W, U-W.
- [ ] Compare suspect against reference.
- [ ] Measure phase-to-ground/chassis insulation.
- [ ] Inspect leads, pins, penetrator, and ESC area if accessible without opening the hull.
- [ ] Check app telemetry: fault code, flagged thruster, voltage, leak/temperature alarms.
- [ ] Confirm whether condensation was benign or real saline ingress.
- [ ] Confirm unit version: M100 or M200.
- [ ] Confirm tether length.
- [ ] Confirm serial number.
- [ ] Confirm purchase data: dealer and date.

---

## 10. Decision matrix

| Result | Likely conclusion | Action |
|---|---|---|
| Suspect rough/locked by hand | Mechanical branch | Do not energize; refer for cleaning/mechanical repair |
| Suspect free by hand but trembled under power | Electrical branch | Measure phases and refer for thruster/ESC repair |
| One phase-to-phase combination open or very high | Corroded/open phase | Phase/connector repair or replacement |
| Low phase-to-ground insulation | Winding compromised | Likely thruster replacement |
| White/green/blue residue | Salt/corrosion confirmed | Cleaning/neutralization + inspection of associated electronics |
| Hot spot over battery bay | Thermal risk | Do not use battery; store safely and inspect |

---

## 11. Recommended route

1. Do not power the ROV on again.
2. Keep the battery removed and in a safe place.
3. Complete non-destructive diagnostics.
4. Complete missing unit data.
5. Send English RMA emails with attachments.
6. Ask about shipping from Argentina, Li-ion battery/IATA, customs, warranty, and re-certification.
7. Use temporary export and keep purchase invoice/receipts.
8. At authorized service: disassembly, repair, salt cleaning, leak/vacuum test, and re-certification before diving.

---

## 12. Service centers considered

| Center | Region | Contact |
|---|---|---|
| FIFISH E.U. Service Center, Hochheim, Germany | Europe | `support@qysea.com` / `info@vitechgmbh.com` |
| Blue Skies Drones, Centralia, WA, USA | North America | `support@blueskiesdroneshop.com` |
| Dominion Drones, Portsmouth, VA, USA | USA | `sales@dominiondrones.com` |

Note: confirm international shipping acceptance before sending the unit.

---

## 13. Critical warnings

- Li-ion + saltwater = thermal risk.
- Do not run power-on test cycles.
- Do not run thrusters in air for more than 30 seconds.
- Do not open the pressure hull without authorization.
- O-rings only with compatible silicone grease; never petroleum-based grease.
- A hair, grain of sand, or salt residue in the O-ring groove can cause a leak at depth.
- After any repair, require leak/vacuum testing or re-certification before submerging.

---

## 14. Available deliverables

- Full technical diagnosis.
- RMA emails ready to complete and send.
- English case summary for attachment.
- Official manuals and extracted text.
- Referral and international shipping route.

---

## 15. Immediate next steps

1. Replace bracketed placeholders in the emails.
2. Complete the diagnosis measurement template.
3. Choose a service center based on RMA response, cost, timeline, and logistics.
4. Prepare packaging and temporary export documentation.
5. Confirm Li-ion battery handling according to IATA and service-center guidance.

---

## 16. Sources and references

Local files:
- `diagnostico-fifish-v6-expert.md`
- `case-summary-en.pdf`
- `correos-reparacion-centros.md`
- `correos-reparacion-centros-ES.md`
- `QYSEA_V6Expert_QuickStart_Manual_V2.2_EN.pdf`
- `QYSEA_V6Expert_QuickStart_Manual_V2.2_EN_extracted-text.txt`
- `QYSEA_V6Expert_Motors_Battery_Maintenance_Guide.pdf`

External references:
- QYSEA FIFISH V6 Expert: https://www.qysea.com/products/fifish-v6-expert.html
- QYSEA guides: https://www.qysea.com/support/guides/fifish-v6-expert/
- QYSEA repair: https://www.qysea.com/support/repair/
- QYSEA after-sales: https://www.qysea.com/support/after-sales/

---

## 17. Project status

Ready to complete measurements and send the RMA request.  
Main blocker: missing physical suspect-thruster data and unit details.  
Decision already made: do not open the hull locally; refer to authorized service with pressure-seal re-certification.
