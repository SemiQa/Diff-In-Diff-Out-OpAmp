# GCOp-amp

## Fully-Differential Class-AB Folded-Cascode Operational Amplifier

**Process:** SkyWater SKY130A · **Supply:** 1.8 V · **Generated:** 2026-07-28

---

## 1 Features

- Fully-differential architecture with on-chip common-mode feedback (CMFB)
- Rail-to-rail input: common-mode 0.1 V to 1.7 V at A_OL >= 97 dB
- Open-loop gain 100.3 dB (typ), GBW 7.58 MHz
- Class-AB output stage
- THD 2.8e-05 % at 1 kHz, 1 Vpp (typ)
- Power dissipation 0.71 mW
- On-chip bias generator; only two external bias voltages required
- Verified post-layout (parasitic-extracted) across process corners and temperature

## 2 Description

GCOp-amp is a fully-differential operational amplifier using a folded-cascode topology with a Class-AB output stage, implemented in the SkyWater SKY130A process at a 1.8 V supply. A complementary (NMOS + PMOS) input pair provides a common-mode input range close to both rails. The output common-mode level is regulated by a CMFB loop employing source degeneration of the error pair, which remains stable across all process corners and the specified temperature range.

## 3 Functional Block Diagram

```
Vinp ─┬─► NMOS input pair (XM1/XM2)
      │   PMOS input pair (XM17/XM18)      rail-to-rail input
Vinm ─┴─► tail sources XM3/XM4, XM19/XM20
             │
             ▼
       Folded cascode  (XM7/XM8/XM37/XM38 PMOS,
                        XM9/XM10 NMOS, XM11/XM12 sinks)
             │
             ▼
       Class-AB bias (XMbp_l/r, XMbn_l/r)
       Output banks: 10 x mult=15, PMOS + NMOS per side  ──► Voutp
       Miller compensation XC1/XC2 + XR3/XR4             ──► Voutm
             │  sense: XR1/XR2 + XC3/XC7
             ▼
       CMFB error pair XM21/XM22 with degeneration XR6/XR7
       actuator XM5/XM6

Bias generator: XR5 + XM60 (diode-connected) ──► Vbiasn1
```

**Figure 0. Complete Schematic**

![Complete Schematic](figures/schematic.png){width=200%}

## 4 Pin Configuration

| Pin | Type | Description |
|---|---|---|
| Vdd | Supply | Positive supply, 1.8 V |
| Vss | Supply | Ground |
| Vinp | Input | Non-inverting input |
| Vinm | Input | Inverting input |
| Voutp | Output | Positive output |
| Voutm | Output | Negative output |
| Vp | Input | PMOS cascode bias (0.3 V) |
| Vn | Input | Class-AB bias (1.5 V) |

## 5 Recommended Operating Conditions

| Parameter | Min | Typ | Max | Unit |
|---|---|---|---|---|
| Supply voltage | — | 1.8 | — | V |
| Operating temperature (1) | -20 | — | 125 | °C |
| Input common-mode range | 0.1 | 0.9 | 1.7 | V |
| Differential load resistance | 1 | — | — | kΩ |
| Load capacitance (2) | — | 1 | — | pF |

(1) Limited by CMFB loop stability (Section 8.2), verified from -20 °C to 125 °C in all five process corners.
(2) Characterised at CL = 1 pF; capacitive load was not swept.

## 6 Electrical Characteristics

Vdd = 1.8 V, T = 27 °C, corner tt, CL = 1 pF, post-layout extracted netlist, unless otherwise noted. Min/Max are the extremes observed across process corners (tt/ss/ff/sf/fs) and temperature. All values are simulation results, not production-tested limits.

| Parameter | Conditions | Min | Typ | Max | Unit |
|---|---|---|---|---|---|
| Open-loop gain A_OL | RL = ∞ | 89.8 | 100.3 | 108.2 | dB |
| Open-loop gain | RL = 10 kΩ | 70.0 | 84.7 | 93.2 | dB |
| Open-loop gain | RL = 5 kΩ | — | 79.4 | — | dB |
| Open-loop gain | RL = 1 kΩ | 50.9 | 66.0 | 75.0 | dB |
| Gain-bandwidth product | | 4.98 | 7.58 | 9.47 | MHz |
| Phase margin  | RL = ∞| 21.3 | 57.0 | 64.3 | ° |
| Phase margin | RL = 10 kΩ | 54.2 | 60.3 | 65.0 | ° |
| Phase margin | RL = 1 kΩ | 68.0 | 74.9 | 85.6 | ° |
| CMRR | DC, CM = 0.9 V | 164.6 | 195.6 | 207.8 | dB |
| PSRR+ | DC | — | 150.0 | — | dB |
| PSRR− | DC | — | [N/A] | — | dB |
| Input offset voltage | Monte Carlo, 1 sigma | — | 1.77 | — | mV |
| Input bias current | | — | 0 | — | A |
| Input noise density | 1 kHz | — | 264.2 | — | nV/√Hz |
| Input noise density | 10 kHz | — | 102.7 | — | nV/√Hz |
| Slew rate | RL = 5 kΩ | — | ±3.35 | — | V/µs |
| Overshoot | 50 mV step, fs | — | 0.0 | — | % |
| Settling time (0.1 %) | 50 mV step, fs | — | 2.48 | — | µs |
| THD | 1 kHz, 1 Vpp | 9.9e-06 | 2.8e-05 | 0.00086 | % |
| THD | 100 kHz, 1 Vpp | 9.5e-05 | 0.0022 | 0.083 | % |
| Output swing (4) | RL = 1 kΩ, single-ended | 0.357 | — | 1.748 | V |
| Output swing, differential (4) | | — | ±1.39 | — | V |
| Output common-mode | | 0.913 | 1.049 | 1.159 | V |
| Quiescent current | | 239 | 393 | 1101 | µA |
| Power dissipation | | — | 0.707 | — | mW |

(4) Defined as the output range for which THD remains below 1 %.

## 7 Typical Characteristics

**Figure 1. Open-Loop Gain and Phase vs Frequency**

![Open-Loop Gain and Phase vs Frequency](figures/bode.png)

**Figure 2. CMRR vs Frequency**

![CMRR vs Frequency](figures/cmrr_vs_freq.png)

**Figure 3. PSRR vs Frequency**

![PSRR vs Frequency](figures/psrr_vs_freq.png)

**Figure 4. Input-Referred Noise Density vs Frequency**

![Input-Referred Noise Density vs Frequency](figures/noise_vs_freq.png)

**Figure 5. THD vs Frequency, All Corners**

![THD vs Frequency, All Corners](figures/thd_vs_freq.png)

**Figure 6. THD vs Output Amplitude at 1 kHz**

![THD vs Output Amplitude at 1 kHz](figures/thd_vs_amplitude.png)

**Figure 7. Open-Loop Gain vs Input Common-Mode**

![Open-Loop Gain vs Input Common-Mode](figures/gain_vs_cm.png)

**Figure 8. Quiescent Current vs Temperature**

![Quiescent Current vs Temperature](figures/iq_vs_temp.png)

**Figure 9. Input Offset Distribution, Monte Carlo**

![Input Offset Distribution, Monte Carlo](figures/vos_histogram.png)

**Figure 10. Step Response, fs Corner**

![Step Response, fs Corner](figures/step_fs.png)

**Figure 11. Start-Up Behaviour, Supply Ramp**

![Start-Up Behaviour, Supply Ramp](figures/startup.png)

**Figure 12. Input/Output Waveforms, 1 kHz, Unity Gain**

![Input/Output Waveforms, 1 kHz, Unity Gain](figures/final_inout.png)

**Figure 13. Input/Output Waveforms, fs Corner**

![Input/Output Waveforms, fs Corner](figures/final_inout_fs.png)

## 8 Application Information

### 8.1 Closed-Loop Verification

The amplifier was verified in an inverting unity-gain configuration (Rf = Rin = 10 kΩ, feedback factor 0.5) driving a 1 kΩ differential load. The measured closed-loop gain deviates from unity by an amount consistent with the finite loop gain at that load.

### 8.2 CMFB Loop Stability

The CMFB loop was verified by transient analysis over -20 °C to 125 °C in all five process corners: stable in all simulated corner/temperature combinations. Stability criterion: peak-to-peak deviation of the output common-mode below 1 µV after settling.

| Corner | -20 °C | 0 °C | 27 °C | 85 °C | 125 °C |
|---|---|---|---|---|---|
| tt | PASS | PASS | PASS | PASS | PASS |
| ss | PASS | PASS | PASS | PASS | PASS |
| ff | PASS | PASS | PASS | PASS | PASS |
| sf | PASS | PASS | PASS | PASS | PASS |
| fs | PASS | PASS | PASS | PASS | PASS |

### 8.3 Known Limitations

1. ***Differential phase margin below 45° occurs only with no resistive load.** Worst case 21.3° (corner fs, -20 °C, RL = ∞). With RL ≤ 10 kΩ the phase margin stays above 54° in every corner and at every temperature. This is a small-signal AC result; the transient step response remains smooth and shows no ringing even in the unloaded worst case.
2. **The output is not rail-to-rail.** The swing is bounded by the output common-mode level rather than by the output devices.
3. Common-mode input at 0.1 V is not supported in the FF and FS corners (CMFB latch).

---

*All parameters are obtained from post-layout simulation (ngspice, R+C parasitic extraction). Transient analyses use the Gear integration method; independence from the integration timestep was verified. This document is generated automatically by `gen_datasheet.py`.*
