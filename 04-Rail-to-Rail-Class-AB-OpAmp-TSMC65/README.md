# Project 04: Rail-to-Rail Class-AB Folded-Cascode Op-Amp (TSMC 65nm 2.5V)

## Executive Summary
This design challenge presents a high-drive, rail-to-rail operational amplifier implemented in **TSMC 65 nm CMOS (2.5 V I/O devices)** capable of driving heavy resistive and capacitive loads ($R_L = 10\text{ k}\Omega$ and $2\text{ k}\Omega$ to $V_{MID}$, $C_L = 20\text{ pF}$)[cite: 4, 6]. 

The amplifier features complementary PMOS and NMOS differential input stages for complete rail-to-rail common-mode tracking ($0\text{ V}$ to $2.5\text{ V}$), a Monticelli translinear floating bias network to control a Class-AB push-pull output stage ($I_Q \approx 200\,\mu\text{A}$), and self-tracking cascode bias generators derived from a single reference current seed[cite: 4, 6].

---

## Target vs. Simulated Performance

| Metric | Design Target | Simulated (Nominal) | Performance Margin | Status |
| :--- | :---: | :---: | :---: | :---: |
| **Technology / Supply** | TSMC 65 nm / 2.5 V | 2.5 V | Compliant | Compliant[cite: 4, 6] |
| **Open-Loop DC Gain ($A_{OL}$)** | $\ge 60.0\text{ dB}$ | **77.04 dB** | $+17.04\text{ dB}$ | Pass[cite: 4, 6] |
| **Unity-Gain Frequency ($f_u$)** | $\ge 10.0\text{ MHz}$ | **10.09 MHz** | $+0.09\text{ MHz}$ | Pass[cite: 4, 6] |
| **Phase Margin (PM)** | $\ge 60.0^\circ$ ($C_L = 20\text{ pF}$) | **$66.27^\circ$** | $+6.27^\circ$ | Pass[cite: 4, 6] |
| **Gain Margin (GM)** | $\ge 10.0\text{ dB}$ | **22.85 dB** | $+12.85\text{ dB}$ | Pass[cite: 4, 6] |
| **Input Common-Mode Range** | Rail-to-Rail ($0\text{ V}$ to $2.5\text{ V}$) | $0.0\text{ V}$ to $2.5\text{ V}$ | Full Supply Window | Pass[cite: 4, 6] |
| **Minimum PM across CM Range** | $\ge 60.0^\circ$ | **$66.2^\circ$ (Mid-rail) to $89.0^\circ$ (Rails)**| Unconditionally Stable | Pass[cite: 4] |
| **Quiescent Output Current ($I_Q$)**| $200\,\mu\text{A}$ | $200.7\,\mu\text{A}$ (NMOS) / $201.1\,\mu\text{A}$ (PMOS)| $< 0.5\%$ error | Pass[cite: 4, 6] |
| **Peak Drive ($R_L = 2\text{ k}\Omega$)** | High Dynamic Boost | **$> 614\,\mu\text{A}$ Sunk / $> 615\,\mu\text{A}$ Sourced** | $> 3\times I_Q$ Boost | Pass[cite: 4] |
| **Slew Rate ($SR$)** | $\Delta V_{IN} = 1.0\text{ V}_{pp}$ | **$4.339\text{ V}/\mu\text{s}$** (Monotonic, 0% overshoot)| Well-Damped Settling | Pass[cite: 4] |
| **Dynamic Inverting Swing** | $A_v = -1.0\text{ V/V}$ ($2.0\text{ V}_{pp}$) | $-1.00\text{ V/V}$ ($0.25\text{ V}$ to $2.25\text{ V}$)| Error $< 0.2\%$, zero clipping | Pass[cite: 4, 6] |

---

## Circuit Highlights & Engineering Deep Dive

### 1. Rail-to-Rail Input Stage & Parametric Stability
* **Complementary Input Topology:** Combines an NMOS pair (active for high common mode) and a PMOS pair (active for low common mode) to eliminate input stage cutoff[cite: 4].
* **Parametric Sweep Verification ($V_{ICM} = 0\text{ V}$ to $2.5\text{ V}$ in $50\text{ mV}$ steps):**
  * **Low Rail ($0.0\text{ V} - 0.5\text{ V}$):** PMOS active, $f_u \approx 5.1\text{ MHz}$, $\text{PM} \approx 77.8^\circ$[cite: 4].
  * **Mid Rail ($0.8\text{ V} - 1.6\text{ V}$):** Both pairs active, $f_u \approx 10.09\text{ MHz}$, $\text{PM} \approx 66.2^\circ$[cite: 4].
  * **High Rail ($2.0\text{ V} - 2.5\text{ V}$):** NMOS active, $f_u \approx 5.2\text{ MHz}$, $\text{PM} \approx 76.8^\circ$[cite: 4].
  * Proved unconditional stability ($\text{PM} > 66^\circ$) across the entire $2.5\text{ V}$ rail[cite: 4].

### 2. Monticelli Translinear Class-AB Output Stage
* **The Operation:** The push-pull power FETs (M16, M17) are biased by a translinear floating battery loop (M10, M11)[cite: 4].
* **Dynamic Current Boosting:** Under nominal conditions, the output devices maintain $I_Q \approx 200\,\mu\text{A}$[cite: 4, 6]. When subjected to heavy resistive loading ($R_L = 2\text{ k}\Omega$ to $V_{MID}$):
  * At $V_{IN} = 0.2\text{ V}$, the NMOS transistor surges to **$614.2\,\mu\text{A}$** while the PMOS device is held safely in weak conduction ($89.4\,\mu\text{A}$), completely preventing crossover distortion[cite: 4].
  * At $V_{IN} = 2.3\text{ V}$, the PMOS transistor sources **$615.1\,\mu\text{A}$**[cite: 4].
* **Dynamic Range:** In closed-loop inverting configuration ($R_1 = R_2 = 20\text{ k}\Omega$), the amplifier processed a $10\text{ kHz}$, $2.0\text{ V}_{pp}$ sinusoid, delivering $A_v = -1.00\text{ V/V}$ tracking from $0.25\text{ V}$ to $2.25\text{ V}$ without gain compression or harmonic clipping[cite: 4, 6].

---

## Documentation
* [View Full Technical Report (PDF)](./Class_AB_OpAmp_Report.pdf)[cite: 4]
