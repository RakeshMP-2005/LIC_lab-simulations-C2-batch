# Linear Integrated Circuits Laboratory  

## Experiment 02  
### MOS Common Source Amplifier Configurations (TSMC 180 nm)

---

# 1. Objective

The objective of this experiment is to **design, simulate, and analyze different MOSFET amplifier configurations** using **TSMC 180 nm CMOS technology** in **LTspice**.

The amplifier configurations investigated in this experiment are:

1. **Common Source amplifier with source degeneration**
2. **Cascode MOS amplifier**
3. **Current mirror loaded common source amplifier**

Each circuit is analyzed using **DC operating point analysis, transient simulation, and AC frequency response** to evaluate:

- Biasing conditions  
- Voltage gain  
- Signal inversion  
- Bandwidth and frequency response  

---

# 2. Software and Technology

| Item | Specification |
|-----|---------------|
| Simulation Tool | LTspice |
| Technology | TSMC 180 nm CMOS |
| Supply Voltage | 1.5 V |
| Devices Used | NMOS and PMOS |

The circuits are designed using **TSMC 180 nm MOSFET models**, which represent realistic transistor behavior used in CMOS integrated circuits.

---

# 3. Background Theory

MOSFET amplifiers operate with the transistor biased in the **saturation region**, where the drain current mainly depends on the **gate–source voltage**.

When the MOSFET operates in saturation, it behaves as a **voltage-controlled current source**, which enables the device to amplify small input signals.

For an NMOS transistor to remain in saturation:

V<sub>DS</sub> ≥ V<sub>GS</sub> − V<sub>TH</sub>

where:

- V<sub>GS</sub> → Gate–source voltage  
- V<sub>TH</sub> → Threshold voltage  
- V<sub>DS</sub> → Drain–source voltage  

To ensure proper amplification, the transistor must **remain in the saturation region for the entire signal swing**.

---

## Important MOSFET Relations

| Parameter | Expression | Description |
|-----------|------------|-------------|
| Drain Current | I<sub>D</sub> = (1/2) μC<sub>ox</sub>(W/L)V<sub>ov</sub><sup>2</sup> | Drain current in saturation |
| Overdrive Voltage | V<sub>ov</sub> = V<sub>GS</sub> − V<sub>TH</sub> | Effective gate voltage |
| Transconductance | g<sub>m</sub> = 2I<sub>D</sub> / V<sub>ov</sub> | Small-signal gain parameter |
| Output Resistance | r<sub>o</sub> = 1 / (λI<sub>D</sub>) | Effect of channel length modulation |
| Voltage Gain | A<sub>v</sub> = − g<sub>m</sub>R<sub>out</sub> | Approximate voltage gain of a CS amplifier |

These equations are used for **transistor sizing and theoretical gain calculations** in MOS amplifier design.

---
---

# 4. Technology Parameters (TSMC 180 nm)

The design is implemented using **TSMC 180 nm CMOS technology**.  
The following technology parameters are used for device sizing and circuit analysis.

| Parameter | Value |
|----------|------|
| Supply Voltage | 1.5 V |
| Target Drain Current | 200 µA |
| Overdrive Voltage | 0.25 V |
| Channel Length | 180 nm |
| Electron Mobility | 273.8 × 10<sup>-4</sup> m²/V·s |
| Hole Mobility | 115.7 × 10<sup>-4</sup> m²/V·s |
| Oxide Thickness | 4.1 nm |

---

## Gate Oxide Capacitance

The gate oxide capacitance per unit area is given by

C<sub>ox</sub> = ε<sub>ox</sub> / t<sub>ox</sub>

Where

- ε<sub>ox</sub> = Permittivity of silicon dioxide  
- t<sub>ox</sub> = Oxide thickness  

Substituting the values:

C<sub>ox</sub> = (3.54 × 10<sup>-11</sup>) / (4.1 × 10<sup>-9</sup>)

C<sub>ox</sub> ≈ **8.63 mF/m²**

---

# 5. Process Transconductance Parameters

The process transconductance parameter is calculated as

μC<sub>ox</sub>

| Device | Result |
|------|------|
| μ<sub>n</sub>C<sub>ox</sub> | ≈ 236 µA/V² |
| μ<sub>p</sub>C<sub>ox</sub> | ≈ 100 µA/V² |

These parameters are used to determine the **required transistor width for the desired drain current**.

---

# 6. Transistor Sizing

The MOSFET width is obtained using the **saturation current equation**

I<sub>D</sub> = (1/2) μC<sub>ox</sub> (W/L) V<sub>ov</sub><sup>2</sup>

Rearranging the equation to obtain the transistor width:

W = (2 I<sub>D</sub> L) / [ μC<sub>ox</sub> V<sub>ov</sub><sup>2</sup> ]

This equation is used to determine the required **NMOS and PMOS widths** for the target drain current of **200 µA**.

---
---

# 7. Calculated Transistor Widths

Using the saturation current equation

I<sub>D</sub> = (1/2) μC<sub>ox</sub> (W/L) V<sub>ov</sub><sup>2</sup>

the transistor widths are calculated to achieve the **target drain current of 200 µA** with **V<sub>ov</sub> = 0.25 V** and **L = 0.18 µm**.

| Device | Calculated Width |
|------|------------------|
| NMOS | ≈ 5 µm |
| PMOS | ≈ 11.8 µm |

The **PMOS width is larger than the NMOS width** because **hole mobility is lower than electron mobility**, requiring a wider device to carry the same current.

---

## Final Transistor Aspect Ratios

| Transistor | W | L | W/L |
|-----------|---|---|-----|
| NMOS | 5 µm | 0.18 µm | 27.7 |
| PMOS | 11.83 µm | 0.18 µm | 65.7 |

These aspect ratios are used in the LTspice simulation to ensure the desired **drain current of approximately 200 µA** while maintaining proper biasing conditions.

---
---

# Circuit 2A  
## Source Degenerated Common Source Amplifier

A **source resistor R<sub>S</sub>** is introduced at the source terminal of the NMOS transistor.  
This configuration provides **local negative feedback**, which improves bias stability and linearity.

### Advantages

- Improved bias stability  
- Reduced gain sensitivity to device variations  
- Better linearity  
- Controlled voltage gain  

The approximate voltage gain is given by

A<sub>v</sub> = − g<sub>m1</sub>R<sub>out</sub> / (1 + g<sub>m1</sub>R<sub>S</sub>)

---

## LTspice Circuit

<img width="1121" height="787" alt="image" src="https://github.com/user-attachments/assets/1fc35518-b618-42fc-b97a-4ebe1778e848" />


The circuit is designed such that the MOSFET operates with

I<sub>D</sub> ≈ 200 µA

while maintaining maximum possible output voltage swing.

---

# DC Operating Point Analysis

The DC biasing is selected to ensure that **both NMOS and PMOS operate in saturation**.

## Design Conditions

| Parameter | Value |
|----------|------|
| V<sub>DD</sub> | 1.5 V |
| I<sub>D</sub> | 200 µA |
| V<sub>THn</sub> | 0.36 V |
| V<sub>THp</sub> | −0.39 V |

---

## Saturation Conditions

| Device | Condition |
|------|-----------|
| NMOS | V<sub>DS</sub> ≥ V<sub>ov</sub> |
| PMOS | V<sub>SD</sub> ≥ V<sub>ov</sub> |

---

## Choice of Drain Voltage

To obtain **maximum symmetrical output swing**, the drain voltage is chosen approximately at half of the supply voltage.

V<sub>D</sub> ≈ V<sub>DD</sub> / 2

V<sub>D</sub> = 1.5 / 2

V<sub>D</sub> ≈ **0.75 V**

---

## Source Voltage

A small voltage drop is introduced across the source resistor to stabilize the operating point.

V<sub>S</sub> ≈ **0.2 V**

---

## Output Voltage

V<sub>DS</sub> = V<sub>out</sub> − V<sub>S</sub>

0.75 = V<sub>out</sub> − 0.2

V<sub>out</sub> ≈ **0.95 V**

---

## Source Resistor

R<sub>S</sub> = V<sub>S</sub> / I<sub>D</sub>

R<sub>S</sub> = 0.2 / (200 µA)

R<sub>S</sub> = **1 kΩ**

---

## NMOS Gate Voltage

V<sub>GS</sub> = V<sub>TH</sub> + V<sub>ov</sub>

V<sub>GS</sub> = 0.36 + 0.25

V<sub>GS</sub> = **0.61 V**

Gate voltage:

V<sub>G</sub> = V<sub>GS</sub> + V<sub>S</sub>

V<sub>G</sub> ≈ **0.81 V**

---

## PMOS Gate Voltage

V<sub>SG</sub> = V<sub>ov</sub> + |V<sub>THp</sub>|

V<sub>SG</sub> = 0.25 + 0.39

V<sub>SG</sub> = **0.64 V**

PMOS gate voltage:

V<sub>Gp</sub> = V<sub>DD</sub> − V<sub>SG</sub>

V<sub>Gp</sub> ≈ **0.86 V**

---

## Saturation Verification

| Device | Condition | Check |
|------|-----------|------|
| NMOS | V<sub>DS</sub> ≥ V<sub>ov</sub> | 0.75 ≥ 0.25  |
| PMOS | V<sub>SD</sub> ≥ V<sub>ov</sub> | 0.55 ≥ 0.25  |

Both transistors operate in the **saturation region**, ensuring proper amplifier operation.
