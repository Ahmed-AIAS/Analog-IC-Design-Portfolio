# Project 02: Fully-Differential Folded-Cascode OTA (GF180MCU 2.5V)

## Executive Summary
This project details the design and optimization of a fully-differential folded-cascode OTA implemented in the **GlobalFoundries 180 nm MCU (2.5 V)** process[cite: 2, 8]. The circuit is configured in an inverting capacitive feedback network ($C_{in} = 2\text{ pF}$, $C_f = 1\text{ pF}$, $A_{cl} = 2\text{ V/V}$) driving a $500\text{ fF}$ load[cite: 2, 8]. 

Key design elements include a transistor-level Common-Mode Feedback (CMFB) circuit utilizing an 80/20 tail current-split architecture to eliminate dynamic latch-up risks, split-ratio current optimization ($S = I_{B1}/I_{B2} = 2$) to clear settling bottlenecks without power penalties, and an on-chip self-tracking wide-swing cascode bias generator[cite: 2].

---

## Target vs. Simulated Performance

| Metric | Specification | Baseline ($S=1$) | Optimized ($S=2$) | Status |
| :--- | :---: | :---: | :---: | :---: |
| **Technology / Supply** | GF180MCU / 2.5 V | 2.5 V | 2.5 V | Compliant[cite: 2, 8] |
| **Closed-Loop Gain ($A_{cl}$)** | $\ge 2.0\text{ V/V}$ (6.02 dB) | 2.003 V/V | 2.003 V/V | Pass[cite: 2, 8] |
| **DC Loop Gain ($LG_{DC}$)** | $\ge 60.0\text{ dB}$ | 61.31 dB | 61.31 dB | Pass[cite: 2, 8] |
| **Closed-Loop Bandwidth** | — | 7.71 MHz | 9.45 MHz | Enhanced[cite: 2] |
| **Differential Phase Margin** | $\ge 70^\circ$ | $89.0^\circ$ | $84.2^\circ$ | Pass[cite: 2, 8] |
| **CMFB Phase Margin** | $\ge 70^\circ$ | $87.9^\circ$ | $87.9^\circ$ | Pass[cite: 2, 8] |
| **1% Settling Time ($t_s$)** | $\le 100\text{ ns}$ (100 mV step) | **110.8 ns (FAIL)** | **84.92 ns** | **PASS**[cite: 2, 8] |
| **Differential Output Swing** | $\ge 1.2\text{ V}_{pk-pk}$ | $1.82\text{ V}_{pk-pk}$ | $1.82\text{ V}_{pk-pk}$ | Pass[cite: 2, 8] |
| **CM Input Range (Low / High)** | $\le 0.0\text{ V}$ / $\ge 1.0\text{ V}$ | $-0.49\text{ V}$ / $1.42\text{ V}$ | $-0.49\text{ V}$ / $1.42\text{ V}$ | Pass[cite: 2, 8] |
| **Auxiliary Bias Current** | $\le 5.0\,\mu\text{A}$ | — | $3.934\,\mu\text{A}$ ($9.84\,\mu\text{W}$) | Pass[cite: 2, 8] |

---

## Circuit Highlights & Engineering Deep Dive

### 1. Transient Settling Bottleneck & Split Ratio Optimization ($S=2$)
* **The Problem:** Under equal branch currents ($I_{B1} = I_{B2} = 10\,\mu\text{A}$, $S=1$), the OTA satisfied DC gain and phase margin but failed the dynamic 1% settling specification ($t_s = 110.8\text{ ns} > 100\text{ ns}$)[cite: 2].
* **The Analytical Remedy:** Leveraging the framework from *Dr. Hesham Omran's research on optimum folded-cascode split ratios*, the $40\,\mu\text{A}$ core current was reallocated with $S = I_{B1}/I_{B2} = 2$[cite: 2, 8]:
  * Input branch current: $I_{B1} = 13.33\,\mu\text{A}$ ($+33\%$)[cite: 2]
  * Cascode branch current: $I_{B2} = 6.67\,\mu\text{A}$ ($-33\%$)[cite: 2]
* **Result:** Total static power dissipation remained identical, while input transconductance jumped from $163.8\,\mu\text{S}$ to $217.5\,\mu\text{S}$ ($+32.8\%$), cutting the 1% settling time to **$84.92\text{ ns}$** (15 ns safety margin)[cite: 2].

### 2. Transistor-Level CMFB with 80/20 Tail Split
* **Architecture:** Utilizes PMOS source-follower level shifters ($|V_{GSP}| \approx 845\text{ mV}$) and $100\text{ k}\Omega$ sensing resistors driving an NMOS differential error amplifier[cite: 2].
* **Headroom Centering:** The output common-mode target was re-centered from $1.25\text{ V}$ to $V_{REF} = 0.95\text{ V}$ to provide symmetric $\pm 600\text{ mV}$ single-ended compliance between upper buffer triode limits and bottom sink saturation[cite: 2].
* **80/20 Anti-Starvation Split:** Sizing 100% of the tail current to the CMFB error amplifier risks startup latch-up[cite: 2]. Partitioning the tail into an $80\%$ fixed quiescent current ($16\,\mu\text{A}$) and a $20\%$ dynamic trimming current ($4\,\mu\text{A}$) locked steady-state $V_{OCM}$ at $951.4\text{ mV}$ ($1.4\text{ mV}$ error) with guaranteed startup robustness[cite: 2].
* **The 30% Bandwidth Rule:** The CMFB loop crossover frequency was sized to $f_{c,CM} = 1.975\text{ MHz}$, exactly $28.57\% \approx 30\%$ of the differential crossover ($f_{c,diff} = 6.912\text{ MHz}$), optimizing power while eliminating $>95\%$ of common-mode disturbances within 3 time constants[cite: 2].

### 3. Integrated Wide-Swing Cascode Bias Generator
* Replaced ideal voltage sources with complementary self-tracking cascode bias generators ($V_{CASCP} = 1.301\text{ V}$, $V_{CASCN} = 1.109\text{ V}$)[cite: 2].
* Enforces $V_{SD} = I_{bias} \cdot R$, maintaining current mirror saturation margins ($V_{margin} \approx 125\text{ mV}$) across PVT at a total static draw of only $3.93\,\mu\text{A}$ ($9.84\,\mu\text{W}$)[cite: 2].

---

## Documentation
* [View Full Technical Report (PDF)](./Mini_Project_2_Report.pdf)[cite: 2]
