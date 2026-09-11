# Analog IC Design Portfolio

This repository documents transistor-level analog integrated circuit designs developed during the **Information Technology Institute (ITI)** intensive summer training track in **Analog Integrated Circuit Design**, supervised by **Dr. Hesham Omran**.

The projects span **GF180MCU (180 nm, 2.5 V)** and **TSMC 65 nm (1.2 V / 2.5 V)** processes, focusing on $g_m/I_D$-based sizing, multi-stage frequency compensation, closed-loop stability verification, and multi-corner PVT robustness using **Cadence Virtuoso** and **Spectre**.

---

## Projects Overview

| Project | Node / Supply | Architecture Highlights | Key Performance Highlights |
| :--- | :---: | :--- | :--- |
| **[01. Two-Stage Miller OTA](./01-Two-Stage-Miller-OTA/)** | 180 nm / 1.8 V | PMOS input pair, NMOS CS stage, Johns & Martin tracking lead compensation | $A_{OL} = 76.56\text{ dB}$, $\text{PM} = 70.4^\circ$, $SR = 5.02\text{ V}/\mu\text{s}$ ($C_L = 5\text{ pF}$) |
| **[02. Fully-Diff Folded-Cascode OTA](./02-Folded-Cascode-OTA-GF180/)** | GF180MCU / 2.5 V | 80/20 dynamic CMFB split, split-ratio settling optimization ($S=2$), on-chip wide-swing bias | $1\%\text{ }t_s = 84.92\text{ ns}$, $\text{Diff PM} = 89.0^\circ$, $\text{CMFB PM} = 87.9^\circ$ |
| **[03. Self-Biased Sub-1V BGR](./03-Sub1V-Bandgap-Reference-TSMC65/)** | TSMC 65 nm / 1.2 V | Current-mode (Banba), thin-oxide self-biased OTA, chatter damping, real PDK passives | $V_{REF} = 800.0\text{ mV}$, $TC = 2.35\text{ ppm}/^\circ\text{C}$, $I_{tot} = 7.78\,\mu\text{A}$ across 25 corners |
| **[04. Rail-to-Rail Class-AB Op-Amp](./04-Rail-to-Rail-Class-AB-OpAmp-TSMC65/)** | TSMC 65 nm / 2.5 V | Complementary rail-to-rail input, Monticelli translinear output stage, self-tracking bias | $A_{OL} = 77.04\text{ dB}$, $f_u = 10.09\text{ MHz}$, dynamic $I_{out} > 610\,\mu\text{A}$ ($R_L = 2\text{ k}\Omega$) |

---

## EDA & Simulation Toolflow
* **Schematic Capture & Simulation:** Cadence Virtuoso, Spectre Simulation Platform (ADE L, ADE XL)
* **Design Methodology:** Transistor sizing via the $g_m/I_D$ methodology using the **ADT Sizing Assistant** (lookup-table sweeps for intrinsic gain $g_m/g_{ds}$, transit frequency $f_T$, current density $I_D/W$, and early voltage $V_A$)
* **Stability & Dynamic Analysis:** Spectre `stb` stability engine (`iprobe`), `xf` transfer function analysis, small-signal AC, and large-signal transient characterization

---

## Intellectual Property & NDA Compliance
All proprietary foundry technology files, device compact models (`.scs`), and design rule decks have been omitted to comply with foundry non-disclosure agreements (TSMC, GlobalFoundries). The repository contains self-authored technical documentation, design hand calculations, and simulation verification data.
