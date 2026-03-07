# MOS Common Source Amplifier Design and Analysis
**Technology:** TSMC 180 nm  
**Simulation Tool:** LTspice  

---

## Objective

To design and analyze a **MOS Common Source amplifier** using **TSMC 180 nm CMOS technology** and evaluate its performance in terms of **bias current, gain, bandwidth, and output swing** through simulation.

---

## Design Specifications

| Parameter | Value |
|---|---|
| Technology | TSMC 180 nm |
| Supply Voltage | 1.5 V |
| Power Constraint | ≤ 0.5 mW |
| Load Capacitance | 1 pF |
| Channel Length | 180 nm |

---

## Circuit Diagram

*(Insert LTspice circuit screenshot here)*  

**Figure 1: Common Source Amplifier Circuit**

---

## Design Assumptions

The overdrive voltage was assumed as

V<sub>OV</sub> = **0.25 V**

This value ensures that the MOS transistor operates in the **strong inversion saturation region**, providing stable transconductance and sufficient gain.

A moderate overdrive voltage also allows **adequate output swing under the low supply voltage (1.5 V)**.

In low-voltage CMOS analog design, **V<sub>OV</sub> values typically lie between 0.2 V and 0.3 V**, providing a good trade-off between gain, power consumption, and linearity.

---

## Bias Current Calculation

Maximum allowed power

P ≤ 0.5 mW

Using

P = V<sub>DD</sub> × I<sub>D</sub>

0.5 mW = 1.5 × I<sub>D</sub>

I<sub>D</sub> ≤ 0.333 mA

For safe operation

I<sub>D</sub> = **200 µA**

---

## Source Resistance Calculation

Given

V<sub>S</sub> = 0.2 V  
I<sub>D</sub> = 200 µA  

R<sub>S</sub> = V<sub>S</sub> / I<sub>D</sub>

R<sub>S</sub> = 0.2 / (200 × 10⁻⁶)

R<sub>S</sub> = **1 kΩ**

---

## Gate Bias Voltage

Given

V<sub>OV</sub> = 0.25 V  
V<sub>TH</sub> = 0.36 V  

V<sub>GS</sub> = V<sub>OV</sub> + V<sub>TH</sub>

V<sub>GS</sub> = 0.25 + 0.36

V<sub>GS</sub> = **0.61 V**

Bias voltage

V<sub>B</sub> = V<sub>GS</sub> + I<sub>D</sub>R<sub>S</sub>

V<sub>B</sub> = 0.61 + 0.2

V<sub>B</sub> = **0.81 V**

---

## Process Parameter Calculation

### NMOS

K<sub>n</sub> = μ<sub>n</sub>C<sub>ox</sub>

K<sub>n</sub> = **2.3039 × 10⁻⁴ A/V²**

### PMOS

K<sub>p</sub> = μ<sub>p</sub>C<sub>ox</sub>

K<sub>p</sub> = **9.7361 × 10⁻⁵ A/V²**

---

## NMOS Width Calculation

Drain current equation

I<sub>D</sub> = (K<sub>n</sub>/2)(W/L)(V<sub>GS</sub> − V<sub>TH</sub>)²

Rearranging

W = (2I<sub>D</sub>L) / (K<sub>n</sub>(V<sub>OV</sub>)²)

Substituting

W = (2 × 200 ×10⁻⁶ × 180 ×10⁻⁹)  
/ (2.3039 ×10⁻⁴ × (0.25)²)

W<sub>n</sub> ≈ **5 µm**

---

## PMOS Width Calculation

I<sub>D</sub> = (K<sub>p</sub>/2)(W/L)(V<sub>SG</sub> − V<sub>TH</sub>)²

Rearranging

W = (2I<sub>D</sub>L) / (K<sub>p</sub>(V<sub>OV</sub>)²)

Substituting

W = (2 × 200 ×10⁻⁶ × 180 ×10⁻⁹)  
/ (9.7361 ×10⁻⁵ × (0.25)²)

W<sub>p</sub> ≈ **11.83 µm**

---

## Theoretical Device Width

| Transistor | Width |
|---|---|
| NMOS | 5 µm |
| PMOS | 11.83 µm |

---

## Reason for Current Difference

The theoretical calculations assume the **ideal square-law MOSFET model**.

However simulation uses **BSIM transistor models** which include:

| Effect | Description |
|---|---|
| Short channel effects | Important in 180 nm |
| Mobility degradation | Mobility reduces at high field |
| Channel length modulation | Current depends on V<sub>DS</sub> |
| Process variations | Model parameters differ |

Hence the calculated width does not produce exactly **200 µA** in simulation.

---

## Width Adjustment in Simulation

| Transistor | Final Width |
|---|---|
| NMOS | **12.8 µm** |
| PMOS | **38.63 µm** |

These values produce approximately **200 µA** current.

---

## DC Operating Point

*(Insert DC operating point screenshot here)*  

**Figure 2: Operating Point**

| Parameter | Value |
|---|---|
| I<sub>D</sub> | 200 µA |
| V<sub>GS</sub> | 0.61 V |
| V<sub>S</sub> | 0.2 V |
| V<sub>OUT</sub> | **0.95 V** |

---

## Output Swing Concept (Headroom and Legroom)

The output voltage cannot vary freely between **0 and V<sub>DD</sub>** because the MOS transistors must remain in **saturation**.

The limits are defined by:

- **Legroom of NMOS**
- **Headroom of PMOS**

### Legroom (Minimum Output Voltage)

V<sub>OUT(min)</sub> = V<sub>S</sub> + V<sub>OV</sub>

V<sub>OUT(min)</sub> = 0.2 + 0.25

V<sub>OUT(min)</sub> = **0.45 V**

### Headroom (Maximum Output Voltage)

V<sub>OUT(max)</sub> = V<sub>DD</sub> − V<sub>OV</sub>

V<sub>OUT(max)</sub> = 1.5 − 0.25

V<sub>OUT(max)</sub> = **1.25 V**

### Output Swing

Swing = V<sub>OUT(max)</sub> − V<sub>OUT(min)</sub>

Swing = 1.25 − 0.45

Output Swing = **0.8 V**

---

## Transient Analysis

*(Insert transient waveform screenshot here)*  

**Figure 3: Transient Response**

| Parameter | Value |
|---|---|
| Vin (p-p) | 0.01969 V |
| Vout (p-p) | 0.17281 V |
| Gain | 8.7765 V/V |
| Gain (dB) | **18.86 dB** |

---

## AC Analysis

*(Insert Bode plot here)*  

**Figure 4: Frequency Response**

| Parameter | Value |
|---|---|
| Midband Gain | 18.77 dB |
| Bandwidth | 374.11 MHz |

---

## Gain Bandwidth Product (GBP)

GBP = A<sub>v</sub> × BW

GBP = 8.7765 × 374.11 MHz

GBP ≈ **3.28 GHz**

---

## Unity Gain Bandwidth (UGB)

From AC Bode plot

UGB ≈ **3.508 GHz**

---

## Frequency Performance Summary

| Parameter | Value |
|---|---|
| Midband Gain | 18.77 dB |
| Bandwidth | 374.11 MHz |
| Gain Bandwidth Product | **3.28 GHz** |
| Unity Gain Bandwidth | **3.508 GHz** |

---

## Inference

1. The amplifier is successfully biased at **200 µA** satisfying the power constraint.  
2. The operating point **V<sub>OUT</sub> = 0.95 V** confirms saturation operation.  
3. Output swing is limited by **headroom and legroom constraints**.  
4. The amplifier achieves **18.77 dB midband gain**.  
5. The **UGB of 3.508 GHz** indicates good high-frequency performance.

---

## Conclusion

A **Common Source MOS amplifier** was designed using **TSMC 180 nm technology** under the given power constraint.

Theoretical sizing was first performed using MOSFET equations and later adjusted in simulation to obtain the required **200 µA bias current**.

The amplifier provides **18.77 dB gain**, **374 MHz bandwidth**, and **3.508 GHz unity gain bandwidth**, confirming proper amplifier performance.

---
# Source Degenerated Common Source Amplifier (TSMC 180 nm)

## Objective

To design and analyze a **Source Degenerated Common Source MOS amplifier** using **TSMC 180 nm CMOS technology** and evaluate its performance in terms of **gain, bandwidth, unity gain bandwidth and output swing**.

---

## Circuit Diagram

<img width="995" height="686" alt="image" src="https://github.com/user-attachments/assets/cb40456f-c5c3-43e3-bcea-cce60e16e1ca" />


*Figure: Source Degenerated Common Source Amplifier*

---

## Design Specifications

| Parameter | Value |
|---|---|
| Technology | TSMC 180 nm |
| Supply Voltage V<sub>DD</sub> | 1.5 V |
| Power Constraint | ≤ 0.5 mW |
| Load Capacitance C<sub>L</sub> | 1 pF |
| Channel Length L | 180 nm |

---

## Bias Current Calculation

P ≤ V<sub>DD</sub> × I<sub>D</sub>

0.5 mW ≥ 1.5 × I<sub>D</sub>

I<sub>D</sub> ≤ 0.33 mA

Chosen operating current:

I<sub>D</sub> = **200 µA**

---

## Assumption of Overdrive Voltage

V<sub>OV</sub> = **0.25 V**

### Justification

1. Provides a **balance between gain and power consumption**.  
2. Ensures the MOS transistor operates in **strong inversion and saturation region**.  
3. Lower V<sub>OV</sub> increases **transconductance (g<sub>m</sub>)**, improving gain.

---

## Gate Source Voltage Calculation

V<sub>OV</sub> = V<sub>GS</sub> − V<sub>TH</sub>

V<sub>TH</sub> = 0.39 V

V<sub>GS</sub> = 0.25 + 0.39

V<sub>GS</sub> ≈ **0.64 V**

---

## Width Calculation

Using MOS saturation equation

I<sub>D</sub> = (μ<sub>n</sub>C<sub>ox</sub>/2) × (W/L) × (V<sub>OV</sub>)²

After substituting parameters

W ≈ **11.83 µm**

---

## Width Scaling in Simulation

| Transistor | Width |
|---|---|
| PMOS | 36.58 µm |
| NMOS1 | 60.3 µm |
| NMOS3 | 16.3 µm |

---

## DC Operating Point

<img width="443" height="564" alt="image" src="https://github.com/user-attachments/assets/1b998d98-7f1a-4b52-bd6c-05730b4febec" />


V<sub>OUT</sub> ≈ **0.875 V**

This confirms that the MOSFET operates in the **saturation region**.

---

## Output Swing

### Maximum Output Voltage

V<sub>DD</sub> − V<sub>OUT</sub> ≥ V<sub>OV</sub>

1.5 − V<sub>OUT</sub> ≥ 0.25

V<sub>OUT,max</sub> ≈ **1.25 V**

### Minimum Output Voltage

V<sub>OUT</sub> − V<sub>S</sub> ≥ V<sub>OV</sub>

V<sub>OUT,min</sub> ≈ **0.50 V**

### Output Swing Range

| Parameter | Value |
|---|---|
| V<sub>OUT,min</sub> | 0.50 V |
| V<sub>OUT,max</sub> | 1.25 V |
| Q-point | 0.875 V |

---

## Headroom and Legroom

**Headroom**

Voltage margin between **V<sub>OUT</sub> and V<sub>DD</sub>** ensuring PMOS remains in saturation.

**Legroom**

Voltage margin between **V<sub>OUT</sub> and ground** ensuring NMOS remains in saturation.

---

## Transient Analysis

<img width="1911" height="413" alt="image" src="https://github.com/user-attachments/assets/041ef09c-ffb1-4073-9df5-bb52c978bd7c" />


| Parameter | Value |
|---|---|
| V<sub>OUT,pp</sub> | 53.98 mV |
| V<sub>IN,pp</sub> | 19.18 mV |

Voltage gain

A<sub>v</sub> = V<sub>OUT</sub> / V<sub>IN</sub>

A<sub>v</sub> ≈ **2.8146 V/V**

---

## Gain in dB

Gain(dB) = 20 log₁₀(A<sub>v</sub>)

Gain ≈ **8.49 dB**

---

## AC Analysis

<img width="1911" height="421" alt="image" src="https://github.com/user-attachments/assets/1d2376e5-2ee3-4ca2-9645-ddc6b36cad01" />


| Parameter | Value |
|---|---|
| Midband Gain | 8.985 dB |
| Bandwidth | 98.107 MHz |

---

## −3 dB Frequency

f<sub>-3dB</sub> ≈ **6.6038 MHz**

---

## Unity Gain Bandwidth

UGB ≈ **17.4186 MHz**

---

## Gain Bandwidth Product

GBP = A<sub>v</sub> × f<sub>-3dB</sub>

GBP ≈ **18.587 MHz**

UGB ≈ GBP

---

## Frequency Performance Summary

| Parameter | Value |
|---|---|
| Voltage Gain | 2.81 V/V |
| Gain (dB) | 8.49 dB |
| Bandwidth | 98.107 MHz |
| Unity Gain Bandwidth | 17.4186 MHz |
| Gain Bandwidth Product | 18.587 MHz |

---

## Inference

1. The amplifier was successfully biased at **200 µA**, satisfying the power constraint.  
2. Width scaling in simulation was required to obtain accurate current.  
3. The DC operating point confirms proper **saturation operation**.  
4. Source degeneration improves **linearity and stability**.  
5. The amplifier achieves **moderate gain with good bandwidth**.

---

## Conclusion

A **Source Degenerated Common Source amplifier** was successfully designed using **TSMC 180 nm technology**.

The amplifier achieves:

- Gain ≈ **2.81 V/V**
- Bandwidth ≈ **98 MHz**
- Unity Gain Bandwidth ≈ **17.4 MHz**

The design demonstrates the trade-off between **gain and stability introduced by source degeneration**.
