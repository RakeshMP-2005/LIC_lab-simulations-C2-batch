# Experiment 02 – Common Source Amplifier using TSMC 180nm

**Technology:** TSMC 180 nm  
**Tool:** LTspice  
**Supply Voltage:** 1.5 V  

---

# Circuit Diagram

The circuit is a **Common Source amplifier** with an NMOS input transistor and a PMOS active load.

- **M<sub>1</sub>** : NMOS amplifying transistor  
- **M<sub>2</sub>** : PMOS current source load  
- **R<sub>S</sub>** : Source degeneration resistor  

<img width="985" height="806" alt="image" src="https://github.com/user-attachments/assets/6fda5258-7a39-49cc-bd99-f45e496cfe2d" />


---

# 1. DC Operating Point Analysis

DC analysis is performed using the `.op` command in LTspice to determine the **bias point** of the circuit.

## Operating Point Values

| Parameter | Value |
|------|------|
| V<sub>DD</sub> | 1.5 V |
| V<sub>G</sub> | 0.81 V |
| V<sub>S</sub> | 0.2001 V |
| V<sub>out</sub> | 0.8506 V |
| I<sub>D</sub> | ≈ 200 µA |

<img width="463" height="426" alt="image" src="https://github.com/user-attachments/assets/05b899dc-7078-4033-b201-0c78ea2541a6" />

---

## Drain Current Verification

The current through the source resistor confirms the drain current.

I<sub>D</sub> = V<sub>S</sub> / R<sub>S</sub>

I<sub>D</sub> = 0.200082 / 1000

I<sub>D</sub> ≈ 200 µA

This matches the LTspice operating point value.

---

## Saturation Condition Verification

For NMOS operation in saturation

V<sub>DS</sub> ≥ V<sub>GS</sub> − V<sub>TN</sub>

### Gate-Source Voltage

V<sub>GS</sub> = V<sub>G</sub> − V<sub>S</sub>

V<sub>GS</sub> = 0.81 − 0.200082

V<sub>GS</sub> = 0.6099 V

Given

V<sub>TN</sub> = 0.36 V

### Overdrive Voltage

V<sub>OVn</sub> = V<sub>GS</sub> − V<sub>TN</sub>

V<sub>OVn</sub> = 0.6099 − 0.36

V<sub>OVn</sub> ≈ 0.25 V

Since the transistor satisfies this condition, **M<sub>1</sub> operates in saturation**.

---

# Output Swing Limits

## Minimum Output Voltage

V<sub>out,min</sub> = V<sub>S</sub> + V<sub>OVn</sub>

V<sub>out,min</sub> = 0.200082 + 0.25

V<sub>out,min</sub> ≈ **0.45 V**

---

## Maximum Output Voltage

For PMOS

V<sub>SG</sub> = V<sub>DD</sub> − V<sub>G,p</sub>

V<sub>SG</sub> = 1.5 − 0.86

V<sub>SG</sub> = 0.64 V

Given

V<sub>TP</sub> = 0.39 V

V<sub>OVp</sub> = V<sub>SG</sub> − |V<sub>TP</sub>|

V<sub>OVp</sub> = 0.64 − 0.39

V<sub>OVp</sub> = 0.25 V

Therefore

V<sub>out,max</sub> = V<sub>DD</sub> − V<sub>OVp</sub>

V<sub>out,max</sub> = 1.5 − 0.25

V<sub>out,max</sub> ≈ **1.25 V**

---

## Output Swing

V<sub>swing</sub> = V<sub>out,max</sub> − V<sub>out,min</sub>

V<sub>swing</sub> = 1.25 − 0.45

V<sub>swing</sub> ≈ **0.80 V**

---

## Symmetry Check

V<sub>out,sym</sub> = ( V<sub>out,max</sub> + V<sub>out,min</sub> ) / 2

V<sub>out,sym</sub> = (1.25 + 0.45) / 2

V<sub>out,sym</sub> ≈ **0.85 V**

From LTspice

V<sub>out</sub> = **0.8506 V**

Therefore the amplifier is **almost perfectly symmetric**.

---

# 2. DC Sweep Analysis

A **DC sweep** of the input voltage V<sub>in</sub> is performed to observe the transfer characteristics of the amplifier.

The sweep helps identify the **linear operating region**.

<img src="images/dc_sweep.png" width="600">

The selected bias point **V<sub>in</sub> = 0.81 V** places the amplifier near the **center of the linear region**, allowing maximum symmetrical output swing.

---

# 3. Transient Analysis

A small sinusoidal signal is applied at the input to verify time-domain amplification.

### Input Signal

V<sub>in</sub> = V<sub>bias</sub> + V<sub>m</sub> sin(ωt)

Example:

- Frequency = 1 kHz  
- Amplitude = 10 mV  

### Gain Calculation

A<sub>v</sub> = V<sub>out(pp)</sub> / V<sub>in(pp)</sub>

| Parameter | Value |
|------|------|
| V<sub>in(pp)</sub> | 20 mV |
| V<sub>out(pp)</sub> | (from simulation) |

<img src="images/transient.png" width="600">

The output waveform is **inverted**, confirming the behavior of a **common source amplifier**.

---

# 4. AC Analysis

AC analysis is performed to determine the **frequency response** of the amplifier.

<img src="images/ac_analysis.png" width="600">

Important parameters extracted:

| Parameter | Value |
|------|------|
| Midband Gain | (from simulation) |
| Phase | ≈ −180° |
| Bandwidth | (from plot) |

The −180° phase shift confirms that the amplifier operates as an **inverting voltage amplifier**.

---

# Power Consumption

Total DC power drawn from the supply

P = V<sub>DD</sub> × I<sub>D</sub>

P = 1.5 × 200 µA

P = **300 µW**

---

# Final Summary

| Parameter | Value |
|------|------|
| V<sub>DD</sub> | 1.5 V |
| I<sub>D</sub> | ≈ 200 µA |
| V<sub>out</sub> | 0.8506 V |
| V<sub>out,min</sub> | ≈ 0.45 V |
| V<sub>out,max</sub> | ≈ 1.25 V |
| Output Swing | ≈ 0.80 V |
| Power | ≈ 300 µW |

The amplifier is biased close to the **optimal symmetric operating point**, enabling maximum undistorted output swing.
