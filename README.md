# DIY Atmospheric Mushroom Substrate Sterilizer
**An industrial-grade, fail-safe atmospheric steam sterilizer engineered for commercial mushroom farms.**

![Sterilizer Setup](./Pasted%20image.png)

## Overview
* **Boiler Vessel:** 20L / 40L SS304 Stainless Steel Canister
* **Sterilization Chamber:** 180L Metal Drum (20–25 beds / 50–60 blocks per batch)
* **Heat Source:** 2 kW 230V Immersion Heating Element (8.7A draw)
* **Temperature Regulation:** REX-C100 PID Controller driving an SSR-40DA (Zero-Cross PWM)
* **Cycle Timing:** Digital Countdown Timer with Battery Backup (BlackT BT41D5, 5-hour countdown mode)
* **Dry-Boil Safety:** Dual Ceramics KSD302 135°C NC Discs (in series) + 230V Fault Lockout Relay (Omron MY2N)
* **Personnel Protection:** Dedicated 2-Pole 25A 30mA RCCB + 16A MCB + Chassis Earth Grounding
* **All-Inclusive Cost:** ~₹14,500 total (compared to ₹85,000–₹1,50,000 commercial retort units)

---

## Documentation Index
1. **[`DETAILED_BUILD_PLAN.md`](./DETAILED_BUILD_PLAN.md):** Complete engineering specifications, vertical height budgeting, wiring schematics, and full BOM.
2. **[`TRAPS_AND_FAILURE_MODES.md`](./TRAPS_AND_FAILURE_MODES.md):** 14 critical physical, electrical, and biological failure traps and their preventative countermeasures.
3. **[`BUILD_GUIDE_MALAYALAM.md`](./BUILD_GUIDE_MALAYALAM.md):** Full step-by-step fabrication and assembly manual in Malayalam (മലയാളം ഗൈഡ്).
4. **[`DESIGN_CHOICES.md`](./DESIGN_CHOICES.md):** Chronological log of all engineering decisions, pivots, and budget tracking.
5. **[`sterilizer_3d_model.html`](./sterilizer_3d_model.html):** Interactive 3D WebGL model of the sterilizer system.

---

## Electrical & Safety Architecture

```
[ 230V AC Mains Supply ]
           │
   ┌───────▼───────┐
   │ 25A 30mA RCCB │  <-- Master personnel shock protection (<30 ms trip)
   └───────┬───────┘
           │
   ┌───────▼───────┐
   │    16A MCB    │  <-- Short-circuit and overcurrent protection
   └───────┬───────┘
           │
 ┌─────────┴───────────────────────────────────────────────────────┐
 │                                                                 │
 │ [ POWER PATH: 2.5 mm² FR Copper ]               [ CONTROL PATH: 0.75 mm² Wire ]
 │                                                                 │
 ▼                                                                 ▼
Contactor Terminal L1 (Phase)                     Digital Countdown Timer (BT41D5 in 5h mode)
Contactor Terminal L2 (Neutral)                                    │
 │                                                  ┌──────────────┴──────────────┐
[ CONTACTOR MAIN CONTACTS ]                         │                             │
(Clacks ON at start, drops OFF at finish/fault)     ▼                             ▼
 │                                            REX-C100 Power (1, 2)       Timer Switched Out
 ├── Switched Phase (T1) ────────┐                  │                             │
 │                               │            PID 12V DC Out (4+, 5-)             ▼
 └── Switched Neutral (T2) ──┐   │                  │                 [ DUAL KSD302 DISCS IN SERIES ]
                             │   ▼                  ▼                 ├── Disc #1 @ H=40mm (Left of heater)
                             │ SSR-40DA AC (1, 2)   SSR-40DA DC (3+, 4-)└── Disc #2 @ H=60mm (Top of heater)
                             │ (Global HSKA Sink)   (Zero-Cross PWM)              │
                             │ (Silent 2s cycling)  (Throttles to ~1 kW)          ▼
                             │   │                                        Contactor Coil Terminal A1
                             │   │                                        Contactor Coil Terminal A2 <── Neutral
                             ▼   ▼
                  ┌──────────────────────┐
                  │ 16A 3-Pin Socket Box │
                  │ (L ── SSR, N, Earth) │
                  └──────────┬───────────┘
                             │
                             ▼
                  [ Autoclave Cable to 2 kW Element ]
```

---

## Operating Invariants
* **Grid Outage Resiliency:** Brief blackouts (<10 min) preserve heat due to the drum's 150 kg thermal mass; the battery-backed timer resumes automatically without restarting the cycle.
* **Permanent Dry-Boil Lockout:** If water runs dry and the pot reaches 135°C, the Omron relay trips and locks the contactor off permanently until manually reset via the red button.
* **Atmospheric Pressure Guarantee:** The 180L drum exhaust chimney (1") remains permanently open. Dual commercial 6 mm cooker vents on the boiler provide 4.6× mass-flow relief in the event of steam line obstruction.
* **Anti-Implosion Protection:** A 1/2" solar vacuum breaker valve prevents negative pressure collapse during cooldown.
