# Comprehensive Engineering Build Plan: Atmospheric Mushroom Substrate Sterilizer
**System:** 20 L External Steam Boiler + Continuous Auto-Fill Float System + 55-Gallon Atmospheric Chamber  
**Target Operation:** 500–600 bags/day scaling, continuous 16–24 hr run @ 98°C–100°C (atmospheric, 0.0 psi)  
**Document Version:** 2.0 (Converged PID+SSR Architecture, 11-Tier Safety, Full BOM)  
**Critical Engineering Traps Reference:** See [`TRAPS_AND_FAILURE_MODES.md`](./TRAPS_AND_FAILURE_MODES.md) for 14 mandatory failure mode countermeasures.

---

## 1. System Architecture & Operating Principles

```
  [Mains Water / Gravity Reservoir]
                  │
          [Shut-Off Ball Valve]
                  │
          [Sediment Filter 80#]
                  │
      [Pressure Regulator / Needle Valve] (5–10 psi)
                  │
                  ▼
   ┌─────────────────────────────────────────────────────────┐
   │            20L STAINLESS CANISTER BOILER                │
   │                                                         │
   │  [Top Lid with Clamp Latches]                           │
   │  ┌────────────────────────┐                             │
   │  │ [Safety Vent / PRV]    │                             │
   │  └────────────────────────┘                             │
   │                                                         │
   │  [Steam Exit 1/2" Ball Valve] ═══════► (Insulated SS Corrugated Hose)
   │   (H = ~330 mm)                                         ║
   │                                                         ║
   │  [High-Level Overflow Port 1/2"] ────► (To Drain/Floor) ║
   │   (H = 230 mm)                                          ║
   │                                                         ║
   │  [Auto-Refill SS Mini Float Valve]                      ║
   │   (Operating Level H = 170 mm)                          ║
   │   (With internal splash baffle)                         ║
   │                                                         ║
   │  [Low-Water Cutoff Float Switch]                        ║
   │   (Trigger Level H = 100 mm)                            ║
   │                                                         ║
   │  [2 kW Immersion Heating Element]                       ║
   │   (Coil Height H = 30–75 mm)                            ║
   │   (External Snap-Disc Thermal Fuse @ 115°C)             ║
   │                                                         ║
   │  [Bottom Drain & Sight Tube Port 1/2"]                  ║
   │   (H = 15 mm)                                           ║
   └─────────────────────────────────────────────────────────┘
                                                             ║
                                                             ▼
                                     ┌────────────────────────────────┐
                                     │     55-GALLON DRUM CHAMBER     │
                                     │                                │
                                     │  [Open Vent / Exhaust Pipe]    │
                                     │   (Unrestricted Atmospheric)   │
                                     │                                │
                                     │  [Substrate Blocks / Bags]     │
                                     │   (500–600 bags batch cycle)   │
                                     │                                │
                                     │  ════════════════════════════  │
                                     │  [Perforated False Bottom 4"]  │
                                     │  ────────────────────────────  │
                                     │  [Perforated Steam Diffuser]   │
                                     │   ◄══ 1" Lower Ball Valve Port │
                                     │                                │
                                     │  [Bottom Condensate P-Trap]    │
                                     └────────────────────────────────┘
```

---

## 2. Thermodynamics, Mass Balance & Sizing

### 2.1 Water Boil-Off Rate
* **Heating Element Power:** $P = 2000\text{ W} = 2.0\text{ kW} = 7{,}200\text{ kJ/h}$.
* **Latent Heat of Vaporization ($h_{fg}$):** $2{,}257\text{ kJ/kg}$ at $100^\circ\text{C}$.
* **Sensible Heat ($\Delta h_{sensible}$):** Water enters at $\sim 25^\circ\text{C}$, heated to $100^\circ\text{C}$:
  $$\Delta h = 4.184 \times (100 - 25) = 313.8\text{ kJ/kg}$$
* **Total Enthalpy Input per kg:** $h_{total} = 2{,}257 + 313.8 = 2{,}570.8\text{ kJ/kg}$.
* **Steady-State Evaporation Rate ($\dot{m}$):**
  $$\dot{m} = \frac{7{,}200\text{ kJ/h}}{2{,}570.8\text{ kJ/kg}} \approx 2.80\text{ kg/h} \approx 2.80\text{ Liters/hour}$$

### 2.2 Consumption Across Operational Cycles
| Cycle Duration | Total Water Evaporated | Total Canister Capacity | State Without Float Valve |
| :--- | :--- | :--- | :--- |
| **4 Hours** | $11.2\text{ L}$ | $20\text{ L}$ (usable $\sim 12\text{ L}$) | **Element starts exposing; imminent dry burn** |
| **8 Hours** | $22.4\text{ L}$ | $20\text{ L}$ | **Catastrophic dry run & coil meltdown** |
| **16 Hours** | $44.8\text{ L}$ | $20\text{ L}$ | **Destroyed** |
| **24 Hours** | $67.2\text{ L}$ | $20\text{ L}$ | **Destroyed** |

**Conclusion:** A 20 L canister holds at most 10–12 L of active boiling water without foaming over into the steam pipe. Continuous automatic replenishment via a high-temperature float valve is an absolute operational requirement.

---

## 3. Vertical Level Budget & Port Geometry

The 20 L Rudra stainless canister measures approximately **360 mm in height** and **280 mm in internal diameter**. Port locations must strictly satisfy the liquid-vapor budget:

```
Height (mm)
   360 ─── Top Rim / Lid Gasket (Vessel Open Top)
   330 ─── EXISTING STEAM OUTLET: 1/2" Nipple & Ball Valve
   280 ─── Vapor Disengagement Zone (Clear headroom to prevent droplet priming)
   230 ─── CRITICAL SAFETY OVERFLOW PORT: 1/2" Bulkhead to Drain
   170 ─── NOMINAL OPERATING WATER LEVEL: Mini Float Valve Centerline
   140 ─── Normal Float Operating Band (140 mm - 170 mm = ~9 to 11 Liters)
   100 ─── LOW-WATER CUTOFF (LWCO): SS Magnetic Reed Switch (Trip threshold)
    75 ─── Top edge of 2 kW IndoSurgical Heating Element loops
    40 ─── EXISTING HEATING ELEMENT MOUNT: Centerline of Flange
    15 ─── SIGHT GLASS BOTTOM PORT / DRAIN VALVE
     0 ─── Canister Base
```

* **Vapor Disengagement Headroom ($100\text{ mm}$):** Maintains dry steam entry into the 1/2" pipe; prevents boiling surges from ejecting liquid water into the substrate drum.
* **Liquid Safety Margin ($65\text{ mm}$):** Nominal water line ($170\text{ mm}$) sits $95\text{ mm}$ above the top loop of the heating element. If the water supply drops, the boiler retains $\sim 5\text{ L}$ of reserve water before the Low-Water Cutoff triggers at $100\text{ mm}$ (which is still $25\text{ mm}$ above the heating element).

---

## 4. Float Valve Subsystem: Engineering Specification

### 4.1 Component Selection
* **Part:** 1/2" BSP / NPT Stainless Steel 304 Miniature Adjustable Float Valve.
* **Why NOT plastic / brass toilet valves:**
  1. Plastic arms, balls, and rubber flappers warp and melt in continuous $100^\circ\text{C}$ boiling steam.
  2. Standard brass toilet ball valves have long levers ($180–250\text{ mm}$), which exceed the canister's inner radius ($140\text{ mm}$).
* **Required Specifications:**
  * **Material:** 100% SS304 body, SS304 lever arm, SS304 hollow spherical float ($\varnothing 45\text{–}55\text{ mm}$).
  * **Seal Disc:** High-temperature Food-Grade Silicone or PTFE (Teflon) rated to $\ge 150^\circ\text{C}$.
  * **Arm Length:** Compact short arm ($\le 85\text{ mm}$ total reach from inner wall).
  * **Working Pressure:** Rated $0.05\text{ to }0.6\text{ MPa}$ ($0.5\text{ to }6\text{ bar}$).

### 4.2 Installation & Anti-Turbulence Baffling
Boiling water inside a 20 L vessel with 2 kW power generates severe surface froth and rolling waves. A float ball sitting directly in the boil will vibrate, chatter, and cause water splashing.

1. **Mounting Location:** Drill a $21\text{ mm}$ hole on the canister side wall at height $H = 170\text{ mm}$ (measured from canister floor to hole center). Mount with double SS washers and dual high-temp silicone O-rings.
2. **Internal Splash Baffle (Stilling Well):**
   * Fabricate a simple open-bottom, open-top cylindrical shield around the float ball using a perforated stainless sheet or a trimmed SS tumbler/cup ($70\text{ mm}$ diameter).
   * Fasten it inside the canister around the float arm.
   * *Function:* Allows water to seek its hydrostatic level while dampening bubbling waves, ensuring smooth, steady valve seating without water hammer.
3. **Inlet Pressure & Water Feed Control:**
   * Do NOT connect unfiltered high-pressure mains directly to the mini float.
   * Install an inline 80-mesh sediment strainer upstream (debris under the seat will cause continuous weeping).
   * Install a 1/2" brass needle valve or pressure reducing valve throttled to $0.5\text{–}1.0\text{ bar}$ ($7\text{–}15\text{ psi}$).

---

## 5. Comprehensive Safety Interlocks (11-Tier Defense)

### Tier 1: Emergency High-Level Overflow Drain
* **Hazard:** Float valve stuck open by debris or mechanical jam $\rightarrow$ boiler fills to top $\rightarrow$ scalding water surges through the steam line into the substrate drum, waterlogging bags.
* **Countermeasure:** Install a dedicated 1/2" stainless bulkhead fitting at $H = 230\text{ mm}$ ($60\text{ mm}$ above the float line, $100\text{ mm}$ below the steam outlet).
* **Plumbing:** Pipe out through a heat-resistant braided hose or copper tube directed down to a floor drain or overflow collection bucket. Any excess water automatically discharges by gravity before reaching the steam exit.

### Tier 2: Low-Water Cutoff (LWCO) Electrical Interlock
* **Hazard:** External water line disconnected or float jammed shut $\rightarrow$ boiler dries out $\rightarrow$ element burns out, blows breaker, melts gaskets, fire risk.
* **Countermeasure:** Install a high-temperature SS304 vertical magnetic float switch (rated $125^\circ\text{C}$, 230V capable or switching 24V/230V relay coil) mounted at $H = 100\text{ mm}$ ($25\text{ mm}$ above the heating element).
* **Operation:** Float switch wired in series with the main contactor control coil. If water drops below $100\text{ mm}$, the circuit breaks instantly, dropping the contactor and de-energizing the 2 kW element.

### Tier 3: High-Limit Thermal Cutoff Thermostat (Snap-Disc)
* **Hazard:** Electronics/relay failure or sensor calcification leading to unmitigated boil-dry.
* **Countermeasure:** Mount two ceramic bimetallic snap-disc thermostats (**Ceramics KSD302, 135°C, 16A, Normally Closed**, Robu.in SKU: `R150050`) in direct contact with the outer stainless shell mounted onto **TIG-welded external M4 stainless studs** (zero wall penetrations), secured with M4 locknuts and thermal paste.
* **Operation:** Water keeps the wall at ~100°C. If water drops and dry boiling occurs, wall temperature spikes past 135°C within 25–35 seconds. The KSD302 snaps open, cutting line power to the contactor coil (A1).

### Tier 4: Redundant Overpressure Venting (Napier-Sized Commercial Cooker Vents)
* **Mass Generation Rate:** $2\text{ kW}$ vaporizes water at maximum $\dot{m} = \frac{2000}{2257000} \times 3600 = \mathbf{3.19\text{ kg/h}}$.
* **Napier Orifice Sizing:** Standard 3 mm domestic kitchen vents discharge only $\approx 1.83\text{ kg/h}$ at 1 bar gauge (insufficient for full dead-blockage venting).
* **Countermeasure:**
  1. **Dual Primary Vents:** Two commercial hotel/canteen pressure cooker vent tubes ($6.0\text{ mm}$ inner bore, $28.3\text{ mm}^2$ area) with matching brass deadweights. Each flows $7.32\text{ kg/h}$ ($14.64\text{ kg/h}$ combined capacity $\approx 4.6\times$ margin over $3.19\text{ kg/h}$ generation). Provides loud audible whistle alarm if steam line restricts.
  2. **Tertiary Failsafe:** One commercial cooker alloy fusible blowout plug (or 1/2" brass pop-safety valve set to 1.5 bar) for unjammable backup.

### Tier 5: Vacuum Relief Valve (Anti-Implosion Protection)
* **Hazard:** Hot steam condenses rapidly on shutdown ($1{,}600\times$ volume collapse), pulling negative pressure that crushes thin-walled vessels.
* **Countermeasure:** Install a 1/2" solar water heater vacuum breaker valve (PTFE/silicone seat, drops open at $-0.02\text{ bar}$) or an inverted 1/2" brass spring-loaded NRV with PTFE disc pointing downward into the boiler. Sucks open instantly on negative pressure; zero steam leakage during operation.
### Tier 6: Electrical Earth Grounding & GFCI / RCCB
* **Hazard:** High-current immersion element in boiling water with metal plumbing. Element sheath micro-crack causes line voltage leakage.
* **Countermeasure:**
  * **RCCB:** Feed the circuit exclusively from a dedicated 16A, 30 mA Residual Current Circuit Breaker (RCCB / GFCI).
  * **Earth Bonding:** Drill an M6 hole in the base rim of both the 20 L canister and the 55-gal drum. Fasten heavy $4\text{ mm}^2$ green copper earth wire with star lock washers and serrated flange nuts directly to clean, paint-stripped metal. Continuity to mains earth must measure $< 0.1\text{ }\Omega$.

### Tier 7: IP65 Splash-Proof Element Terminal Enclosure
* **Hazard:** In the build video, the 230V element terminals were exposed on the vessel exterior. Condensation runoff or float splash will cause a direct phase-to-neutral or phase-to-earth flashover.
* **Countermeasure:** Enclose the element screw terminals inside a die-cast aluminum or polycarbonate IP65 electrical terminal enclosure sealed against the canister wall with high-temp RTV silicone. Cable entry via an M20 / PG-13.5 liquid-tight compression cable gland.

### Tier 8: External Borosilicate Sight Glass / Level Gauge
* **Hazard:** Farmer cannot see whether the internal float is stuck or operating normally without opening the lid and releasing live steam.
* **Countermeasure:** Connect high-temperature clear silicone or borosilicate glass tube between the lower drain tee ($H = 15\text{ mm}$) and an upper vent tee ($H = 260\text{ mm}$) on the boiler side. Level is visible at a glance from across the room.

### Tier 9: Continuous Drum Condensate Drainage
* **Hazard:** 24 hours of steam generates $40\text{–}60\text{ L}$ of condensed liquid inside the 55-gallon drum. If bottom port is closed, bottom bags drown in tepid stagnant water.
* **Countermeasure:** Leave the bottom 1-inch drain valve cracked open into a P-trap drain hose. Steam is retained while excess liquid condensation exits continuously.

### Tier 10: Drum False Floor Clearance
* **Countermeasure:** Weld or assemble a rigid 304 SS or heavy expanded steel mesh rack standing on minimum $100\text{ mm}$ ($4\text{ in}$) legs inside the drum base. Bags rest high and dry above the steam diffuser and condensate line.

### Tier 11: Lid Steam Exhaust Vent (Unrestricted)
* **Countermeasure:** The 55-gallon drum lid clamp ring MUST NOT hermetically seal the drum under pressure. The lid must have a dedicated $25\text{ mm}$ ($1\text{ in}$) chimney/vent pipe elbow facing downward. Steam must visibly plume freely throughout the 24-hour cycle.

---

## 6. Complete Bill of Materials (BOM) & Sourcing

### 6.1 Mechanical, Plumbing & Boiler Components
| Item # | Component Description | Specifications / Size | Qty | Est. Cost (INR) | Sourcing / Search Keyword |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **M-01** | SS Milk Canister / Boiler Vessel | 20 L Rudra / Shiner SS304 with clamp lid | 1 | ₹2,100 | Sourced locally (Dairy / steel dealer) |
| **M-02** | 180L Drum (Sterilization Chamber) | 180 L open-top metal drum with lid and lock-ring | 1 | ₹500 | Sourced locally (Vatakara scrap/barrel market) |
| **M-03** | Immersion Heating Element | 2 kW 230V autoclave screw-plug heater (1.25" or 1.5" BSP) | 1 | ₹900 – ₹1,200 | IndoSurgical / Amazon |
| **M-04** | **Miniature All-SS Float Valve** | **1/2" BSP SS304 body, arm, and hollow ball, PTFE/silicone seal** | **1** | **₹650 – ₹900** | Mallinath Metals (Nagdevi, Mumbai) / Calicut |
| **M-05** | **Sight Glass cum Boiler Drain** | **2× 1/2" SS Tees + 1/2" SS Ball Valve + 2× Barbs + Clear Tube** | **1 set** | **₹650 – ₹850** | Calicut (Cherooty Rd) |
| **M-06** | Steam Transfer Line | 1/2" all-metal corrugated SS flexible hose (1.5 m, female BSP swivel) | 1 | ₹200 – ₹350 | Geyser corrugated pipe (pure metal, no PVC core) |
| **M-07** | **Dual Commercial Cooker Vents** | **2× Commercial 6 mm bore cooker vent spindles + brass weights** | **2 sets** | **₹180 – ₹250** | Local utensil / stove repair shop (Vatakara) |
| **M-08** | Cooker Fusible Safety Plug | Threaded alloy backup blowout plug | 1 | ₹30 – ₹50 | Local stove repair shop |
| **M-09** | Vacuum Breaker Valve | 1/2" solar water heater vacuum valve or inverted SS/brass NRV (PTFE) | 1 | ₹180 – ₹350 | Solar equipment dealer / Mallinath Metals |
| **M-10** | **Drum Steam & Drain Ports** | **2× 1/2" SS Bulkhead Connectors + 2× 1/2" SS Ball Valves** | **2 sets** | **₹900 – ₹1,200** | Calicut (Cherooty Rd) |
| **M-11** | **Drum Internal Steam Diffuser** | 1/2" SS/Galvanized pipe (15 cm) + 1/2" Tee + 2 End Caps (drilled @ 45°) | 1 set | ₹300 – ₹450 | Local plumbing / fabrication |
| **M-12** | Drum False Bottom Grate | Heavy expanded metal / SS perforated sheet on 100 mm legs | 1 | ₹800 – ₹1,200 | Local metal fabrication shop |
| **M-13** | **Waterline Y-Pattern Strainer** | **1/2" BSP SS304 or Brass Y-Strainer (80-mesh SS basket)** | **1** | **₹250 – ₹350** | **Plumbing merchant (Cherooty Rd)** |
| **M-14** | High-Temp Sealants & Fasteners | Red RTV silicone (Anabond 666), PTFE tape, M4/M6 hardware | 1 set | ₹300 | Local auto / hardware store |

### 6.2 Electrical, Automation & Safety Switchgear
| Item # | Component Description | Specifications / Size | Qty | Est. Cost (INR) | Sourcing / Search Keyword |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **E-01** | Master Leakage Protection (RCCB) | 2-Pole 25A 30mA DP RCCB (Havells / Schneider / L&T) | 1 | ₹1,200 – ₹1,600 | Electrical wholesale (Lohar Chawl / Calicut) |
| **E-02** | Branch Circuit Breaker (MCB) | 2-Pole 16A C-curve MCB | 1 | ₹300 – ₹450 | Local electrical shop |
| **E-03** | Upstream Master Contactor | 2-Pole 25A AC-1 contactor (230V coil, L&T / Schneider) | 1 | ₹650 – ₹850 | Electrical wholesale shop |
| **E-04** | **Solid State Relay (SSR)** | **SSR-40DA (3–32V DC to 24–380V AC 40A, Fotek)** | **1** | **₹279** | **Robu.in SKU: 43625 (Confirmed Selection)** |
| **E-05** | **Digital PID Controller & Probe** | **REX-C100 SSR 12V Output (Robu SKU: 1150773, ₹1,159) + K-Type 2M 100mm Probe (Robu SKU: R191936, ₹249)** | **1 set** | **₹1,408** | **Robu.in (Confirmed Selection)** |
| **E-06** | **Dry-Boil Thermal Cutoff Discs** | **2× Ceramics KSD302 135°C NC 16A in Series (Robu: `R150050`)** | **2 units** | **₹38 total** | **Robu.in (1oo2 Redundant Safety)** |
| **E-07** | **Digital Countdown Timer** | **Digital Timer with Battery Backup (Blackt BT41D5, 5h mode)** | **1** | **₹800 – ₹1,100** | **Amazon / Electrical Counter** |
| **E-08** | Modular Switched Socket Box | 16A 3-pin heavy-duty modular surface socket box + 25A switch | 1 | ₹250 – ₹350 | Local electrical shop |
| **E-09** | **Timeout Alarm Buzzer** | **Red AC/DC 220V 16mm AD16-16SM Flashing Buzzer/LED** | **1** | **₹79** | **Robu.in SKU: 1364622 (Confirmed Selection)** |
| **E-10** | **Fault Lockout Relay & Base** | **230V AC 8-Pin Relay (Omron MY2N) + DIN Socket (PYF08A)** | **1 set** | **₹120 – ₹180** | **Local electrical shop** |
| **E-10b**| **Dry-Boil Fault Reset Button** | **Single 22mm Red Momentary Pushbutton (Normally Closed 1 NC)** | **1** | **₹80 – ₹120** | **Local electrical shop (Schneider / generic)** |
| **E-11** | Autoclave Power Cable | Push-on round ceramic/bakelite socket with spring clips to 16A plug | 1 | (In hand / ₹200) | Autoclave spares counter |
| **E-12** | Enclosure & Wiring | 4–6 way IP65 DIN rail box, 2.5 mm² FR copper wire, 4 mm² earth wire | 1 set | ₹600 – ₹800 | Local electrical store |

**Total Project Capital Budget Ceiling:** **₹20,000 Max Budget** (Base Hardware: **₹14,535** + Contingency Margin: **₹5,465** for drum thermal insulation, step drill bits, logistics, welding variance, and spare element). Commercial 180L autoclave equivalent is ₹85,000+.
**Robu.in Package Status:** **ORDER PLACED & PROCESSING (30/09/2026, ₹2,011.14 - Free Shipping)**. Includes KSD302(×5), SSR-40DA, REX-C100, K-Type Probe, PG-7, PG-13.5, and 220V Buzzer.  
**Amazon India Package Status:** **ORDER PLACED (30/09/2026, ₹3,815.00)**. Includes BT41D5 Timer, Omron MY2N Relay + Base, Schneider XB5AA42N Button, uxcell Box, Global HSKA Heatsink, IndoSurgicals 2kW Heater, Cable, and Anabond 666 RTV.

---

## 7. Electrical & Safety Interlock Schematic

```
                230V AC 50Hz Mains Supply
             (Phase: L, Neutral: N, Earth: PE)
                           │
              ┌────────────▼────────────┐
              │ 2-Pole 25A 30mA DP RCCB │  <-- Local shock & leakage trip (<30 ms)
              └────────────┬────────────┘
                           │
              ┌────────────▼────────────┐
              │     2-Pole 16A MCB      │  <-- Overcurrent / short-circuit trip
              └────────────┬────────────┘
                           │
     ┌─────────────────────┴─────────────────────────────────────┐
     │                                                           │
 [ POWER PATH: 2.5 mm² FR Copper ]               [ CONTROL PATH: 0.75 mm² Wire ]
     │                                                           │
     ▼                                                           ▼
 Contactor Pole L1                                        PID Power (L) ──> Neutral (N)
 Contactor Pole L2 <── Neutral                                   │
     │                                                    PID SSR Output (12V DC)
 [ CONTACTOR MAIN CONTACTS ]                                     │ (Terminals 3/4)
 (Clacks ON once at start, OFF at end/emergency)                 ▼
     │                                                    SSR-40DA DC Input (3+, 4-)
     ├── Switched Phase (T1) ────────┐                           │
     │                               │                    [ PID Soak Countdown Timer ]
     └── Switched Neutral (T2) ──┐   │                           │
                                 │   ▼                           ▼
                                 │ SSR-40DA AC Load (1, 2)  Contactor Coil Interlock:
                                 │ (Zero-Cross PWM Pulse)        │
                                 │   │                      Timer Switched Run Output (NO)
                                 │   │                           │
                                 │   │           ┌───────────────┴─────────────────────────┐
                                 │   │           │ [ AUTO-RESUME PATH: Normal & Blackouts ]│
                                 │   │           │ Relay R1 NC Contact (Pins 9 to 1)       │
                                 │   │           └───────────────┬─────────────────────────┘
                                 │   │                           ▼
                                 │   │             [ DUAL KSD302 135°C DISCS IN SERIES ]
                                 │   │             ├── Disc #1 @ H=40mm (Left of heater)
                                 │   │             └── Disc #2 @ H=60mm (Top of heater)
                                 │   │                           │
                                 │   │                           ▼
                                 │   │                    Contactor Coil Terminal A1
                                 │   │                    Contactor Coil Terminal A2 <── Neutral
                                 │   │                           ▲
                                 │   │           ┌───────────────┴─────────────────────────┐
                                 │   │           │ [ FAULT LATCH LOOP: Dry-Boil Lockout ]  │
                                 │   │           │ Phase ──► [ RED RESET BUTTON (1 NC) ]   │
                                 │   │           │             │                           │
                                 │   │           │   Relay R1 Coil (13) + NO Latch (12-8)  │
                                 │   │           └─────────────────────────────────────────┘
                                 │   │
                                 │   │                      Timer Timeout Output (NC)
                                 │   │                           │
                                 │   │                           ▼
                                 │   │                    [ AD16-22SM 230V BUZZER / LED ]
                                 │   │                    (BEEPS & FLASHES on cycle complete!)
                                 │   │
                                 ▼   ▼
                      ┌─────────────────────────┐
                      │   16A 3-Pin Socket Box  │
                      │   (L ── SSR Out, N, E)  │
                      └────────────┬────────────┘
                                   │
                                   ▼
                      [ Braided Autoclave Cable ]
                      (Quick-disconnect ceramic socket)
                                   │
                                   ▼
                     ┌───────────────────────────┐
                     │ 2 kW Screw-Plug Element   │
                     └───────────────────────────┘
                                   │
                       (Chassis Ground PE Lead)
                                   │
 ══════════════════════════════════╧════════════════════════════════════════
 Solid Earth Ground Bus (Bonded with 4 mm² Green Copper to Canister & Drum)
```

### Operating & Failsafe Sequence
1. **Startup:** Master switch closes. Contactor pulls in **once** with a solid *clack*. 230V is presented to SSR input.
2. **Modulation:** PID monitors drum PT100 probe. PID rapidly pulses the SSR-40DA via zero-cross DC PWM (2-second time window). Contactor sits 100% stationary (zero contact wear).
3. **Steam Equilibrium:** Once drum reaches 99°C, PID dials back duty cycle to ~50% (~1 kW average), maintaining rolling steam while conserving electricity and water.
4. **Normal Finish:** PID soak countdown expires $\rightarrow$ cuts signal $\rightarrow$ contactor drops open $\rightarrow$ 16A socket dead.
5. **Dry-Boil Emergency (Float Water Supply Cut):** Boiler metal wall climbs above 100°C. At 110°C, KSD301 snaps open $\rightarrow$ cuts 50 mA coil current $\rightarrow$ contactor instantly disconnects mains before element burns out.
6. **Semiconductor Failure Failsafe (SSR Shorted Closed):** If SSR triac fails in permanent short $\rightarrow$ boiler overheats $\rightarrow$ KSD301 trips contactor upstream $\rightarrow$ physical air gap cuts power.
7. **Shock Protection:** Any moisture or sheath degradation $>30\text{ mA}$ trips RCCB in $<30\text{ ms}$.

---

## 8. Step-by-Step Fabrication & Assembly Plan

### Step 1: Boiler Port Layout & Hole Preparation
1. Clean and degrease the 20 L stainless canister.
2. Mark the following hole centers on the canister wall:
   * **Overflow Port:** $H = 230\text{ mm}$ from base (drill $\varnothing 21\text{ mm}$ with step bit).
   * **Mini Float Valve Port:** $H = 170\text{ mm}$ from base, offset $90^\circ$ around circumference from heating element (drill $\varnothing 21\text{ mm}$).
   * **LWCO Float Switch Port:** $H = 100\text{ mm}$ from base, vertically aligned or offset from float (drill $\varnothing 12\text{ mm}$ or per switch spec).
   * **Sight Glass Bottom Port:** $H = 25\text{ mm}$ from base (drill $\varnothing 21\text{ mm}$).
3. Deburr all drilled holes with a half-round file and sand edges smooth with an angle grinder disc.

### Step 2: Float Valve & Anti-Turbulence Baffle Installation
1. Thread the 1/2" SS mini float valve through the $H = 170\text{ mm}$ hole.
2. Fit food-grade silicone washers on both inside and outside faces, with thin coat of high-temp RTV silicone. Tighten the exterior brass/SS locknut firmly with a wrench.
3. Fabricate a 304 SS mesh shield ($70\text{ mm}$ diameter cylinder, open top and bottom) and mount it over the float ball inside the canister using SS wire ties or an inner bracket.
4. Calibrate the float arm: Bend or adjust the thumbscrew so that the valve shuts completely off when the liquid level reaches exactly $170\text{ mm}$.

### Step 3: High-Level Overflow & Sight Tube Plumbing
1. Install the 1/2" SS tank connector at $H = 230\text{ mm}$ with silicone gaskets. Thread a 1/2" brass barb or compression fitting on the outside.
2. Attach a reinforced heat-resistant silicone hose ($12\text{ mm}$ ID) to the overflow barb and route it down to a floor drain or drain pan.
3. At the lower $H = 25\text{ mm}$ port, install a 1/2" SS tee. The branch feeds the bottom of the transparent sight tube; the run feeds a 1/2" ball valve for boiler draining and descaling.
4. Connect the top of the sight tube to an upper vent port (or tee off the overflow fitting) to equalize vapor pressure.

### Step 4: Heating Element & Protective Terminal Box Mounting
1. Re-inspect the 2 kW IndoSurgical element installed in Part 2.
2. Ensure the inner rubber washer and outer red vulcanized fiber washer are seated flat with automobile red gasket maker.
3. Position an IP65 electrical terminal enclosure over the exterior element mount. Drill a matching center clearance hole in the back of the box and bolt it directly to the canister wall with silicone sealant.
4. Wire the element terminals using $2.5\text{ mm}^2$ fiberglass-sleeved heat-resistant wire through the PG-13.5 gland.
5. Bolt the KSD301 $115^\circ\text{C}$ thermal switch flush against the exterior stainless steel wall adjacent to the element using thermal paste and a spring-loaded bracket.

### Step 5: Drum Chamber Integration & Steam Distribution
1. Position the 55-gallon drum on sturdy cinder blocks or an angle-iron stand $150\text{ mm}$ off the floor.
2. In the bottom 1-inch port, thread a 1-inch SS nipple and ball valve.
3. Inside the drum, connect a horizontal 1-inch pipe running to the center, terminating in a cross or T-manifold. Drill $4\text{ mm}$ steam distribution holes every $50\text{ mm}$ along the bottom edge of the pipes (facing downward at $45^\circ$ to prevent condensate blockage).
4. Install the $100\text{ mm}$ high false bottom steel grate over the diffuser.
5. Connect the boiler steam outlet (upper 1/2" ball valve) to the drum inlet (lower 1" ball valve) using the 1/2" braided stainless steel corrugated hose ($1.5\text{ m}$). Slope the hose continuously upward toward the drum to ensure any line condensation drains naturally.
6. Fit the drum lid with a 1-inch open exhaust elbow and insert a dial thermometer through an airtight grommet into the upper vapor space.

---

## 9. Commissioning, Calibration & Safety Validation Protocol

Before loading substrate bags for a full 24-hour run, execute the following cold and hot tests:

| Test ID | Procedure | Acceptance Criteria | Pass/Fail |
| :--- | :--- | :--- | :--- |
| **TEST-01: Earth Continuity** | Measure resistance between mains plug ground pin and both the 20 L canister shell and 55-gal drum. | Resistance must be $< 0.1\text{ }\Omega$. | [ ] |
| **TEST-02: Cold Hydrostatic Fill** | Turn on mains water line with needle valve half open. Observe boiler filling. | Float valve stops fill cleanly at $H = 170\text{ mm}$ ($\pm 5\text{ mm}$). Zero leaks from gaskets. | [ ] |
| **TEST-03: Forced Overflow Proof** | Manually push down float arm to simulate stuck valve; allow continuous fill. | Water rises to $230\text{ mm}$ and discharges freely out overflow. Water NEVER reaches $330\text{ mm}$ steam port. | [ ] |
| **TEST-04: LWCO Electrical Trip** | With boiler full, isolate main heater power. Connect multimeter buzzer to LWCO circuit. Drain water via bottom valve. | LWCO switch opens and continuity buzz stops at $H = 100\text{ mm}$ ($25\text{ mm}$ above heating element). | [ ] |
| **TEST-05: Unrestricted Steam Flow** | Power 2 kW element with water at $170\text{ mm}$. Observe time to boil and steam entry into drum. | Vigorous boiling begins in 18–22 min. Steam flows freely through hose into drum without whistling/backpressure. | [ ] |
| **TEST-06: 4-Hour Steady-State Soak** | Run system continuously for 4 hours. Inspect sight glass and water feed. | Float valve automatically tops up small pulses of water. Water line remains steady at $165\text{–}175\text{ mm}$. | [ ] |

---

## 10. Standard Operating Procedure (SOP) for 24-Hour Sterilization

1. **Pre-Flight Checks:**
   * Verify bottom drain valve of boiler is closed.
   * Open mains water supply valve; verify sight glass shows water at nominal mark ($170\text{ mm}$).
   * Confirm drum lid exhaust vent is completely unobstructed.
   * Crack drum bottom condensate drain valve open $10^\circ$ ($2\text{–}3\text{ mm}$ gap).
2. **Loading:**
   * Load substrate bags onto the false bottom in staggered rows, leaving $20\text{ mm}$ gaps between bags for steam circulation.
   * Place the control thermocouple probe inside the center core of the densest central bag.
   * Secure drum lid with the lever locking ring (do not clamp hermetically tight).
3. **Initiation:**
   * Switch on the main electrical isolator and press the Start button on the contactor controller.
   * Monitor boiler for 20 minutes until steam exhaust begins discharging from drum lid.
4. **Sterilization Soak (24 Hours):**
   * Confirm top exhaust thermometer reaches $99^\circ\text{C}\text{–}100^\circ\text{C}$.
   * Start 24-hour sterilization countdown timer.
   * Periodically check sight glass during daily rounds; water level must remain fixed at $170\text{ mm}$.
5. **Cooldown & Unloading:**
   * At timer expiration, contactor cuts power automatically.
   * Allow chamber to sit undisturbed for 8–12 hours until core temperature drops below $40^\circ\text{C}$ before opening lid to inoculate. Vacuum breaker prevents drum collapse.