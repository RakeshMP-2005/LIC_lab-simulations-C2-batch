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

<img width="793" height="683" alt="image" src="https://github.com/user-attachments/assets/36f3eb07-de29-4078-90e4-412284c09b34" />


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

<img width="498" height="445" alt="image" src="https://github.com/user-attachments/assets/97817f35-73e8-4152-a299-30d1cecf061f" />


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

<img width="1912" height="876" alt="image" src="https://github.com/user-attachments/assets/eb87d5c8-e9dc-4ec3-a68c-30bae3c9cc38" />
  
## Practical Gain Calculation

| Parameter | Value |
|---|---|
| V<sub>in</sub> (p-p) | 0.01969 V |
| V<sub>out</sub> (p-p) | 0.17281 V |
| Gain | 8.7765 V/V |
| Gain (dB) | 18.86 dB |

Voltage gain

A<sub>v</sub> = V<sub>OUT</sub> / V<sub>IN</sub>

A<sub>v</sub> ≈ **8.7765 V/V**

### Gain in dB

A<sub>v</sub>(dB) = 20 log<sub>10</sub>(A<sub>v</sub>)

A<sub>v</sub>(dB) ≈ **18.86 dB**

---

## Theoretical Gain Calculation

### Assumption of Channel Length Modulation Parameter (λ)

In MOSFETs, the channel length modulation parameter (λ) models the dependence of the drain current on the drain–source voltage (V<sub>DS</sub>) when the transistor operates in the saturation region. Because of this effect, the drain current slightly increases with V<sub>DS</sub>, resulting in a **finite small-signal output resistance**.

In practical CMOS technologies, λ depends on the **channel length and fabrication parameters**. For short-channel technologies such as **180 nm**, λ typically lies in the range of **0.05 – 0.1 V⁻¹** for hand calculations.

For simplicity in theoretical analysis, we assume:

λ = **0.1 V⁻¹**

This value is commonly used in first-order small-signal calculations to estimate the **output resistance of MOS transistors**.

---

### Step 1: Transconductance

g<sub>m</sub> = 2I<sub>D</sub> / V<sub>OV</sub>

g<sub>m</sub> = (2 × 200 × 10⁻⁶) / 0.25

g<sub>m</sub> = **1.6 × 10⁻³ S**

g<sub>m</sub> = **1.6 mS**

---

### Step 2: Output Resistance

r<sub>o</sub> = 1 / (λ I<sub>D</sub>)

r<sub>o</sub> = 1 / (0.1 × 200 × 10⁻⁶)

r<sub>o</sub> = **50 kΩ**

---

### Step 3: Effective Output Resistance

(r<sub>o1</sub> ∥ r<sub>o2</sub>)

= (50k ∥ 50k)

= **25 kΩ**

---

### Step 4: Voltage Gain

For a source-degenerated amplifier

A<sub>v</sub> = − g<sub>m</sub> (r<sub>o1</sub> ∥ r<sub>o2</sub>) / (1 + g<sub>m</sub>R<sub>S</sub>)

Substituting values

A<sub>v</sub> = − (1.6 × 10⁻³ × 25 × 10³) / (1 + 1.6 × 10⁻³ × 1 × 10³)

A<sub>v</sub> = − 40 / 2.6

A<sub>v</sub> ≈ **−15.38 V/V**

---

### Gain in dB

A<sub>v</sub>(dB) = 20 log<sub>10</sub>(|A<sub>v</sub>|)

A<sub>v</sub>(dB) = 20 log<sub>10</sub>(15.38)

A<sub>v</sub>(dB) ≈ **23.74 dB**

---

## Final Comparison

| Type | Gain (V/V) | Gain (dB) |
|---|---|---|
| Practical | **8.7765** | **18.86 dB** |
| Theoretical | **15.38** | **23.74 dB** |

The difference between theoretical and practical gain occurs due to **non-ideal MOSFET effects such as channel length modulation, mobility degradation, parasitic capacitances, and simplified assumptions used in hand calculations**.

---

## AC Analysis

<img width="1914" height="419" alt="image" src="https://github.com/user-attachments/assets/3e839792-b607-4c56-88a0-8321e366a7a3" />


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

---
## Practical Gain Calculation

The voltage gain of the amplifier is obtained from the transient waveform using the ratio of output voltage to input voltage.
A<sub>v</sub> = V<sub>OUT</sub> / V<sub>IN</sub>

A<sub>v</sub> ≈ **2.8146 V/V**

### Gain in dB

Gain(dB) = 20 log<sub>10</sub>(A<sub>v</sub>)
Gain ≈ **8.49 dB**

---

## Theoretical Gain Calculation

### Channel Length Modulation Parameter (λ)

In MOSFETs, the channel length modulation parameter (λ) represents the dependence of the drain current on the drain–source voltage (V<sub>DS</sub>) in the saturation region. Due to channel length modulation, the drain current slightly increases with V<sub>DS</sub>, which results in a **finite small-signal output resistance** for the transistor.

In practical CMOS technologies, the values of λ for **NMOS and PMOS devices are not exactly the same**. One of the primary reasons for this difference is the variation in **carrier mobility**. The mobility of electrons (in NMOS devices) is significantly higher than the mobility of holes (in PMOS devices). Because of this mobility difference, the channel characteristics and effective channel length modulation behavior differ between NMOS and PMOS transistors.

However, for simplified theoretical analysis, a typical approximate value is assumed based on commonly used ranges for **180 nm CMOS technology**.

Assumed values:

λ<sub>n</sub> = **0.1 V⁻¹**  
λ<sub>p</sub> = **0.1 V⁻¹**

---

### Step 1: Transconductance (g<sub>m1</sub>)

| Parameter | Expression | Substitution | Result |
|---|---|---|---|
| g<sub>m1</sub> | 2I<sub>D</sub> / V<sub>OV</sub> | (2 × 200×10⁻⁶) / 0.25 | **1.6 mS** |

Since transistors **M1 and M3 carry the same current and have the same overdrive voltage**,  

g<sub>m1</sub> = g<sub>m3</sub> = **1.6 mS**

---

### Step 2: Output Resistance

| Transistor | Expression | Substitution | Result |
|---|---|---|---|
| NMOS (r<sub>o1</sub>, r<sub>o2</sub>) | 1 / (λ I<sub>D</sub>) | 1 / (0.1 × 200×10⁻⁶) | **50 kΩ** |
| PMOS (r<sub>o3</sub>) | 1 / (λ I<sub>D</sub>) | 1 / (0.1 × 200×10⁻⁶) | **50 kΩ** |

---

### Step 3: Effective Output Resistance

r<sub>o,eff</sub> = r<sub>o1</sub> ∥ r<sub>o3</sub>

r<sub>o,eff</sub> = (50k × 50k) / (50k + 50k)

r<sub>o,eff</sub> ≈ **25 kΩ**

---

### Step 4: Voltage Gain (With Source Degeneration)

A<sub>v</sub> = − g<sub>m1</sub> × r<sub>o,eff</sub> / (1 + g<sub>m1</sub> r<sub>o2</sub>)

Substituting values

A<sub>v</sub> = − (1.6×10⁻³ × 25×10³) / (1 + 1.6×10⁻³ × 50×10³)

A<sub>v</sub> = − 40 / 81

A<sub>v</sub> ≈ **−0.494 V/V**

---

### Gain in dB

A<sub>v</sub>(dB) = 20 log<sub>10</sub>(|A<sub>v</sub>|)

A<sub>v</sub>(dB) ≈ **−6.12 dB**

---

## Final Comparison

| Type | Gain (V/V) | Gain (dB) |
|---|---|---|
| Practical | **2.8146** | **8.49 dB** |
| Theoretical | **0.494** | **−6.12 dB** |

The difference between theoretical and practical gain arises due to **parasitic capacitances, mobility degradation, channel length modulation effects, and non-ideal MOSFET model parameters present in the simulation models**.

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


---
# Common Source Amplifier with PMOS Active Load and Diode-Connected NMOS Bias (TSMC 180 nm)

## Objective

To design and analyze a **Common Source MOS amplifier with PMOS active load and diode-connected NMOS bias** using **TSMC 180 nm CMOS technology** and evaluate its performance in terms of **gain, bandwidth, unity gain bandwidth, and output voltage swing**.

---

## Circuit Diagram

<img width="943" height="846" alt="image" src="https://github.com/user-attachments/assets/95a90ffb-b433-42c8-ada6-82b7df0286ad" />


*Figure: Common Source Amplifier with PMOS Active Load and Diode-Connected NMOS Bias*

---

## Design Specifications

| Parameter | Value |
|---|---|
| Technology | TSMC 180 nm |
| Supply Voltage V<sub>DD</sub> | 1.5 V |
| Power Constraint | ≤ 0.5 mW |
| Load Capacitance C<sub>L</sub> | 1 pF |
| Channel Length L<sub>n</sub> | 180 nm |

---

## Bias Current Calculation

From the power constraint

P ≤ V<sub>DD</sub> × I<sub>D</sub>

0.5 mW ≥ 1.5 × I<sub>D</sub>

I<sub>D</sub> ≤ 0.33 mA

Chosen operating current

I<sub>D</sub> = **200 µA**

This value satisfies the power constraint while maintaining proper transistor operation.

---

## Assumption of Overdrive Voltage

V<sub>OV1</sub> = 0.2 V  
V<sub>OV2</sub> = 0.2 V  

### Justification

1. A moderate overdrive voltage provides a **balance between gain and power consumption**.  
2. It ensures the MOS transistor operates in the **strong inversion saturation region**.  
3. Lower V<sub>OV</sub> increases **transconductance g<sub>m</sub>**, which improves amplifier gain.

---

## DC Bias Analysis

Overdrive voltage relation

V<sub>OV</sub> = V<sub>GS</sub> − V<sub>TH</sub>

Given

V<sub>TH</sub> ≈ 0.36 V

Therefore

V<sub>GS3</sub> = V<sub>OV3</sub> + V<sub>TH</sub>

V<sub>GS3</sub> = 0.2 + 0.36

V<sub>GS3</sub> = **0.56 V**

Thus

V<sub>G3</sub> = **0.56 V**

For the amplifying NMOS transistor

V<sub>GS1</sub> = V<sub>OV1</sub> + V<sub>TH</sub>

V<sub>GS1</sub> = 0.2 + 0.36

V<sub>GS1</sub> = **0.56 V**

Gate bias

V<sub>G1</sub> = V<sub>S</sub> + V<sub>GS1</sub>

V<sub>G1</sub> = 0.56 + 0.56

V<sub>G1</sub> = **1.12 V**

---

## Output Swing Analysis

For PMOS to remain in saturation

V<sub>SD</sub> ≥ V<sub>OV</sub>

Since

V<sub>SD</sub> = V<sub>DD</sub> − V<sub>OUT</sub>

Therefore

V<sub>DD</sub> − V<sub>OUT</sub> ≥ V<sub>OV2</sub>

Thus

V<sub>OUT,max</sub> = V<sub>DD</sub> − V<sub>OV2</sub>

V<sub>OUT,max</sub> = 1.5 − 0.2

V<sub>OUT,max</sub> = **1.30 V**

---

## Minimum Output Voltage

For NMOS saturation

V<sub>DS</sub> ≥ V<sub>OV</sub>

V<sub>DS</sub> = V<sub>OUT</sub> − V<sub>RS</sub>

Where

V<sub>RS</sub> = 0.56 V

Therefore

V<sub>OUT,min</sub> = V<sub>RS</sub> + V<sub>OV1</sub>

V<sub>OUT,min</sub> = 0.56 + 0.2

V<sub>OUT,min</sub> = **0.76 V**

---

## Output Swing

| Parameter | Value |
|---|---|
| V<sub>OUT,max</sub> | 1.30 V |
| V<sub>OUT,min</sub> | 0.76 V |

Output swing

V<sub>swing</sub> = V<sub>OUT,max</sub> − V<sub>OUT,min</sub>

V<sub>swing</sub> = **0.54 V**

---

## Symmetrical Bias Point

For symmetrical signal swing

V<sub>OUT(Q)</sub> = (V<sub>OUT,max</sub> + V<sub>OUT,min</sub>) / 2

V<sub>OUT(Q)</sub> ≈ **1.03 V**

Thus the output node is biased at **1.03 V**.

---
## DC Analysis
<img width="459" height="513" alt="image" src="https://github.com/user-attachments/assets/3deece04-dfe4-4bc3-a2e1-9eb548d640f9" />

---

## Headroom and Legroom

**Headroom**

Voltage margin between **V<sub>OUT</sub> and V<sub>DD</sub>** ensuring the PMOS transistor remains in saturation.

**Legroom**

Voltage margin between **V<sub>OUT</sub> and ground** ensuring the NMOS transistor remains in saturation.

Maintaining sufficient headroom and legroom prevents **signal clipping and distortion**.

---

## Width Calculation

MOS saturation current equation

I<sub>D</sub> = (μ<sub>n</sub>C<sub>ox</sub>/2) × (W/L) × (V<sub>OV</sub>)²

Rearranging

W = (2 × I<sub>D</sub> × L) / (μ<sub>n</sub>C<sub>ox</sub> × V<sub>OV</sub>²)

Theoretical NMOS width

W ≈ **7.81 µm**

---

## PMOS Width Calculation

Using the same equation

W ≈ **18.49 µm**

---

## Width Optimization in Simulation

| Transistor | Final Width |
|---|---|
| NMOS1 | 32.8 µm |
| NMOS3 | 32.8 µm |
| PMOS | 80 µm |

---

## Reason for Difference Between Theoretical and Simulated Current

The difference between theoretical and simulated current occurs due to practical MOSFET effects:

1. **Channel length modulation**
2. **Mobility degradation**
3. **Accurate BSIM transistor models in TSMC technology**

Therefore transistor widths are adjusted during simulation.

---

## Transient Analysis

<img width="1912" height="815" alt="image" src="https://github.com/user-attachments/assets/b8baae12-1845-45e2-9e0a-461bef7c3101" />


| Parameter | Value |
|---|---|
| V<sub>OUT,pp</sub> | 304.37 mV |
| V<sub>IN,pp</sub> | 19.69 mV |

Voltage gain

A<sub>v</sub> = V<sub>OUT</sub> / V<sub>IN</sub>

A<sub>v</sub> ≈ **15.458 V/V**

---

## Practical Gain in Decibels

Gain(dB) = 20 log<sub>10</sub>(A<sub>v</sub>)

Gain ≈ **23.283 dB**

---
## Theoretical Calculations

### Assumption of Channel Length Modulation Parameter (λ)

In theoretical hand calculations, the channel length modulation parameter is assumed as

λ = **0.1 V⁻¹**

This value is commonly used for **short channel CMOS technologies such as 180 nm** during first-order analysis.  
The parameter λ models the variation of drain current with V<sub>DS</sub> in saturation, which results in a **finite output resistance**.

---

### Transconductance Calculation

The transconductance of a MOS transistor operating in saturation is

g<sub>m</sub> = 2I<sub>D</sub> / V<sub>OV</sub>

Given

I<sub>D</sub> = 200 µA  
V<sub>OV</sub> = 0.2 V

Substituting
g<sub>m</sub> = (2 × 200 × 10⁻⁶) / 0.2
g<sub>m</sub> = 2 × 10⁻³ S
g<sub>m</sub> = **2 mS**

Since transistors **M1 and M3 carry the same current and have the same overdrive voltage**, their transconductance values are equal.
g<sub>m1</sub> = g<sub>m3</sub> = **2 mS**

---

### Output Resistance Calculation

The small-signal output resistance is
r<sub>o</sub> = 1 / (λ I<sub>D</sub>)
Given
λ = 0.1 V⁻¹  
I<sub>D</sub> = 200 µA

Substituting
r<sub>o</sub> = 1 / (0.1 × 200 × 10⁻⁶)
r<sub>o</sub> = 1 / (20 × 10⁻⁶)
r<sub>o</sub> = **50 kΩ**

---

### Voltage Gain Calculation

The small signal voltage gain of the amplifier is
A<sub>v</sub> = − g<sub>m1</sub> r<sub>o2</sub> / (1 + g<sub>m1</sub> / g<sub>m3</sub>)
Since
g<sub>m1</sub> = g<sub>m3</sub> ( both have same V<sub>ov</sub> and same current flowing through them)
The denominator becomes
1 + g<sub>m1</sub> / g<sub>m3</sub> = 2

Substituting

g<sub>m1</sub> = 2 mS  
r<sub>o2</sub> = 50 kΩ
A<sub>v</sub> = − (2 mS × 50 kΩ) / 2
A<sub>v</sub> = **−50 V/V**

--

### Gain Magnitude
|A<sub>v</sub>| = **50**
---

### Gain in Decibels

Gain in dB is calculated using

A<sub>v</sub>(dB) = 20 log<sub>10</sub>(|A<sub>v</sub>|)

A<sub>v</sub>(dB) = 20 log<sub>10</sub>(50)

A<sub>v</sub>(dB) ≈ **33.97 dB**

---

### Summary of Theoretical Results

| Parameter | Value |
|---|---|
| I<sub>D</sub> | 200 µA |
| V<sub>OV</sub> | 0.2 V |
| g<sub>m1</sub> = g<sub>m3</sub> | 2 mS |
| r<sub>o</sub> | 50 kΩ |
| Voltage Gain | −50 V/V |
| Gain Magnitude | 50 |
| Gain (dB) | 33.97 dB |
## Gain Summary

| Type of Gain | Value |
|---|---|
| Theoretical Voltage Gain A<sub>v</sub> | 50 V/V |
| Theoretical Gain (dB) | 33.97 dB |
| Practical Voltage Gain A<sub>v</sub> | 15.458 V/V |
| Practical Gain (dB) | 23.283 dB |

> **Gain Comparison**
>
> **Theoretical Gain**
> - A<sub>v</sub> = −50 V/V  
> - Gain = 33.97 dB  
>
> **Practical Gain (Simulation)**
> - A<sub>v</sub> = 15.458 V/V  
> - Gain = 23.283 dB
> - *The difference between theoretical and practical gain occurs due to non-ideal effects such as channel length modulation, mobility degradation, and parasitic capacitances present in the MOSFET models used in simulation.**
## AC Analysis

<img width="1919" height="840" alt="image" src="https://github.com/user-attachments/assets/0f8b50da-a791-4f0a-92a7-0c68fea6b782" />


| Parameter | Value |
|---|---|
| Midband Gain | 23.987 dB |
| Gain at −3 dB | 20.98 dB |

High-frequency cutoff

f<sub>H</sub> = **280.97 MHz**

---

## Gain Bandwidth Product

GBP = A<sub>v</sub> × f<sub>-3dB</sub>

GBP ≈ **4.3 GHz**

---

## Unity Gain Bandwidth

Unity gain bandwidth occurs when gain becomes **0 dB**

UGB ≈ **4.018 GHz**

Thus

GBP ≈ UGB

---

## AC Analysis with Load Capacitor

<img width="1915" height="859" alt="image" src="https://github.com/user-attachments/assets/87e32265-475b-4c47-b409-19e0fbbe8a63" />


For

C<sub>L</sub> = 1 pF
Gain remain constant but Bw changes
| Parameter | Value |
|---|---|
| Bandwidth | 17.519 MHz |
| Unity Gain Frequency | 264.97 MHz |

---

## Gain Bandwidth Product (with C<sub>L</sub>)

GBP = A<sub>v</sub> × BW

GBP ≈ **270.808 MHz**

Again

GBP ≈ UGB

---

## Inference

1. The amplifier was successfully biased at **200 µA**.  
2. The DC output voltage was set to **1.03 V** for symmetrical output swing.  
3. The **PMOS active load increases output resistance**, improving voltage gain.  
4. Device widths required adjustment due to **non-ideal MOS effects in simulation**.  
5. Adding load capacitance significantly **reduces bandwidth while gain remains nearly constant**.

---

## Conclusion

A **Common Source amplifier with PMOS active load and diode-connected NMOS bias** was designed and simulated using **TSMC 180 nm technology**.

The amplifier achieved

- Voltage Gain ≈ **15.45 V/V (23.28 dB)**  
- High-frequency bandwidth ≈ **280.97 MHz**  
- Unity gain bandwidth ≈ **4.018 GHz**

The use of an **active load improves gain while maintaining proper biasing and output swing**.

---
## Performance Comparison of Implemented Amplifier Topologies

| Performance Metric | Circuit 1: CS with PMOS Active Load | Circuit 2: Source Degenerated CS | Circuit 3: CS with Diode-Connected Load |
|---|---|---|---|
| Amplifier Structure | Common Source with PMOS Active Load | Source Degenerated Common Source | Common Source with Diode-Connected Bias |
| Bias Current I<sub>D</sub> | 200 µA | 200 µA | 200 µA |
| Supply Voltage V<sub>DD</sub> | 1.5 V | 1.5 V | 1.5 V |
| Power Dissipation | 0.30 mW | 0.30 mW | 0.30 mW |
| Theoretical Gain | 23.74 dB | −6.12 dB | 33.97 dB |
| Practical Gain (Transient) | 18.86 dB | 8.49 dB | 23.28 dB |
| Midband Gain (AC) | 18.77 dB | 8.985 dB | 23.987 dB |
| Bandwidth | 374.11 MHz | 98.107 MHz | 280.97 MHz |
| Unity Gain Bandwidth | 3.508 GHz | 17.4186 MHz | 4.018 GHz |
| Output Resistance | Moderate | Lower (due to source degeneration) | Higher (active load effect) |
| Linearity | Moderate | Improved (degeneration resistor present) | Moderate |
| Frequency Performance | Good | Limited due to degeneration | High |

---
## Inference

1. All amplifier circuits were successfully biased at **I<sub>D</sub> = 200 µA** while satisfying the given **power constraint** under a **1.5 V supply voltage**.

2. The **Common Source amplifier with PMOS active load** provides a balanced trade-off between **gain and bandwidth**, making it suitable for moderate gain applications.

3. The **Source Degenerated Common Source amplifier** exhibits **lower voltage gain** due to the presence of the degeneration resistor R<sub>S</sub>, but it improves **linearity and stability** of the amplifier.

4. The **Common Source amplifier with diode-connected load** achieves the **highest theoretical gain** because the active load increases the **effective output resistance**.

5. The AC analysis confirms the expected **gain-bandwidth trade-off**, where circuits with higher gain generally exhibit reduced bandwidth.

6. The simulation results validate the theoretical design approach while demonstrating the impact of **non-ideal MOSFET effects** in practical circuits.

---
## Conclusion

In this study, three different MOS amplifier topologies were designed and analyzed using **TSMC 180 nm CMOS technology** under the same bias conditions. Each amplifier was operated with a **drain current of 200 µA** and a **supply voltage of 1.5 V**, ensuring that the power consumption remained within the specified constraint.

The **Common Source amplifier with PMOS active load** provided a good balance between gain and bandwidth, making it suitable for moderate-gain analog applications. The **Source Degenerated Common Source amplifier** exhibited lower gain due to the presence of the degeneration resistor, but it improved the **linearity and stability** of the amplifier. The **Common Source amplifier with diode-connected load** achieved the **highest theoretical gain** because the active load increases the effective output resistance of the circuit.

Frequency analysis showed that the amplifiers demonstrate the expected **gain-bandwidth trade-off**, where higher gain generally results in reduced bandwidth. The simulation results closely follow theoretical predictions, while small differences arise due to **non-ideal MOSFET effects such as channel length modulation, mobility degradation, parasitic capacitances, and accurate BSIM device models used in simulation**.

Overall, the experiment successfully demonstrates the **impact of circuit topology on gain, bandwidth, linearity, and stability**, highlighting the fundamental design trade-offs involved in **analog CMOS amplifier design**.
