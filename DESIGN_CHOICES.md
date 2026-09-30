# Live Engineering Decision & Choice Tracker
**Project:** DIY Atmospheric Mushroom Substrate Sterilizer  
**Status:** Live Tracker (Updated continuously per user choices)

---

## 1. Current Active Baseline: Final Converged Architecture

| Parameter | Final Specification | Status | Context / Justification |
| :--- | :--- | :--- | :--- |
| **Boiler Vessel** | **20L Stainless Steel Canister (Rudra / Shiner)** | **CONFIRMED** | Reuses existing vessel (₹0 new vessel cost). Sized for rapid warmup (~20 min) and compact footprint. |
| **Water Feed System** | **All-SS304 Mechanical Float Valve (Mallinath Metals / Nagdevi)** | **CONFIRMED** | Continuous micro-trickle (~45 mL/min into 15L pool; $<0.25^\circ\text{C}$ bulk temp drop). Eliminates mid-cycle refilling and supports 3–4h or 24h runs. |
| **Heating Power** | 2 kW IndoSurgical Autoclave Screw-Plug (1.25" / 1.5" BSP) | **CONFIRMED** | Single-hole mount with red high-temp silicone gasket; draws 8.7A @ 230V. |
| **Power Connection** | Push-on Autoclave Socket + Braided Cord to 16A Switched Box | **CONFIRMED** | Quick-disconnect modularity; easy boiler removal for cleaning and descaling. |
| **Primary Overpressure** | **Dual Commercial 6 mm Bore Cooker Vents + Deadweights** | **CONFIRMED** | Napier flow: $2 \times 7.32\text{ kg/h} = 14.64\text{ kg/h}$ capacity vs $3.19\text{ kg/h}$ generation ($4.6\times$ safety margin). Audible whistle alarm; ₹150–₹250 cost. |
| **Tertiary Overpressure** | 1/2" Brass Pop Valve (1.5 bar) or Fusible Blowout Plug | **CONFIRMED** | Jam-proof secondary defense if mineral scale ever clogs both gravity weights. |
| **Vacuum Implosion Defense** | Inward-Facing Brass Spring Check Valve (PTFE/Silicone) or Solar Breaker | **CONFIRMED** | Sucks open at $-0.02\text{ bar}$ upon power cutoff; stops boiler/drum crush without expensive industrial valves. |
| **Dry-Boil Safety Interlock** | **Manual-Reset Geyser Cutout (Primary Target) OR Dual KSD302 + Omron Relay (Fallback)** | **ACTIVE STRATEGY** | **Primary:** Sourcing a 120°C–130°C manual-reset switch with red pin locally (~₹60) saves ₹805 and eliminates panel relay. **Fallback:** Amazon Omron MY2N + Schneider button if not found locally. |
| **Temperature & Modulation** | **REX-C100 (Robu SKU: 1150773) + K-Type 2M Probe (Robu SKU: R191936) + SSR-40DA (Robu SKU: 43625)** | **CONFIRMED (SELECTED)** | 100 mm stainless probe monitors bag core. 12V DC pulses SSR-40DA with zero-cross PWM, throttling from 2 kW down to ~1 kW at 99°C. |
| **Cycle Timing & Grid Policy** | **Digital Countdown Timer with Battery Backup + End-of-Cycle Alarm** | **CONFIRMED (SELECTED)** | Timer runs 5h countdown. Timer NC terminal (16) energizes 220V 16mm Red Flashing Buzzer (Robu SKU: 1364622) at timeout to alert workers. |
| **Master Disconnect & Reset** | **2-Pole 25A AC Contactor + Single Red Pushbutton (NC Reset)** | **CONFIRMED (SELECTED)** | Zero green start button needed. Machine starts on master switch; recovers from blackouts automatically; single Red button clears dry-boil fault. |
| **Personnel Protection** | **Dedicated 2-Pole 25A 30mA RCCB + 16A MCB + Chassis Earth** | **CONFIRMED** | Localized leakage isolation ($<30\text{ ms}$ trip) without tripping farm distribution board. |
| **Total Project Capital Budget** | **₹20,000 Max Budget (₹14,500 Base + ₹5,500 Margin)** | **CONFIRMED BASELINE** | Direct hardware is ₹14,535. ₹5,465 margin covers drum thermal insulation, step drill tooling, logistics/freight, welder variance, and spare parts. |
| **Robu.in Core Electronics** | **KSD302(×5) + SSR-40DA + REX-C100 + K-Probe + Glands + Buzzer** | **ORDER PLACED (PROCESSING)** | **Ordered 30/09/2026. Invoice: ₹2,011.14 (Free Shipping). Delivery to Vatakara.** |
| **Amazon India Package** | **Omron Relay + Schneider Button + uxcell Box + HSKA Heatsink + BT41D5 Timer + 2kW Heater + Cable + Anabond 666** | **ORDER PLACED (SHIPPED/PROCESSING)** | **Ordered 30/09/2026. All 8 items confirmed & paid.** |
---

## 2. Chronological Decision & Pivot Log

### Record #001: Initial 24-Hour Commercial Build Concept
* **Date / Phase:** Inception (Part 1 & 2 Video Context)
* **Initial Proposal:** 20L Rudra stainless canister + 2 kW IndoSurgical element running 24 hours into a 55-gallon drum to sterilize 500–600 bags/day.
* **Status:** **SUPERSEDED**
* **Why Changed:** 2 kW element consumes $\sim 67.2\text{ L}$ in 24 hours. A 20L canister runs completely dry in under 4 hours, causing coil destruction and fire risk.

---

### Record #002: Float Valve Auto-Fill Integration
* **User Input:** *"add a float valve also. We need float valve also. And we need a small float valve because the chamber is small for the steam generator. All the safety devices include since float valve will uh ensure that it won't overflow."*
* **Decision Taken:**
  * Selected 1/2" SS304 miniature short-arm float valve ($\le 80\text{ mm}$ reach) with high-temp PTFE/silicone seal.
  * Added perforated mesh stilling well / splash baffle around float to stop boiling chatter.
  * Integrated Tier 1 Overflow Port at $H = 230\text{ mm}$ to guarantee float failure cannot flood steam line.
  * Integrated Low-Water Cutoff (LWCO) magnetic switch at $H = 100\text{ mm}$.
* **Status:** **ARCHIVED / ON-HOLD** (Fully engineered in `DETAILED_BUILD_PLAN.md` if 24h runs are ever needed in the future).

---

### Record #003: Consideration of Large 70L – 80L Tank
* **User Input:** *"i guess using a bigger 70l or 80l tank would be ideal since with that we can avoid using float valve and simplify"*
* **Engineering Evaluation:**
  * Heating 75L of cold water with 2 kW takes **$3.5\text{ to }4.0\text{ hours}$** just to reach boiling point.
  * Creates an incubation hazard ($30^\circ\text{C}\text{–}50^\circ\text{C}$) where endospores germinate before heat kills them.
  * Requires a 100L–120L vessel for proper vapor headroom ($\sim ₹10{,}000+$ cost in India; 100 kg filled weight).
* **Status:** **REJECTED** (Inefficient with a 2 kW power source).

---

### Record #004: Cycle Duration & Substrate Pivot (4–5 Hours)
* **User Input:** *"no, i plan for a simple cycle of 4-5 hrs for simple substarate only, like sawdust + small amount of bran, not heavy suplmeentaion"*
* **Decision Taken:**
  * **Cycle shortened to 4–5 hours.**
  * Water consumption plummets to **$11.2\text{ L – }14.0\text{ L}$**.
  * **Float valve is completely discarded** to simplify construction, reduce cost, and eliminate plumbing failure points.
  * Shift to batch boiler mode.
* **Status:** **ACTIVE DIRECTIVE**

---

### Record #005: 40L Canister Batch Boiler Confirmed
* **Date / Phase:** Active Choice
* **User Input:** *"40l it is"*
* **Decision Taken:**
  * Adopted standard **40-Liter Stainless Steel Milk/Dairy Canister** as the standalone steam generator.
  * **Float valve is 100% eliminated.**
  * Initial cold fill: **$22.0\text{ Liters}$**.
  * Boil-off over 5 hours: **$14.0\text{ Liters}$**.
  * Water remaining at completion: **$8.0\text{ Liters}$** ($25\text{–}30\text{ mm}$ liquid cushion submerging the 2 kW element).
  * Vapor headroom above liquid: **$>180\text{ mm}$** (prevents priming / boiling splash into steam line).
  * **Result:** 100% walk-away operation. Set timer for 5 hours, turn it on, and leave.
* **Status:** **CONFIRMED BASELINE**

---

### Record #006: Converged Final Architecture (Gemini Session 9/30/2026)
* **Context & Realizations:**
  1. **Boiler Re-use:** Sourcing an all-SS304 industrial float valve from Mallinath Metals (Nagdevi Street, Mumbai / Calicut) enables the existing 20L Rudra SS vessel to run continuously without boiling dry.
  2. **Thermal Impact of Float Feed:** Evaporating $2.8\text{ L/hr}$ consumes only $\sim 45\text{ mL/min}$. Entering a 15,000 mL boiling pool drops bulk water by $<0.25^\circ\text{C}$—zero disruption to boiling.
  3. **Napier Orifice Calculation:** A single 3 mm domestic cooker vent discharges only $1.83\text{ kg/h}$ (insufficient for $3.19\text{ kg/h}$ generation). Sizing up to **dual commercial 6 mm hotel cooker vents** gives $14.64\text{ kg/h}$ discharge capacity ($4.6\times$ margin) with clear audible warning whistles.
  4. **Low-Cost Vacuum Relief:** Inverted brass NRV (pointing inward) or rooftop solar water heater vacuum breaker provides automatic negative pressure relief for under ₹200.
  5. **Modularity & Maintenance:** Single-hole screw-plug element (1.25" / 1.5" BSP) with push-on autoclave cable allows unplugging the boiler for easy sink cleaning/descaling.
  6. **Contactor + SSR Division of Labor:**
     * Contactor coil is switched by master start/timer and interrupted by KSD301 110°C thermal disc. Operates twice per batch (zero contact wear).
     * SSR-40DA (finned heatsink) carries the PID time-proportional PWM pulses silently with zero arcing.
     * Contactor physically isolates the line if SSR ever fails shorted.
  7. **KSD301 Current Relocation:** Moving the KSD301 from the 8.7A power line to the contactor coil (A1) line reduces its current from 8.7A to $0.05\text{A}$ ($50\text{ mA}$), preventing contact welding and guaranteeing dry-boil trip.
* **Status:** **FINAL CONVERGED SPECIFICATION**
---

## 3. Comparison Matrix: Current Options for the 4–5 Hour Cycle

| Criterion | Option A: Existing 20L Canister (Modified) | Option B: 35L–40L Stainless Canister (Recommended) |
| :--- | :--- | :--- |
| **New Vessel Cost** | **₹0** (Already have Rudra 20L) | ₹2,400 – ₹3,200 (Standard 35L SS milk can) |
| **Float Valve Needed?** | **NO** | **NO** |
| **Initial Water Fill** | 12 Liters | 22 Liters |
| **Vapor Headroom** | 140 mm (Good) | 160 mm (Excellent) |
| **Warm-Up Time (to 100°C)** | **~26 minutes** | **~50 minutes** |
| **Operator Intervention** | **Needs 1 manual top-up** (5L hot water at hr 2.5) | **ZERO (100% Walk-away / set and forget)** |
| **End-of-Run Water Level** | ~3.0 L (Tightly close to element top) | ~8.0 L (Safe $25\text{ mm}$ liquid cushion over element) |
| **Risk of Dry Burn** | Moderate if operator forgets mid-cycle top-up | Near Zero (Protected by KSD301 snap-disc) |

---

## 4. Retained Mandatory Safety Baseline (Passive Protections)

Even with the float valve removed, the following passive safety interlocks remain strictly active:

1. **Overheat / Dry-Run Defense:**
   * **KSD301 $115^\circ\text{C}$ Snap-Disc Thermostat:** Clamped flush to outer boiler shell beside heating element. If water runs out, outer wall temperature jumps $>110^\circ\text{C}$; snaps open and cuts 230V power permanently until manual reset button is pressed.
2. **Overpressure Defense:**
   * **0.5 – 1.0 psi Pop Safety Relief Valve:** On boiler lid. If the steam line clogs or drum backpressures, it vents automatically.
3. **Vacuum Implosion Defense:**
   * **1/2" Anti-Vacuum Breaker:** Allows atmospheric air entry upon cycle completion, preventing can crush.
4. **Electrical Defense:**
   * **16A MCB + 30mA RCCB (GFCI)** + direct $4\text{ mm}^2$ green earth bonding to boiler and drum chassis.

---

## 5. Confirmed 40L Boiler Specifications & Port Layout

```
                        40L STAINLESS CANISTER BOILER
                        (Diameter ≈ 340 mm, Height ≈ 500 mm)

   H = 500 mm ─── [Canister Lid with Clamp Latches]
                  ├── 0.5 – 1.0 psi Pop Safety Relief Valve (Anti-burst)
                  └── 1/2" Anti-Vacuum Breaker (Anti-implosion)

   H = 440 mm ─── [1/2" Steam Outlet Ball Valve] ══════► (To 55-Gal Drum)

   H = 260 mm ─── INITIAL COLD FILL LINE: 22.0 Liters
                  │
                  │   VAPOR HEADROOM: 180 mm (Zero water carryover)
                  │
                  │   5-HOUR BOIL-OFF ZONE: 14.0 Liters Evaporated
                  │
   H = 100 mm ─── END-OF-RUN WATER LINE: 8.0 Liters Remains
                  │
                  │   LIQUID CUSHION: 25 mm above heating element
                  │
   H =  75 mm ─── Top edge of 2 kW Heating Element Coil
   H =  50 mm ─── KSD301 115°C Snap-Disc Cutoff (Glued/clamped to outer shell)
   H =  40 mm ─── 2 kW Heating Element Flange + IP65 Terminal Enclosure
   H =  20 mm ─── Sight Glass Bottom Port & 1/2" Drain Ball Valve
   H =   0 mm ─── Base (Bonded with 4 mm² Copper Earth Wire)
```

---

## 6. Revised Simplified Bill of Materials (40L Batch Setup)

| Item | Component | Spec | Est. Cost (INR) |
| :--- | :--- | :--- | :--- |
| **V-01** | 40L Stainless Canister | 40L SS304 dairy/milk canister with clamp lid | ₹2,800 – ₹3,400 |
| **H-01** | Heating Element | 2 kW 230V IndoSurgical autoclave immersion heater | (In hand) |
| **S-01** | Thermal Snap-Disc | KSD301 $115^\circ\text{C}$ 16A manual reset bimetal thermostat | ₹180 |
| **S-02** | Safety Pop Valve | 1/2" low-pressure safety valve ($0.5\text{–}1.0\text{ psi}$) | ₹400 |
| **S-03** | Vacuum Relief Breaker | 1/2" brass anti-vacuum breaker / reverse check valve | ₹350 |
| **S-04** | Sight Glass & Drain | 1/2" SS tee, high-temp silicone/glass tube, 1/2" drain valve | ₹600 |
| **E-01** | Weatherproof Terminal Box | IP65 junction box + PG13.5 gland | ₹250 |
| **E-02** | Circuit Breaker | 16A C-curve MCB + 25A 30mA RCCB | ₹1,200 |
| **P-01** | Steam Transfer Line | 1/2" ID corrugated SS flexible hose ($1.5\text{ m}$) | ₹700 |
| **P-02** | Drum Hardware | 1" ball valve + 1" internal steam diffuser cross pipe | ₹1,000 |
| **Total Added Cost** | *(Excluding existing 55-gal drum & 2 kW heater)* | | **$\approx \mathbf{₹7{,}480\text{ – }₹8{,}080}$** |