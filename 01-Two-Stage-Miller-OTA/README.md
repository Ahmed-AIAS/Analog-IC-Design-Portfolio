# Project 01: Two-Stage CMOS Miller Operational Transconductance Amplifier (OTA)

## Executive Summary
This project presents the systematic design, sizing, and characterization of an unbuffered two-stage Miller-compensated OTA driving a heavy capacitive load ($C_L = 5\text{ pF}$) under a 1.8 V supply rail[cite: 3, 9]. Sized via the $g_m/I_D$ methodology using the ADT Sizing Assistant, the amplifier satisfies strict static gain error ($\le 0.05\%$), CMRR ($\ge 74\text{ dB}$), slew rate ($\ge 5\text{ V}/\mu\text{s}$), and input common-mode range constraints[cite: 3, 9]. 

An advanced PVT-tracking lead compensation network based on the Johns & Martin methodology was implemented to replace the conventional fixed nulling resistor, maintaining closed-loop stability across an industrial temperature range ($-40^\circ\text{C}$ to $125^\circ\text{C}$)[cite: 3].

---

## Target vs. Simulated Performance

| Metric | Target Specification | Hand Calculation | Spectre Simulation | Status |
| :--- | :---: | :---: | :---: | :---: |
| **Supply Voltage ($V_{DD}$)** | 1.8 V | 1.8 V | 1.8 V | Compliant[cite: 3, 9] |
| **Total Quiescent Current** | $\le 60\,\mu\text{A}$ | $50\,\mu\text{A}$ (core) | $49.7\,\mu\text{A}$ (core) | Pass[cite: 3, 9] |
| **Open-Loop DC Gain ($A_o$)** | $\ge 66\text{ dB}$ ($A_{vCL}$ error $\le 0.05\%$) | 76.59 dB | 76.56 dB | Pass[cite: 3, 9] |
| **Unity-Gain Frequency ($f_u$)** | $\approx 5.0\text{ MHz}$ ($t_{rise} \le 70\text{ ns}$) | 5.36 MHz | 4.92 MHz | Pass[cite: 3, 9] |
| **Phase Margin (PM)** | $\ge 70^\circ$ (No doublets) | $70.35^\circ$ | $70.4^\circ$ | Pass[cite: 3, 9] |
| **CMRR @ DC** | $\ge 74\text{ dB}$ | 77.79 dB | 77.95 dB | Pass[cite: 3, 9] |
| **Slew Rate (SR)** | $\ge 5.0\text{ V}/\mu\text{s}$ | $5.32\text{ V}/\mu\text{s}$ | $5.02\text{ V}/\mu\text{s}$ | Pass[cite: 3, 9] |
| **Small-Signal Rise Time ($t_{rise}$)** | $\le 70\text{ ns}$ | 65.26 ns (1st-order) | 46.92 ns (2nd-order) | Pass[cite: 3, 9] |
| **CMIR Low / High** | $\le 0.2\text{ V}$ / $\ge 0.8\text{ V}$ | 29 mV / 835 mV | 70 mV / 850 mV | Pass[cite: 3, 9] |
| **Output Swing** | 0.2 V to 1.6 V | Satisfied via $V^*$ sizing | Compliant | Pass[cite: 3, 9] |

---

## Circuit Highlights & Design Trade-offs

### 1. Topology Selection & Gain Allocation
* **PMOS Input Pair:** Selected to accommodate the strict CMIR-Low constraint ($V_{cm} \le 0.2\text{ V}$) down towards ground without cutting off the differential pair[cite: 3].
* **Asymmetric Gain Budgeting ($A_{v1} \approx 2A_{v2}$):** The first stage was assigned $A_{v1} \approx 63.2\text{ V/V}$ ($g_m/I_D = 13\text{ V}^{-1}$) and the second stage $A_{v2} \approx 31.6\text{ V/V}$ ($g_m/I_D = 11\text{ V}^{-1}$)[cite: 3, 9]. Allocating higher gain to the input stage suppresses second-stage thermal noise and input-referred offset[cite: 3].
* **Systematic Offset Cancellation:** To prevent output rail clamping, the $V_{GS}$ of the first-stage NMOS load ($L=560\text{ nm}$) was matched to the second-stage NMOS driver ($L=520\text{ nm}$) at $V_{GS} = 821.6\text{ mV}$, keeping the open-loop output balanced at $951.6\text{ mV}$ ($V_{DD}/2$)[cite: 3].

### 2. Johns & Martin Tracking Lead Compensation
Using a fixed nulling resistor ($R_z = 1/g_{m2} = 2.27\text{ k}\Omega$) places the RHP zero at infinity under nominal conditions, but temperature shifts cause $g_{m2}$ to drift, degrading phase margin[cite: 3]. 
* An NMOS transistor operating in deep triode was sized to replace the fixed resistor[cite: 3].
* A dedicated tracking bias stack sets $R_z g_{m2} = 1$ based purely on geometric aspect ratios[cite: 3]:
  $$\frac{R_z}{1/g_{m2}} = \frac{(W/L)_{M7}}{(W/L)_{M11}}\sqrt{\frac{(W/L)_{M9}}{(W/L)_{M10}}} = 1$$
* **Verification:** Parametric sweeps from $-40^\circ\text{C}$ to $125^\circ\text{C}$ proved that the tracking circuit holds the phase margin within $70^\circ \pm 4^\circ$, whereas an uncompensated resistor suffered severe phase degradation[cite: 3].

---

## Documentation
* [View Full Technical Report (PDF)](./Mini_Project_1_Report.pdf)[cite: 3]
