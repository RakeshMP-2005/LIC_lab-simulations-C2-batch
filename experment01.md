# DESIGN AND ANALYSIS OF COMMON SOURCE (CS) NMOS AMPLIFIER
**Technology:** TSMC 180nm  
**Tool Used:** LTspice  
**Supply Voltage (VDD):** 1.5V  

---
# Theory
The NMOS transistor is widely used in modern integrated circuits due to its low power consumption and high input impedance. In the Common Source configuration, the input signal is applied at the gate while the output is taken from the drain. This configuration provides significant voltage gain along with a 180° phase shift between input and output. For proper amplification, the NMOS must operate in the saturation region. 

The saturation conditions are V<sub>GS</sub> > V<sub>TH</sub> and V<sub>DS</sub> ≥ V<sub>GS</sub> − V<sub>TH</sub>

---
### Procedure

1. Install and include the TSMC 180 nm technology library in LTspice.

2. Construct the NMOS Common Source amplifier circuit with the following connections:
   - Source connected to ground  
   - Drain connected to V<sub>DD</sub> through a load resistor (R<sub>D</sub>)  
   - Gate connected to the required DC bias voltage along with the AC input signal  
   - Body terminal connected to source  
   - Load capacitor connected at the output (drain) node  

3. Set the channel length (L) to 180 nm and choose an initial value for the transistor width (W).

4. Perform DC operating point analysis.

5. Adjust the transistor width (W) until the required drain current (I<sub>D</sub>) and output voltage are achieved within the specified power limit.

6. Verify that the transistor satisfies the saturation condition:  
   V<sub>GS</sub> > V<sub>TH</sub> and V<sub>DS</sub> ≥ V<sub>GS</sub> − V<sub>TH</sub>.

7. Apply a sinusoidal input signal and perform transient analysis.

8. Perform small-signal AC analysis using a 1 V AC input.

9. Compare the theoretical results with the simulation results.
---
# AIM

To design and analyze a Common Source (CS) NMOS amplifier using nmosfet in tsmc 180nm  and study its DC operating point, DC sweep characteristics, transient response, AC response, and Gain Bandwidth Product.

---

# DESIGN SPECIFICATIONS

- VDD = 1.5V  
- Power constraint P ≤ 0.5mW  
- Load capacitance CL = 1pF  
- Channel length L = 180nm  

---
# CIRCUIT DIAGRAM

### Without capacitor

<img width="1138" height="684" alt="image" src="https://github.com/user-attachments/assets/8fa3c333-9d5b-4994-92d5-ada8e168cda3" />

### With capacitor
<img width="1233" height="738" alt="image" src="https://github.com/user-attachments/assets/db8818e2-abd1-48e7-a132-7a0104e03b71" />

---

# DESIGN CALCULATIONS

For maximum symmetrical swing:

Vout = VDD / 2  
Vout = 1.5 / 2 = 0.75V  

Power constraint:

P = VDD × ID  
0.5mW ≥ 1.5 × ID  
ID ≤ 0.33mA  

##  Selection of Drain Current and Saturation Verification

To satisfy the power constraint (P ≤ 0.5 mW), the drain current must be limited.
Maximum allowable current:
ID ≤ 0.33 mA
Chosen design value:
ID = 200 µA

---

### Threshold Voltage

From the TSMC 0.18 µm model parameters:
VTH = 0.36 V (from datasheet)

---

### Saturation Condition

For an NMOS transistor to operate in saturation:

VDS ≥ VGS − VTH
Rearranging:

VGS ≤ VDS + VTH

In this design:
VDS = Vout = 0.75 V

Substituting:

VGS ≤ 0.75 + 0.36  
VGS ≤ 1.11 V

Applied DC gate voltage:
VGS = 0.9 V  (0.9 V < 1.11 V)


The transistor operates in the saturation region.

### Conclusion

The selected drain current and gate bias ensure proper saturation operation, making the circuit suitable for linear amplification.

---

Drain resistor:

RD = (VDD − VDS) / ID  
RD = (1.5 − 0.75) / 200µA  
RD = 3.75kΩ  

##  Calculation of Process Parameter of (kn) and Transistor Width

The process transconductance parameter is given by:

kn = μn Cox

Where:

μn = 273.8 × 10⁻⁴  
εox = 3.45 × 10⁻¹¹  
tox = 4.1 × 10⁻⁹  

Since:

Cox = εox / tox

Cox = (3.45 × 10⁻¹¹) / (4.1 × 10⁻⁹)

Therefore,

kn = 2.3039 × 10⁻⁴ A/V²

---

### Width Calculation

Using the MOSFET saturation current equation:

ID = (1/2) μnCox (W/L) (VGS − VTH)²
(neglecting  channel length modulation)

Substituting:

ID = 200 × 10⁻⁶ A  
L = 180 nm  
VGS = 0.9 V  
VTH = 0.36 V  

Overdrive voltage:

VGS − VTH = 0.54 V

Rearranging to find W:

W = [2 × ID × L] / [kn × (VGS − VTH)²]

W = (2 × 200 × 10⁻⁶ × 180 × 10⁻⁹) / (2.3039 × 10⁻⁴ × (0.54)²)

After calculation:

W = 1.071 µm

---

The calculated transistor width required to obtain ID = 200 µA is:

W = 1.071 µm

## Practical Width Adjustment in LTspice

From theoretical calculations:

W = 1.071 µm  
for ID = 200 µA

However, when simulated in LTspice using:

W = 1.07 µm

The obtained drain current was:

ID ≈ 147.9 µA

![image](https://github.com/user-attachments/assets/f659b7ea-78bc-4857-a6c8-b0961222435b)

This difference occurs due to second-order effects, mobility degradation, and model non-idealities present in the TSMC 0.18 µm library.

---

### Width Optimization

To obtain the desired drain current of 200 µA in simulation, the transistor width was increased.

Final simulated width:

W = 1.575 µm

Increase in width:

ΔW ≈ 0.51 µm

With this adjusted width, the simulated drain current became:

ID ≈ 200 µA

![image](https://github.com/user-attachments/assets/76ef3a8c-1178-43c8-9bad-b827a3330edf)

---

### Conclusion

The theoretical width provides an initial estimate.  
However, practical simulation requires width tuning due to non-ideal MOSFET model effects.  

Final design width used in simulation:

W = 1.575 µm

---

# DC OPERATING POINT ANALYSIS (.op)

![image](https://github.com/user-attachments/assets/76ef3a8c-1178-43c8-9bad-b827a3330edf)

Observation:

- Vout ≈ 0.75V  
- ID ≈ 200µA  
- VGS = 0.9V  
- Device operating in saturation region  

The biasing is correct and suitable for amplification.

---

# DC SWEEP ANALYSIS (.dc)

![Screenshot 2026-02-25 184608](https://github.com/user-attachments/assets/5a281a39-694c-4cd7-ade8-38b2080bfd7b)

### Observation

As the input voltage increases, the output voltage decreases, showing that the Common Source amplifier behaves as an inverting amplifier.  

At low input voltage, the transistor operates in the cutoff region and the output remains close to VDD.  

In the middle region, the output varies almost linearly with the input, indicating the saturation (active) region where amplification occurs.  

At higher input voltage, the transistor enters the triode region and the output voltage drops to a low value.

---

# Small-Signal Linear Operation Verification

For linear amplification:

v<sub>gs</sub> << 2(v<sub>ov</sub>)

Overdrive voltage:
v<sub>ov</sub> =v<sub>gs</sub> −v<sub>th</sub> 
v<sub>ov</sub> = 0.9 − 0.36  
v<sub>ov</sub> = 0.54 V

From the input configuration:

Amplitude = 10 mV  
Peak-to-peak voltage = 20 mV  

Overdrive voltage:

v<sub>ov</sub>= 0.54 V = 540 mV  

Comparison:

10 mV << 1.08 V  

The small-signal condition is satisfied.

The device operates in the saturation region and satisfies the small-signal condition.  

Hence, the circuit functions as a linear Common Source amplifier.

---
# TRANSIENT ANALYSIS (.tran)

The chosen input signal:
v<sub>gs</sub> = 0.9 + 10m sin(2π × 1000t)

![image](https://github.com/user-attachments/assets/60aa16cd-807f-4a72-a78c-616ed9075e77)

Measured values:

### Output Peak-to-Peak Voltage

From the transient waveform:

Vout(pp) = 769.45 mV − 725.53 mV  
Vout(pp) = 43.92 mV

### Input Peak-to-Peak Voltage

Vin(pp) = 909.55 mV − 890.38 mV  
Vin(pp) = 19.17 mV  

Vout(pp) = 43.92mV  
Vin(pp) = 19.17mV  

Voltage Gain:

Av = 43.92 / 19.17  
Av = 2.29 V/V  

Av(dB) = 20 log(2.29)  
Av = 7.296 dB  

Output waveform is inverted (180° phase shift).

---

## Theoretical Small-Signal Gain Calculation

For a Common Source amplifier, the small-signal voltage gain is given by:

Av = -gm × RD

### Transconductance (gm)

The transconductance of a MOSFET operating in saturation is:

gm = 2ID / (VGS − VTH)

Substituting the design values:

ID = 200 µA  
VGS = 0.9 V  
VTH = 0.36 V  

Overdrive voltage:

VGS − VTH = 0.54 V

Therefore,

gm = (2 × 200 × 10⁻⁶) / 0.54  
gm = 7.407 × 10⁻⁴ S

### Theoretical Gain

Using:

RD = 3.75 kΩ

Av = -gm × RD  
Av = -(7.407 × 10⁻⁴) × (3.75 × 10³)  
|Av|= 2.777 V/V  

### Gain in dB

Av(dB) = 20 log(2.777)  
Av ≈ 8.84 dB  

### Reason for Difference Between Theoretical and Simulated Gain

The theoretical voltage gain was calculated using the ideal expression:

Av = -gm × RD

This expression assumes infinite output resistance (ro) and ideal long-channel MOSFET behavior. However, in practical operation and in TSMC 0.18µm simulation, the MOSFET has finite output resistance due to channel length modulation. The actual small-signal gain is therefore:

Av = -gm (RD || ro)

Since (RD || ro) is always less than RD, the effective load resistance reduces, resulting in lower gain.

Additionally, short-channel effects, mobility degradation, and other non-idealities present in 180nm technology further reduce the effective transconductance (gm).

Therefore, the simulated gain (2.29 V/V) is lower than the ideal theoretical gain (2.77 V/V).

---

# AC ANALYSIS (.ac)

## Without Load Capacitor

![image](https://github.com/user-attachments/assets/16f109b9-bd99-49bc-83d7-49e59c7d92ec)

Midband Gain ≈ 7.178 dB  
Upper cutoff frequency ≈ 100 GHz  

## With Load Capacitor (CL = 1pF)

![image](https://github.com/user-attachments/assets/65e77e7e-0b27-4fe9-a81b-368258d5e41d)

Midband Gain ≈ 7.178 dB  
Upper Cutoff Frequency fH = 47.605 MHz  

Bandwidth ≈ 47.605 MHz  

---

# GAIN BANDWIDTH PRODUCT (GBP)

with capacitor

GBP = Av × BW  

GBP = 2.29 × 47.605 MHz  

GBP ≈ 109 MHz  

Unity gain frequency ≈ 98.4 MHz 
From simulation:
- GBP ≈ 109 MHz  
- Unity gain bandwidth ≈ 98.4 MHz  

Ideally, for a single dominant pole system, GBP should be equal to the unity gain bandwidth. However, in the practical Common Source (CS) NMOS amplifier, a slight difference is observed.
This difference occurs because the amplifier is not an ideal single-pole system. Due to parasitic capacitances (Cgs, Cgd, Cdb), Miller effect, finite output resistance (ro), and short-channel effects in 0.18µm technology, multiple high-frequency poles are introduced.
As a result, the frequency response deviates slightly from ideal single-pole behavior, causing the unity gain frequency to be slightly lower than the calculated GBP.
The observed variation is small and is expected in practical CMOS amplifier designs.

---

# CONCLUSION

The Common Source NMOS amplifier was successfully designed under the given power constraint.

- Proper DC biasing achieved  
- Gain ≈ 2.29 V/V (7.29 dB)  
- Bandwidth ≈ 100GHz without capacitor
   with capacitor
- Bandwidth ≈ 47.6 MHz
- GBP ≈ 109 MHz  

The theoretical and simulated results are reasonably close, validating the design.

---
# Inference
The Common Source NMOS amplifier was successfully designed and analyzed using TSMC 180 nm technology under the given power constraint. The transistor was properly biased in the saturation region with a drain current of 200 µA and power dissipation of 0.3 mW. The simulated voltage gain was 2.29 V/V (7.29 dB), which is close to the theoretical value. The bandwidth with 1 pF load capacitance was found to be 47.6 MHz. The results validate proper amplifier operation considering practical non-ideal effects.
