# Project 03: Self-Biased Sub-1V Bandgap Reference (TSMC 65nm 1.2V)

## Executive Summary
This design challenge presents the realization of a low-power, sub-1V bandgap voltage reference (BGR) implemented in **TSMC 65 nm CMOS** operating from a $1.2\text{ V}$ supply[cite: 5, 7]. Traditional voltage-mode bandgap circuits ($\approx 1.25\text{ V}$) fail under modern sub-micron supplies[cite: 5]. This design uses the **Banba current-mode architecture**, converting PTAT and CTAT voltages into currents to generate a zero-TC reference of $V_{REF} = 800.0\text{ mV}$[cite: 5].

The design features a custom self-biased five-transistor OTA utilizing thin-oxide transistors to resolve cold-temperature saturation bottlenecks, an analytical fix for large-signal startup loop chattering, and full physical integration using TSMC 65 nm PDK passives (`rnpolys` and `mimcap_sin`) verified across 25 PVT corner permutations[cite: 5].

---

## Target vs. Simulated Performance

| Performance Parameter | Target Specification | Behavioral OTA | Full PDK Passives (Post-Tuning) | Status |
| :--- | :---: | :---: | :---: | :---: |
| **Process Node / Supply** | TSMC 65 nm / 1.2 V | 1.2 V | 1.2 V | Compliant[cite: 5, 7] |
| **Output Reference ($V_{REF}$)** | 800.0 mV | 800.07 mV | 800.34 mV (Nominal) | Pass[cite: 5, 7] |
| **Total Quiescent Current** | $< 10.0\,\mu\text{A}$ across PVT | $6.0\,\mu\text{A}$ | **$7.782\,\mu\text{A}$ (Worst-case: $7.9\,\mu\text{A}$)** | Pass[cite: 5, 7] |
| **Temperature Range** | $-40^\circ\text{C}$ to $125^\circ\text{C}$ | $-40^\circ\text{C}$ to $125^\circ\text{C}$ | $-40^\circ\text{C}$ to $125^\circ\text{C}$ | Pass[cite: 5, 7] |
| **Temperature Drift ($\Delta V_{REF}$)**| $< 1.0\text{ mV}$ benchmark | 0.84 mV | **0.31 mV (TT) / 0.61 mV (Worst)** | Pass[cite: 5] |
| **Temperature Coefficient** | — | — | **$2.35\text{ ppm}/^\circ\text{C}$** | Excellent[cite: 5] |
| **Corner Voltage Spread** | — | 2.13 mV | 1.60 mV ($\pm 0.10\%$ global envelope) | Pass[cite: 5] |
| **Loop Stability (PM)** | $\ge 60^\circ$ | Behavioral | **$70.77^\circ\text{ to }71.69^\circ$ (All 25 Corners)** | Pass[cite: 5, 7] |
| **Startup Behavior** | Monotonic, Zero Leakage | Ideal | Monotonic, zero steady-state leakage | Pass[cite: 5] |

---

## Circuit Highlights & Engineering Deep Dive

### 1. Self-Biased OTA Headroom Optimization (Thin-Oxide Integration)
* **The Problem:** The core nodes establish a common-mode level of $V_{CM} \approx V_{EB0} \approx 660\text{ mV}$[cite: 5]. Standard thick-oxide devices (`nch_25_mac`) have $V_{TH} \approx 650-700\text{ mV}$, driving the tail current mirror into cutoff at cold temperatures ($-40^\circ\text{C}$) due to negative threshold temperature coefficients[cite: 5].
* **The Solution:** Implemented the differential input pair using thin-oxide `nch_mac` transistors ($V_{TH} \approx 320\text{ mV}$)[cite: 5]. This elevates the tail node to $V_{tail} \approx 399\text{ mV}$, providing abundant $V_{DS}$ headroom to maintain deep saturation across all PVT corners[cite: 5].

### 2. Diagnosis & Stabilization of Startup Loop Chatter
* **The Phenomenon:** During slow $1.0\text{ ms}$ power-up supply ramps ($0\text{ V} \rightarrow 1.2\text{ V}$), the initial circuit exhibited high-frequency switching chatter and multiple voltage spikes between $V_{DD} = 0.45\text{ V}$ and $0.65\text{ V}$[cite: 5].
* **Root-Cause Analysis:** The startup pull-up device injected current into the core, causing $V_{REF}$ to rise and prematurely trigger the NMOS sensor before the OTA achieved sufficient operational headroom[cite: 5]. The core collapsed, the sensor deactivated, and the loop cycled regeneratively[cite: 5].
* **Circuit Fix:** Added a localized $C_{damp} = 200\text{ fF}$ damping capacitor at the startup sensing gate[cite: 5]. This low-pass filtered the regenerative switching node, guaranteeing a smooth, monotonic startup trajectory to $800.0\text{ mV}$ without consuming static power[cite: 5].

### 3. Migration to Real TSMC 65nm Passives (`rnpolys` & `mimcap_sin`)
* Replaced ideal resistors with physical silicided polysilicon resistors (`rnpolys`) using a constant $W=600\text{ nm}$ grid to ensure lithographic ratio matching[cite: 5].
* Due to parasitic substrate coupling and non-zero temperature coefficients ($TC_1, TC_2$), $R_{PTAT}$ was re-tuned from $160\text{ k}\Omega$ to $155.06\text{ k}\Omega$ and $R_{TUNE}$ to $748.02\text{ k}\Omega$[cite: 5].
* Implemented the dominant pole using a $5.0\text{ pF}$ Metal-8 MIM capacitor (`mimcap_sin`), maintaining $\text{PM} > 70.7^\circ$ across all 25 corners[cite: 5].

---

## Documentation
* [View Full Technical Report (PDF)](./BGR_Design_Challenge_Report.pdf)[cite: 5]
