# Seal Team 2 Project Proposal: Wireless Fault Current Indicator
## Introduction
Across the globe,  electrical power is supplied from source to recipient via power lines. These lines carry single and three phase power while often carrying telecommunication signals below on a separate section. If these power lines fail, it can result in loss of magnitude of power (brownout) or loss of power altogether (blackout). This can happen from a transformer blowing up, power lines grounding through trees, or power lines falling either into other lines or onto the ground. Any instance which results in unprompted behavior is called a fault. Currently, the main method in locating a fault is by mobilizing service workers and garnering information from customer calls \[1\]. This, as one can imagine, is quite costly both monetarily and temporally.

In light of this challenge, **Seal Team 2** shall develop an affordable, replicable prototype sensor which will relay information pertaining to the induced telluric currents. This information shall be sent across cell lines to the company supplying and managing the area's power. Such information will be primarily contained to fault detection for the scope of this prototype. This sensor will take information on the voltage and telluric current density surrounding the power pole, filter the data so that only the $60$, $180$, and $540$ Hz harmonics are considered, then process the data and signal a fault if necessary. This prototype will include electrodes, an analog filter, a PID controller, and a cellular communicator.

This proposal will overview the existing and forming solutions to power line fault detection. Addressing first the theory and research guiding modern solution to this problem will grant clarity to the necessity ofthe project, the challenges anticipated, and  specifications of the prototype. Furthermore, this proposal will outline the measures of success, identify resources available (both in physical quantity and in knowledge), and formulate implications (both specific and general) of the completion of the project.

## Formulating the Problem
### Background
Almost all current methods in distributing power involve transformers and power lines. The smallest of such power lines will hold two lines (a phase and a neutral) or four lines (3 phases and a neutral). Both examples are subsets of three phase power distribution lines which rely primarily on the balancing of all three phases. If power lines fall either by heavy winds, falling trees, or the pole itself falling, two major faults can occur: line-to-line faults or line-to-ground faults. Line-to-line (LL) faults cause an imbalance in the three phases by shorting two of them together. Line-to-ground (LG) faults cause an imbalance by grounding one of the phases. Other specific kinds of faults can be three line (LLL) faults two lines to ground (LLG) and others, but all of these fall in either category of LL or LG or both and will henceforth be referred to by their respective category.

First, consider the LL fault. This occurs when two or three phases have been faulted together. For ease of thought, a fault of lines A and B will be considered. Suppose a line A is faulted in a way which connects it to line B. This will result in the path: Source$\rightarrow$Transformer Winding A$\rightarrow$A$\rightarrow$B$\rightarrow$Transformer Winding B$\rightarrow$Source. Where the path through B and B's transformer winding is traveling in the reverse direction. This causes the line voltage $V_{AB}$ to be very small near the fault but remain very large at the transformer. Such a voltage difference, with the low impedance of wire, will result in immense current surges. Say lines A and B are components of a $480\,\text{V}$ 3-phase power system (US Standard) and an LL fault of lines A and B occurs. $V_{AB}$ nearest the transformer will be $\approx480\,\text{V}$ while near the fault almost $0$. If the combined impedance of the fault and power lines is $0.05\,\text{m}\Omega$, then the resulting current would be $\frac{480}{0.05}=9600\,\text{A}$. Such an amperage is far too high to be maintainable by a system which causes arcing, fires, mechanical torque, electromagnetic disruptions, etc.

Next, consider the LG fault. This occurs when one or more wires are faulted to ground. This results in the path: Source $\rightarrow$ A
$\rightarrow$ Fault $\rightarrow$ Ground $\rightarrow$ Source Neutral $\rightarrow$ Source. Notice that the path relies on the source's neutral node or other ground return scheme to create the loop. This means that if line A has an LG fault, lines B and C *will* be affected because their voltages are relative to the neutral line. Their phase to ground voltage can fluctuate due to the relevance to a now faulty ground. Current through B and C are not primarily affected, but line A's current takes a most intriguing path: through the earth.

Diverting briefly to discuss standard operation: all power lines that carry current inject a portion of it into the ground called grid-induced telluric currents (as opposed to naturally- or geomagnetic-induced telluric currents) \[1\]. For the remainder of this proposal, such currents will be referred to simply as telluric currents. These telluric currents are measurable and show the standard harmonics at 60, 180, and 540 Hz \[1\]. Because there is injected current which extends radially from the power pole, there is an electric field created with equipotential lines of concentric circles emanating from the pole. $$\vec{J}=\sigma\vec{E}:\vec{E}=-\nabla V$$ Research shows that this voltage can be measured by use of two electrodes and an amplifier \[1\]. Given the above relationship (where $\sigma$ is the conductivity of the medium) it's understood that the voltage gradient is proportional to the current density (point-form of Ohm's Law) which means that any voltage measured is proportional to current carried by the power lines.

Recall now that in both LL and LG faults there is a large surge of current on the order of at least hundreds of amps. This will cause the
telluric currents to spike, *especially in the case of LG faults*. LG faults, as a result, shall be the primary focus of this prototype but there may be possibility of uncovering a method to diagnose LL faults in tandem.

\"Grid-Induced Telluric Currents for Non-Contact Load Monitoring and Fault Detection\", by Abigail Farris \[1\], exhibits a prototype used momentarily to monitor the telluric currents produced by a substation. This document shows the use of two electrodes used to measure voltage (parallel to the radius of the voltage gradient) which was then amplified into a system and transformed to the frequency domain. The result of the Fast Fourier Transform (FFT) showed remarkable amplitudes in the 60, 180, and 540 Hz frequencies. Farris concluded in speculation that such monitoring may reveal evidence of ground faults and general load monitoring non-invasively.

Given that modern techniques are either invasive (through the use of component load monitoring), expensive (for a similar reason), or slow (by use of call-ins and field technicians searching for the fault), this method may provide a way to accurately pinpoint and relay information to whomever is heading the power distribution. For such a purpose, it would prove advantageous for this sensor to be able to transmit over long ranges and be self sufficient in power.

Thus, the scope of this prototype is that it shall obtain reliable information on the current density and be able to synthesize such data
into very small packets able to be communicated (via cellular or satellite) over vast distances. This prototype may also be capable of
using the telluric currents to charge its system whilst it remains dormant so that maintenance is far less regular. In light of reducing
the need for maintenance, the prototype shall also resist the changing weather conditions, especially heat and rain, to continue to serve
accurate and reliable information throughout the seasons.

#### Telecommunication Subsystem
The following information provides a background of relevant telecommunication theory for the process of designing the Telluric Current Sensor. 

In general, a communication system can be described by the elements shown in Figure T.1 (Figure of the Block diagram of a generic Telecomm system). The input message, $x(t)$ is the raw physical signal from which information is being extracted. In the case of Seal Team 2's Ground Fault Sensor, this will be the current flowing through the ground from the substation. $x(t)$ is then  processed by a tranducer, which will convert the physical signal into a electrical signal $x_2(t) that contains the desired information. The information is then propagated through some medium using some modulation technique along a carrier wave of the form: $Acos(\omega t + \phi)$. As the modulated wave propagates across the medium, noise is introduced which attenuates and distorts the signal. The receiver takes this noise distorted signal and demodulates it to restore the information from the input signal.

When determining the communication system architecture to be used for a given system, the following parameters are worth considering.

*Data rate* is the speed for which data tranfers from transmitter to receiver, measured in bits/s. 

*Symbol Rate / Baud Rate* is the number of symbols/characters transferred from transmitter to receiver. (Calculated as: $BaudRate = \frac{BitRate}{BitsPerSymbol}$)

*Bandwidth*: Frequency range of the transmitted signal. 

- Throughput: (*define)

*Latency* is the time delay in milliseconds for data to travel from source to destination.

- Channel Capacity (*define)

- Noise and Interference (*define)

- Signal to Noise Ratio (*define)

- Bit Error Rate (*define)

The type of modulation used in the system can minimize the effects of the noise through the medium. For digital modulation, the following techniques are relevant:
- ASK (Amplitude Shift Keying)
- FSK (Frequency Shift Keying)
- PSK (Phase Shift Keying)
- QPSK (Quadrature Phase Shift Keying)
- QAM (Quadrature Amplitude Modulation)
- Chirp-Based Modulation

The above parameters and modulation schemes shall be considered when weighing the options for telecommunication architectures for the Seal Team 2's Ground Fault Sensor.

#### Power Subsystem
A field-ready telluric ground fault sensor needs a highly adaptable power architecture capable of operating in diverse environments ranging from remote, unassisted terrain to high-voltage substations. In either scenario, powering a sensitive micro board requires balancing continuous baseline consumption with high peak bursts, all while maintaining power quality to prevent electrical noise from interfering with the ground measurements. To address these design challenges, the power subsystem explores multiple flexible architectures ranging from off-grid renewable energy to direct power feeds, governed by strict electrical and safety standards [SS-1]-[SS-5].

##### Equations

###### 1. Voltage Regulation and Drop Analysis (Supports SR-01)

$$
\%VR = \left(\frac{V_{\text{no-load}} - V_{\text{full-load}}}{V_{\text{full-load}}}\right) \times 100
$$

Where:
- $V_{\text{no-load}}$ is the output voltage at zero load current.
- $V_{\text{full-load}}$ is the output voltage under maximum active load.

###### 2. Supercapacitor Transient Energy and Voltage Sag Mitigation (Supports SR-05)

$$
E = \frac{1}{2}C\left(V_{\text{initial}}^2 - V_{\text{min}}^2\right)
$$

Rearranged to solve for the allowable voltage drop, $\Delta V$, during a pulse current, $I_{\text{pulse}}$, lasting for duration $\Delta t$:

$$
\Delta V = \frac{I_{\text{pulse}} \cdot \Delta t}{C} + I_{\text{pulse}} \cdot R_{\text{ESR}}
$$

Where:
- $C$ is capacitance (F).
- $I_{\text{pulse}}$ is peak current (A).
- $\Delta t$ is pulse duration (s).
- $R_{\text{ESR}}$ is equivalent series resistance ($\Omega$).

###### 3. Battery State of Charge and Autonomous Runtime (Supports SR-04)

$$
T_{\text{runtime}} = \frac{C_{\text{batt}} \cdot \text{DoD} \cdot \eta_{\text{sys}}}{I_{\text{avg}}}
$$

Where:
- $C_{\text{batt}}$ is battery capacity (Ah).
- $\text{DoD}$ is maximum depth of discharge (0.80 for LiFePO$_4$).
- $\eta_{\text{sys}}$ is power conversion efficiency (0.85).
- $I_{\text{avg}}$ is average continuous current (A).

###### 4. Switching Regulator Output Ripple Voltage (Supports SR-02)

$$
V_{\text{ripple}} = \frac{V_{\text{out}} \cdot (1-D)}{8 \cdot L \cdot C_{\text{out}} \cdot f_{\text{sw}}^2}
$$

Where:
- $V_{\text{out}}$ is regulated output voltage.
- $D$ is duty cycle ($V_{\text{out}}/V_{\text{in}}$).
- $L$ is inductance (H).
- $C_{\text{out}}$ is output capacitance (F).
- $f_{\text{sw}}$ is switching frequency (Hz).

###### 5. LDO Ripple Attenuation and PSRR

An ultra-low-noise LDO, such as the LT3042, downstream of the isolated switching converter attenuates switching-frequency ripple before it reaches the analog front-end:

$$
V_{\text{ripple,out}} = V_{\text{ripple,in}} \times 10^{-\text{PSRR (dB)}/20}
$$

Where:
- $V_{\text{ripple,in}}$ is peak-to-peak ripple voltage entering the LDO from the switching converter or isolated module (mV).
- $\text{PSRR}$ is the LDO's power supply rejection ratio at the switching frequency (e.g., at least 75 dB at 100 kHz).
- $V_{\text{ripple,out}}$ is the resulting ripple on the analog power rail (e.g., $\mu$V).

###### 6. LC Low-Pass Filter Corner Frequency

A passive LC filter between the isolated DC-DC converter and the LDO can pre-condition the power rail. Its corner frequency is:

$$
f_c = \frac{1}{2\pi\sqrt{L \cdot C}}
$$

Where:
- $L$ is filter inductance (H).
- $C$ is filter capacitance (F).

**Design rule:** Set $f_c$ at least one decade below the DC-DC converter's switching frequency, $f_{\text{sw}}$.

###### 7. LDO Power Dissipation and Thermal Limit

Because linear regulators dissipate excess voltage as heat, calculate the LDO's power dissipation:

$$
P_D = (V_{\text{in}} - V_{\text{out}}) \cdot I_{\text{load}}
$$

The resulting semiconductor junction temperature is:

$$
T_J = T_A + (P_D \cdot \theta_{JA})
$$

Where:
- $P_D$ is power dissipation (W).
- $V_{\text{in}}$ and $V_{\text{out}}$ are the LDO input and output voltages.
- $I_{\text{load}}$ is continuous current drawn by the analog section (A).
- $T_A$ is ambient temperature inside the enclosure (°C).
- $\theta_{JA}$ is the IC package's junction-to-ambient thermal resistance (°C/W).


#### Physical Subsystem
Many environmental issues must be considered when designing any project, but with this sensor, the surrounding conditions need to be studied and analyzed even more carefully. Since this system is going to be deployed in Cookeville, Tennessee, it will be exposed to rainfall, soil moisture, drastic temperature changes, humidity, and potentially other hazardous weather conditions. Any of these conditions could affect the performance of the system and, in some cases, make it inoperable.

The telluric ground sensor must maintain electrical contact with the soil while also protecting its electrical connections from the environmental hazards stated above. This creates a difficult design challenge because the sensor must interact directly with the ground even though the soil can contain different levels of moisture. If moisture reaches unintended electrical connections or sensitive components, it could lead to inaccurate readings, corrosion, or equipment failure. One possible solution would be to seal the entire system; however, this would be too aggressive because it would reduce the sensor’s ability to measure the electrical potential of the ground.

Additionally, environmental temperature must also be accounted for because changes in temperature can negatively impact  the electrical potential and resistance of an electrode. Therefore, the system must be designed to remain protected from outdoor conditions while still allowing the sensor to properly interact with the surrounding soil.

### Specifications and Constraints

#### Specifications

In essence, the potential stakeholders for this project other than our customer, Dr. Charles Van Neste, would be groups that deal with managing fault currents, such as utility companies. In particular, a system that can detect faults in this manner would be beneficial for rural utility companies that have to send out fleets of people to search for a fault when it occurs. Since rural utility companies, also known as utility cooperatives, serve multiple counties, a telluric current sensor with fault monitoring would be beneficial since remote areas bring along greater difficulty to locating faults when they occur. Groups that would be invested in this sensor would want a sensor that can detect a fault current using non-invasive techniques, that can transmit the information of the system to a device owned by the stakeholders, and -if possible- having a triangulation system in place to locate the fault in a power system.

##### Telecommunication Subsystems Specifications:

*Maximized data rate:* Minimum data rate threshold shall be determined on a future date.

*Maximized Signal-To-Noise ratio:* According to the CASRAI document, *Limit of Detection vs Limit of Quantitation: How to Calculate LOD and LOQ**, the generally accepted minimum signal-to-noise ratio for quantitation of data is 10:1 \[2\]. This shall be *Seal Team 2's* choice for the minimum.

*Minimized transmission power consumption (including resting power consumption):* Maximum power consumption threshold shall be determined by the constraints of the power subsystem.

*Minimized Latency:* Maximum latency threshold shall be ~1s.

##### Microcontroller Specifications
When a line-to-line fault or a line-to-ground fault is detected by the sensor, a microcontroller shall wirelessly transmit this information to a device. The size of the data transmitted is dependent on the final level of the complexity of the system. At its base form, it is required to indicate when a fault is detected in a transmission line. Ideally, a triage system would be implemented for locating where a fault occurs in the system. The microcontroller shall be able to communicate with the sensors to interpret the data and determine when a fault occurs. If it is determined to convert the analog data to digital data, this may be done with using a Fast Fourier Transform algorithm on the microcontroller, which would increase the need for computational power on the microcontroller. The microcontroller shall also be optimized to consume the minimum amount of power needed to detect a fault. The amount of power consumption for the microcontroller is correlated with the computational demand on the device, so the amount of power that will be consumed with be dependent on the data processing. For conservation of power, a sleep mode may be established to reduce the power consumption of the device.

##### Power Subsystem Specifications

- **SR-01 — Output Regulation:** The power distribution board shall provide regulated DC outputs of 3.3 V and 5 V with a voltage regulation tolerance of $\pm 2\%$ under all operating load steps.
- **SR-02 — Power Quality and Ripple:** The power conditioning circuitry shall limit high-frequency switching ripple to less than 10 mV peak-to-peak to prevent noise contamination of the analog acquisition front-end [SS-7].
- **SR-03 — Galvanic Isolation:** The power supply architecture shall maintain a reinforced isolation barrier rated for a minimum of 2 kV between external power inputs and internal instrument grounds [SS-3], [SS-21].
- **SR-04 — Autonomous Operation:** The primary power architecture shall sustain continuous, unattended operation for a minimum of 24 hours under nominal field load profiles.
- **SR-05 — Transient Response:** The regulation and post-regulation stages shall maintain voltage stability and limit transient sags to less than 5% during sudden microcontroller load steps.
- **SR-06 — Regulatory Compliance:** All power subsystem component placement, wiring methods, and energy storage enclosures shall comply with applicable NEC Articles 250, 706, and 725 requirements [SS-1] along with NFPA 855 safety guidelines [SS-2].

~~(Autonomous Operation): The primary off-grid power architecture (Solar/LiFePO₄/Supercapacitor or LiSOCl2LiSOCl2) shall sustain continuous, unattended operation for a minimum of 24 hours under nominal field load profiles[3].~~

~~(Regulatory Compliance): All power subsystem component placement, wiring methods, and energy storage enclosures shall comply with applicable NEC Articles 250, 690, 706, and 725 requirements.~~

~~(Surge & Transient Immunity): The power input stage shall incorporate surge protection devices complying with IEEE Std C62.41 to withstand high-voltage transient overvoltages and lightning-induced surges.~~

~~(Harmonic Control): The power conversion and switching circuitry shall limit current and voltage harmonic distortion to maintain compliance with IEEE Std 519, preventing conducted noise from contaminating sensitive telluric frequency bands.~~

~~(Grounding & Earth Potential): The system grounding and bonding architecture shall comply with IEEE Std 142 and support earth impedance verification per IEEE Std 81 to mitigate Ground Potential Rise (GPR) hazards in substation environments.~~

~~(Environmental & Thermal Operating Limits): The power and energy storage subsystems shall maintain safe operating parameters across an ambient temperature range of $-40^\circ\text{C}$ to $+65^\circ\text{C}$, incorporating safeguards against cold-weather charging and thermal runaway per NFPA 855.~~

~~(Portability): The complete power management and storage assembly shall be compact and lightweight enough to allow for single-person transport and deployment in remote terrain.~~

##### Physical Subsystem Specifications
- The system shall be encased in a weather-proofing system in attempt  to prevent excess moisture from entering the circuit.
- The system shall be waterproof up to a NEMA standard protection rating.
- Materials used in the enclosure shall be resistant to corrosion and degradation during outdoor exposure.

#### Constraints

##### Telecommunication Subsystem Constraints

*Independently designed transmitter constraints:*
All wireless transmitters must adhere to the authorization constraints enumerated in the FCC 47 CFR section 2.911 document. In particular, any independently constructed RF transmitter/Receiver network in need of authorization "shall be submitted in writing to a Telecommunication Certification Body (TCB) in a manner prescribed by the TCB." \[4\]. Other sections of Part 2 of FCC 47 CFR, notably, 2.1033, 2.1046, and 2.1057 must be considered when designing a transmitter that meets the federal frequency and bandwidth regulations. These legal constraints on transmitter design makes it favorable to choose an already approved (commercial) device for the transmitter.

*Integrated commercial transmitter/modem constraints:*
When integrating a commercial transmitter into a communication module, the design of the communication module should adhere to the *FCC KDB 996369 D01: Transmitter Module Equipment Authorization Guide* \[5\]. As a consequence, if the design of the transmitter of the Ground Fault Sensor incorporates a commercial device, there are still legal requirements that need to be considered.

*Use of Public Mobile Services Constraints*
When designing a transmitter to a cellular network, *FCC CFR 47 Part 22* provides limits for frequencies and effective radiated power. Section 22.352 also goes into interference protection protocols as well \[6\]. This constraint limits the available bandwidth and power output of the transmitter of the Ground Fault Sensor in the case of using a cellular communication architecture.

##### Microcontroller Subsystem Constraints
    
Since the microcontroller will be directly wired to the data acquisition unit, it would be regulated under Supbpart B of Title 47 of the FCC. Under Title 47 of the FCC, the microcontroller would be considered a Class A digital device, which is a device used for industrial and commercial use. Since it would also emits radio frequencies as a byproduct of the function of its clock mechanism, the microcontroller would be considered an unintentional radiator. Because of this, it is regulated as an unintentional radiator, it requires authorization such as the Supplier's Declaration of Conformity to ensure it abides the FCC before it can be marketed \[7\].

Effective December 23, 2027, there will be a procurement ban on products using semiconductors sourced from blacklisted entities due to Section 5949 of the James M. Inhofe National Defense Authorization Act for Fiscal Year 2023 \[8\]. This could be a negligible constraint since it would only affect the market for U.S. federal government.

There are additional regulations pertaining to ensuring cybersecurity of the components being connected to the power grid. The North American Electric Reliability Corporation, also known as NERC, requires that security procedures are implemented for the reliable operation and security of the Bulk Electrical System. Under Section B 1.2.5, it requires that any software used for the MCU is verified to comply with strict cyber security standards to ensure power grid safety \[9\].

Since the microcontroller is apart of a larger fault detection apparatus, it is required by the IEEE Guide for Testing Faulted Circuit Indicators (495-2025) in Section 4 to having an operating condition of -40&degC to 85&degC \[10\].

IEEE 1613 pertains to the testing and environmental requirements of communication devices with electric power apparatus applications. for  this system deals with fault detection, the microcontroller will experience voltage surges. To increase the practicality of the device, meeting Class 1 standards in IEEE 1613 allows the device to experience temporary error as long as the device has a self-recovery system \[9\].

There is also IEEE 1613.1 that is related to transmission and distribution applications. In this document, it defines Zone B as facility with equipment outside of an enclosed area. For our purposes, the MCU would need to be prepared for Zone B use, meaning that the environmental standards due to being outside of a boundary or fenced are \[11\].

IEC-61010-1 pertains to category ratings for safety requirements of equipment used for measurement, among other uses. Since the MCU will be used for monitoring power systems, it would need to be a CAT IV rating, which requires transient protection level up to 12 kilovolts \[12\]. Because of this requirement, protection of the microcontroller will be paramount.

##### Power System Constraints

*Voltage Regulation:* The power distribution and regualtion stages must maintain a tight DC voltage tolerances with minimal ripple to prevent high frequency noise[13].

*Transient and Surge Handling:* The system must absorb rapid load steps and isolate the system from external voltage spikes or lightning surges via proper supercapacitor  buffering[14].

*Electrical and Isolation:* The analog and processing unit must maintain galvanic isolation to prevent high Ground Potential Rise and faults from damaging downstream  circuitry or creating ground loops \[15\], \[16\].

*Power Budget:* Desinged for remote, unattended deployment the systems power must be minimized to ensure the hybrid storage can sustain operation[3].

*Financial:* The total Bill of Materials needs to reamin with limits while using reliable components. 

###### Legal Constraints

- - **Regulatory and Safety Codes:** All internal wiring, power distribution, and grounding must strictly comply with NEC (NFPA 70) requirements, specifically Article 250 (Grounding and Bonding), Article 690 (Solar PV Systems), Article 706 (Energy Storage Systems), and Article 725 (Low-Voltage Control Circuits) [SS-1].
- **Thermal and Chemical Limitations:** Battery charging and discharging parameters must respect chemical limits (preventing lithium or thermal runaway under NFPA 855 guidelines) [SS-2].
- **Geographical and Site Restrictions:** Field deployment requires correct physical orientation and spacing of sensing probes relative to local telluric vectors to help with meaningful data collection.

###### Applicable Standards and Design Implementation

To ensure safe operation, signal integrity, and regulatory compliance across environments ranging from field sites to high-voltage substations, the power subsystem incorporates design criteria from key engineering codes and standards.

###### Standards Mapping and Verification Summary

| Standard / Code | Title / Focus Area | Applicable Subsystem / Aspect | Design Integration and Incorporation Strategy | Target Verification Metric |
|---|---|---|---|---|
| **NEC (NFPA 70) Art. 250** | Grounding & Bonding | Power Distribution & Chassis | Single-point reference grounding; metallic chassis bonded to earth reference. | Chassis ground resistance under $0.1\,\Omega$ |
| **NEC (NFPA 70) Art. 690** | Solar PV Systems | Auxiliary Solar / MPPT Stage | Overcurrent protection inline with PV feed; rated wire sizing and terminal isolation. | Fuse and wire sizing matches NEC ampacity limits |
| **NEC (NFPA 70) Art. 706** | Energy Storage Systems | Battery & Supercap Storage | Fused battery connections, polarity protection, and physical isolation within enclosure. | Fuse clearing and safe battery disconnect verified |
| **NEC (NFPA 70) Art. 725** | Class 1, 2, & 3 Remote-Control / Power-Limited | Low-Voltage Power Rails | Physical segregation of low-voltage DC signals ($3.3\text{V}/5\text{V}$) from high-voltage interfaces. | Minimum $6.3\text{ mm}$ PCB trace clearance |
| **NFPA 855** | Stationary Energy Storage Safety | Battery Management & Storage | Pre-certified USB battery bank with thermal shutdown; firmware temperature monitoring. | Thermal cutoff active at ≥55 °C during charging and ≥60 °C during operation |
| **IEEE Std 80** | Safety in AC Substation Grounding | Enclosure & Ground Probes | Galvanic isolation barrier prevents Substation Ground Potential Rise (GPR) from damaging microcontrollers. | Withstands $2\text{ kV}_{\text{RMS}}$ Hi-Pot test for 60 seconds |
| **IEEE Std 4** | High-Voltage Testing Techniques | Input Power Protection | Transient Voltage Suppressor (TVS) diodes and isolation barrier designed for surge withstand. | Withstands $3\text{ kV}$ transient impulse surge |
| **IEEE Std 1100** | Powering & Grounding Sensitive Electronics | Analog Front-End (AFE) Rails | Low-noise LDO filtering and isolated DC-DC converter prevent noise coupling into sensitive telluric band. | Noise floor under $10\,\mu\text{V}_{\text{RMS}}$ ($0.01\text{ Hz} - 100\text{ Hz}$) |
| **IEEE Std 1547** | DER Interconnection & Interoperability | Substation Control Interfacing | Defines isolation and power quality bounds when interfacing with utility grid buses. | Fault-free operation during grid disturbances |
| **IEEE Std C37.91** | Protective Relaying for Transformers | Substation Sensor Placement | Structural shielding and transient clamping to withstand transformer magnetic fields and fault currents. | Uninterrupted function during high EMF exposure |
| **USB 2.0 / USB-C PD Spec** | USB Power & Delivery Standard | Primary Field Input (VBUS) | Standardized 5 V DC intake interface using commercial USB power banks and wall adapters. | Voltage stable within $4.75\text{V} - 5.25\text{V}$ under load |

###### Detailed Incorporation Strategy by Standard 

*1. NEC (NFPA 70) Compliance Strategy*

- **Article 250 — Grounding and Bonding:** All metal components and external shielding will be bonded to a single reference ground terminal. Grounding paths will be verified using a low-resistance ohmmeter to confirm bonding resistance stays under 0.1 Ω.
- **Article 690 — Solar PV Systems:** For solar-connected deployments, PV input lines will include an inline Littelfuse waterproof fuse holder sized according to short-circuit current calculations to protect against DC wiring faults.
- **Article 706 — Energy Storage Systems:** Battery and supercapacitor storage will be contained within fire-retardant enclosures. Overcurrent protection will be placed adjacent to the positive terminal of the cell pack.
- **Article 725 — Low-Voltage Control Circuits:** Low-voltage DC traces (3.3 V and 5 V) on the power distribution board will be routed on dedicated internal layers with clearance distances greater than 6.3 mm, separating them from external high-voltage field connections.

*2. NFPA 855 — Energy Storage System Safety*

To comply with lithium-ion safety guidelines and prevent thermal runaway:

- Primary energy storage utilizes a pre-certified, off-the-shelf commercial power bank with integrated hardware BMS protection for short circuits, overcharging, and deep discharge.
- The sensor microcontroller tracks ambient enclosure temperature via an analog temperature sensor. Firmware will trip a power-disconnect flag if internal enclosure temperatures exceed 55 °C during charging or 60 °C during operation.

*3. IEEE Substation Safety Standards (IEEE Std 80 and IEEE Std 4)*

Ground Potential Rise (GPR) during utility faults can introduce thousands of volts between the remote ground stake and the sensor electronics.

- **Implementation:** A RECOM/Murata isolated DC-DC converter module is placed directly after the USB intake to establish a certified 2 kV RMS reinforced galvanic barrier between external inputs and internal instrumentation grounds.
- **Testing:** Compliance will be validated using a hipot tester, applying 2 kV RMS across input and output grounds for 60 seconds while verifying leakage current remains under 1 mA.

*4. Power Quality and Noise Standards (IEEE Std 1100, IEEE Std 1547, and IEEE Std C37.91)*

Sensitive telluric ground fault measurements occur in the microvolt range within a 0.01 Hz to 100 Hz frequency band.

- **Implementation:** An ultra-low-noise LT3042 LDO regulator is cascaded downstream of the switching converter to provide high PSRR (>75 dB at 100 kHz).
- **Testing:** High-frequency switching ripple will be measured using an oscilloscope with a 20 MHz bandwidth limit and a 1:1 probe, verifying that output ripple remains under 10 mV peak-to-peak and integrated noise stays below 10 μV RMS.

*5. USB 2.0 / USB-C Power Delivery Specification*

- **Implementation:** Standardized USB Type-C and Type-A input ports are integrated on the enclosure exterior, enabling field connection to portable battery packs or bench AC adapters.
- **Testing:** Voltage stability will be measured across dynamic load transitions (0 mA to 500 mA) to ensure input voltage stays within the specified 4.75 V to 5.25 V operating window.


##### Physical Design Subsystem Constraints

- The weatherproofing solution shall not electrically isolate the sensing electrode from the soil.
- The enclosure shall not introduce conductive materials near the sensing element that significantly interfere with telluric measurements.

### Survey of Existing Solutions
Fault Current Indicators are a vital part of electric power distribution. These devices serve a key role in ensuring that operations of a circuit are not violated so that power delivery and public safety are not compromised. There are numerous examples of these:

1) Schweitzer Engineering Laboratories LINAM AR360 and LINAM AR \[18\]
    - Designed for overhead distribution lines of up to $34.5$ kV and $69$ kV for the AR360 and AR, respectively.
    - Visual LED Indicators with differentiation between momentary and permanent faults.
    - greater than 1,800 flash-hours and 2,500 flash-hours for the AR360 and AR, respectively.
2) Schweitzer Engineering Laboratories LINAM UGFI \[19\]
    - Designed for underground distribution lines
    - Meets and exceeds IEEE 495 testing standards for faulted circuit indicators.
    - Sends fault status to supervisory control and data acquisition (SCADA) system.
3) Horstmann Navigator LED Series Fault Indicators \[20\]
    - Designed for overhead and dsitribution lines of up to ~$161$ kV
    - Holds memory over a 72-hour period of maximum load current sample
    - Battery status indication and 360 degree LED indication

However, these systems do not utilize telluric current readings for fault detection. With our device, the goal is to provide a non-invasive, modular, and portable solution to fault circuit indication. That is, the device shall be a contactless fault detection that utilizes telluric currents to monitor the characteristics of the circuit. 

#### Survey of Existing Solutions for Telecommunications Subsystem

For the Telecommunications design, there are five main architectures to compare:

*WiFi-Based Design* WiFi is a reliable way to wirelessly transmit information locally. Effectiveness of this system depends significantly on the distance from the sensor to the Wifi router.
- Pros: Fast data rate: ~(2 Mbps - 2.4 Gbps) as per the *IEEE 802.11 Wi-Fi protocol summary* \[21\].
- Cons: The physical range for Wi-Fi communication is very small (~90 m at maximum \[22\]).

*Cellular-Based Design:* Cellular typically requires choosing an already commercially available system, which can be potentially expensive depending on the choice. The options can be narrowed down in order of least power/data to most: NB-IoT, LTE-M, LTE/5G. 

*LoRaWAN:* Low power method for transmitting data reliably. This option would require the implementation of RF infrastructure to transfer the data across a ~10-20 km distance. If the power company already has working infrastructure, this would be a powerful option.

*Satellite:* Largest range but expensive.

*Bluetooth:*

#### Survey of Existing Solutions for Microcontroller Selection

For the MCU choices, there are four main suppliers to be compared, in addition to the option of using a laptop for the data processing.
-   -   test
Teensy Development Boards is one of the options considered for this project. It is a USB-based microcontroller development system that is compatible with Arduino Software & Libraries, compact, free software development tools, and can use either Arduino or C languages \[23\], \[24\].

Raspberry Pi is also another manufacturer with the potential of offering an ideal product for us. While there are older models in end-of-phase, such as the Raspberry Pi 2 Model B, there are other Raspberry Pi devices that could supply more computationally powerful processes while providing more portability than a laptop. However, microcontrollers are offered by Raspberry Pi, such as the Raspberry Pi Pico 2 \[25\]. Like Teensy, Raspberry Pi is  compatible with C, but MicroPython and C++ are also compatible with Raspberry Pi \[26\].

If greater computational power and language compatibilty is needed, a laptop could hypothetically be used. However, this option has major drawbacks in terms of power efficiency, portability, and size, this option would have significantly lower use cases to be implemented.

While Particle MCUs are costly option, Particle serves as an option for the MCU because of a program the company includes: if under 100 MB of data is used per month, the buyer can use cellular and WiFi across up to 100 devices for free \[27\]. 

The STM32 family of MCUs are the devices are the most familiar to this group since previous coursework involved writing to an STM32 microcontroller for labs and projects. The STM32 line also has wireless MCUs, and virtually all microcontrollers in the STM family meet the standards for the IEEE 495-2025 temperature ratings \[28\] \[29\].

The ESP32 family of MCUs are an option with built-in WiFi and Bluetooth capabilities, and is also meets the temperature ratings per IEEE 495-2025 \[30\].

#### Survey of Existing Solutions for Power Architecture 

To provide maximum adaptability for different sites, the system design outlines multiple unique power configurations regarding tradeoffs of cost, weight, longevity, and transient handling [SS-6].

##### 1. Standalone Photovoltaic Solar with Battery and Supercapacitor Bank

**Architecture:** A rugged monocrystalline solar panel charges a hybrid storage system through an MPPT charge controller. The storage combines a battery for off-grid energy storage with a parallel supercapacitor bank that absorbs high-frequency power transients, load steps, and voltage spikes.

**Pros:**
- Long-term energy storage and capacity.
- Handles a wide range of climate conditions.
- Complete galvanic isolation from substation and grid potentials, preventing ground loops.
- Supercapacitor protects the battery from high peak currents and cold-weather voltage sags.

**Cons:**
- Requires physical space for panel mounting and adequate sun exposure.
- Weather dependent.
- Hardware complexity due to the multi-stage hybrid system.

**References:** [SS-3], [SS-9]–[SS-14], [SS-16]

##### 2. Dual-Input Hybrid Power Architecture (Field Source + Bench DC Jack)

**Architecture:** A power distribution board that accepts an external field source and an indoor backup adapter. An auto-switching power multiplexer selects between them.

**Pros:**
- Bridges between a transportable field sensor and one that is easy to test on a lab bench.
- Switches automatically to wall power indoors and to the field battery outdoors.
- Same hardware for development, calibration, and field use.

**Cons:**
- Higher circuit complexity and component count on the custom board.
- Requires careful PCB layout and protection to switch sources safely.
- Wall input adds a conductive grid connection during bench use.

**References:** [SS-3]

##### 3. Rechargeable 18650/21700 Li-ion Pack with Integrated BMS

**Architecture:** A multi-cell 18650 or 21700 lithium-ion pack with a dedicated battery management system (BMS) and a low-quiescent-current buck-boost regulator that provides a clean 5 V or 3.3 V rail, followed by a low-noise LDO for the analog section.

**Pros:**
- Smaller self-supply that fits inside a small enclosure.
- Low cost and reliable for temporary deployments.
- Galvanically isolated from the grid, with no external footprint.

**Cons:**
- Finite energy capacity, requiring manual recharging or pack swaps.
- No long-term autonomous generation unless paired with solar.
- Cannot be charged safely below about 0 °C.

**References:** [SS-15], [SS-17]

##### 4. Regulated AC-to-DC Wall Adapter with Isolated SMPS

**Architecture:** An industrial-grade wall adapter plugged into 120 VAC, with high input-to-output galvanic isolation and reinforced insulation, followed by a low-noise LDO.

**Pros:**
- Primary laboratory and bench-testing supply for testing.
- Clean, uninterrupted power during development.
- Baseline noise characterization before field testing.

**Cons:**
- Wall outlet dependent, with zero field portability.
- Introduces a conductive connection to the utility grid during bench testing.

##### 5. Primary Lithium Chloride Industrial Battery Pack

**Architecture:** A non-rechargeable, high-energy primary lithium battery pack paired with a hybrid capacitor to handle high-current transmission spikes. Designed with an ultra-low self-discharge rate so it can be "installed and forgotten about."

**Pros:**
- Capable of powering low-power sensor nodes for multiple years without maintenance or recharging.
- Wide operating temperature range (-55 °C to +85 °C), making it ideal for outdoor or winter substation environments.
- Low annual self-discharge rate.

**Cons:**
- Non-rechargeable; once depleted, the pack must be replaced.
- Requires careful circuit design to prevent initial voltage sags during transmission bursts.

**References:** [SS-15], [SS-17]

##### 6. Modified Bauer 20 V Power Tool Battery with Integrated Dock and Regulation

**Architecture:** A modified Harbor Freight Bauer 20 V battery charger housing functioning as a dock and enclosure, used directly with standard Bauer slide-pack batteries. It integrates an external low-voltage cutoff circuit for cell protection, a high-efficiency synchronous buck converter to step down the 20 V pack voltage, and a post-regulation low-noise LC filter with an ultra-low-noise LDO for the analog front-end.

**Pros:**
- Mechanically robust, off-the-shelf enclosure and high-current slide-lock interface.
- Easy hot-swapping for rapid field deployment without opening the main sensor housing.
- Galvanically isolated from the utility grid during field operation.
- Low-cost and readily available battery packs and chargers.

**Cons:**
- Requires custom modification of charger guts and hand-wiring of protection/regulation boards.
- Manual battery swapping and external recharging required.
- High-frequency switching noise requires careful shielding and filtering to protect sensitive analog measurements.

**References:** [SS-7], [SS-18], [SS-19], [SS-22]

##### 7. Off-the-Shelf USB Power Bank with Isolated DC-DC and Low-Noise LDO

**Architecture:** A commercial USB power bank (portable phone charger / USB-C PD pack) providing a reliable, rechargeable lithium-ion energy source, paired downstream with an isolated DC-DC converter module to break ground loops, followed by an ultra-low-noise LDO (like the LT3042) to feed a pristine analog rail.

**Pros:**
- Solves energy storage and charging safely out of the box with an integrated BMS, short-circuit protection, and charge indicators.
- Seamless bench-to-field transition: plugs into standard USB wall blocks or laptops indoors, and a portable brick outdoors.
- Excellent signal integrity when paired with isolation and low-noise post-regulation, protecting microvolt-level telluric measurements.

**Cons:**
- Requires adding external isolation and post-regulation modules between the USB output and the sensor electronics.
- Finite capacity requiring manual recharging or swapping of the power bank.

**References:** [SS-20], [SS-21], [SS-22]

###### Survey of Existing Power Solutions
- **Standard Power Banks:** Inexpensive and portable, but prone to low-load shutoffs when the sensor sleeps, and lacks galvanic isolation.
- **Unregulated Solar to Battery:** Common in remote monitoring, but standard lead-acid or lithium setups suffer from extreme cold and lack capacity buffering for spikes [SS-6].
- **Tapped Substation Control Power:** Highly reliable due to utility backups, but directly exposes electronics to high-voltage GPR [SS-5],[SS-8].

#### Survey of Existing Weatherproofing Solutions

Custom Plastic/PVC Housing
A custom plastic housing is another possible solution. Previous research has demonstrated the use of a porous-pot electrode constructed with a thick-walled plastic tube and a modular enclosure structure \[31\]. Threaded plastic components were also used to house the wiring and other electrode components, allowing for easier maintenance, cleaning, and replacement of individual parts. The enclosure also incorporated several rubber O-rings to protect important openings, while the electrical connections were routed through sealed threaded plastic sections.

Pros:
- Cheap and material is inexpensive
- Plastic materials is nonconductive
- Size and shape can be customized for the system
- Rubber O-rings can be used to seal openings
- Design can be adapted specifically around the sensing electrode

Cons:
- Water resistance depends heavily on the quality of the build
- O-ring installation can be difficult and non-reliable
- Threaded joints and cable entry points may have weak points
- More fabrication and maintenance will need to be done over time
- Not rated for water or outdoor conditions
- No housing components supplied

EKM-17211 UL Enclosure
The EKM-1721711 is a commercially available watertight electrical enclosure designed for outdoor applications. The enclosure is made from polycarbonate and has a hinged lid, heavy gasket, and stainless steel hinges. This enclosure is rated for outdoor conditions including rain, dust and dirt. It also comes with a rail mounting system that could be used to mount the system.

Pros:
- Easily and readily available
- Polycarbonate construction is nonconductive and corrosion resistant
- Integrated drip Edge to prevent water from entering system
- Gasketed hinged around the opening making secure and water resilient
- Rail system included for mounting
- Relatively cheap
- Rated for outdoor use and water conditions

Cons:
- Cable holes would have to be drilled out and added by the team and would require gasket sealed cable connections
- Clear lid can cause internal temperatures to rise in direct sunlight
- Underground use unknown, primarily used for aboveground \[32\]

NEMA 6P Enclosure
The NEMA 6P enclosure provides the highest level of water protection among these existing solutions. It is specifically designed for outdoor electrical equipment that may be exposed to harsh environmental conditions.\[33\] This type of enclosure is suitable for submersible applications and uses waterproof gaskets to help prevent water, dust, and other contaminants from entering the enclosure.

Pros:
- Provides stronger water protection than any other existing solution
- Designed for environments where prolonged water exposure may occur. \[33\]
- Gasketed enclosure that prevent dust, water, and insects from entering the system
- Available in nonconductive materials
- Tested for outdoor use
- DIN rails, cable glands, and mounting panels come with the system
- Rated for outdoor use and water conditions
Cons:
- Slightly more expensive than a basic outdoor junction box or custom PVC enclosure
- Drill holes would need to be installed and properly sealed with rated cable glands to maintain water resistance
- Does not protect against oil and coolant leakage
- Not instantly available

### Measures of Success
#### Microcontroller Measures of Success
- Sufficient computing power to process the information received by sensor
- Withstand fault current events
- Minimal power consumption during detection
- Minimal power consumption during dormancy

#### Power Subsystem Measures of Success
- **Autonomous Uptime:** Achieve continuous 24-hour operation under load.
- **Transient Voltage Stability:** Testing through oscillating the supercapacitor layer successfully limits voltage sags during spikes.
- **Power Quality and Noise Suppression:** Verify that the output of the regulators and LDOs meet low-noise requirements, ensuring no measurable power supply leaks into the sensor's bandwidth [SS-7],[SS-22].
- **Source Checking:** Demonstrate fault-free switching between the bench DC adapter and field power source without resetting the system.


### Resources

#### Budget

##### Telecommunication Subsystem Budget

*LTE/NB-IoT Motem*: Cost: $8 - 150, depending on if it will be designed from scratch.

*Antenna* Cost: TBD; meets the band requirements of our cell line.

*SIM Provider* Cost: ~$0-$90/mo.

##### Microcontroller Budget

*MCU*: The microcontroller selected will range from $4 to $150 depending on the manufacturer selected from the previously listed options.

*Programming Software*: Any MCU obtained for this project should have free licensing whether through being free for all users or having free seats for student users.

##### Power Subsytem Budget

### 1. Energy Generation: Monocrystalline Solar Panels

*Required for Option 1.*

- **Option A — Renogy 10 W 12 V Monocrystalline Solar Panel (RNG-SOL10W):** Compact, durable glass panel with an aluminum frame ($25.00).
- **Option B — PowerFilm MP3-373 Foldable Solar Panel:** Lightweight, rugged thin-film panel optimized for remote outdoor telemetry ($45.00).
- **Option C — Eco-Worthy 25 W 12 V Monocrystalline Module:** Higher power rating for reliable charging under overcast winter conditions ($32.00).

References: [SS-9], [SS-10], [SS-11]

### 2. Charging and MPPT Controllers

- **Option A — Analog Devices LT3652 (Monolithic Solar Power Battery Charger):** Step-down battery charger IC tailored for MPPT operation ($12.00 eval/IC).
- **Option B — Texas Instruments BQ25713 (Buck-Boost Charge Controller IC):** High-performance IC supporting 1 to 4 cells with I²C control ($8.00).
- **Option C — Seeed Studio / DFRobot Solar Power Manager Micro:** Ready-to-use breakout module featuring MPPT management and protection ($15.00).

References: [SS-12], [SS-13]

### 3. Energy Storage: LiFePO₄ and Primary LiSOCl₂

*Covers Options 1, 3, and 5.*

- **Option A — K2 Energy / Generic 3.2 V 18650 LiFePO₄ Cells:** Cylindrical cells with exceptional thermal stability ($6.00 per cell; Option 3).
- **Option B — Saft LS 33600 (Primary Lithium-Thionyl Chloride D-Cell):** Non-rechargeable, high-energy-density cell for multi-year utility deployments ($22.00; Option 5).
- **Option C — 12 V 6 Ah LiFePO₄ Mini-Pack with Integrated BMS:** Drop-in sealed pack for solar-hybrid setups ($35.00; Option 1).

References: [SS-14], [SS-15]

### 4. Supercapacitors and Hybrid Layer Capacitors (HLC)

- **Option A — Eaton / Bussmann HV Series 2.7 V 100 F Cylindrical Supercapacitor:** High capacitance for smoothing peak loads ($8.00).
- **Option B — Kemet / Maxwell Technologies 3.0 V 50 F Prismatic Supercapacitor Cell:** Low ESR cell suited for fast transient response ($7.50).
- **Option C — Tadiran Hybrid Layer Capacitor (HLC-1520):** Designed to pair with LiSOCl₂ cells to assist with pulse demands ($12.00).

References: [SS-16], [SS-17]

### 5. DC-DC Regulators and Power Management ICs

- **Option A — Texas Instruments TPS63060 (High-Efficiency Buck-Boost Converter):** Maintains stable output when input voltage fluctuates ($5.00).
- **Option B — Texas Instruments LMZ31704 (Power Module with Integrated Inductor):** Simplifies layout and reduces high-frequency switching noise ($9.00).
- **Option C — Analog Devices / Linear Technology LT3042 (Ultra-Low-Noise, High-PSRR LDO):** Placed downstream to reduce remaining ripple ($4.00).

References: [SS-22]

### 6. Auxiliary Interface and Protection Hardware

*Supports Option 2.*

- **Option A — Texas Instruments TPS2121 (Active Power Multiplexer IC):** Auto-switching power multiplexer with priority selection ($6.00).
- **Option B — CUI Devices PJ-102AH (DC Barrel Jack Connector):** Standard indoor lab-bench power connection port ($1.50).
- **Option C — Littelfuse 313 Series / Bussmann Waterproof Inline Fuse Holders:** Overcurrent protection required by NEC Articles 240 and 706 ($3.00).

References: [SS-1]

### 7. Cordless Drill Battery Setup: Harbor Freight Bauer 20 V System

*Supports Option 6.*

- **Option A — Harbor Freight Bauer 20 V Battery Pack (2.0 Ah to 5.0 Ah slide pack):** Store-bought rechargeable slide battery ($30.00–$60.00).
- **Option B — Modified Bauer 20 V Charger Shell and Dock:** Empty, hollowed-out charger case acting as a secure plastic holder ($15.00 donor charger).
- **Option C — Safety Cutoff and Voltage Regulator Board:** Circuit board stepping down high battery voltage while preventing over-discharge ($12.00).

References: [SS-18], [SS-19]

### 8. Portable USB Power and Isolation Modules

*Supports Option 7.*

- **Option A — Commercial USB-C Power Bank (10,000 mAh–20,000 mAh):** Certified rechargeable lithium battery pack with built-in safety management ($20.00–$40.00).
- **Option B — RECOM / Murata 1 W Isolated DC-DC Converter Module:** Compact board-mount module providing galvanic isolation to eliminate ground loops ($8.00–$12.00).
- **Option C — Analog Devices / Linear Technology LT3042 Ultra-Low-Noise LDO Breakout Board:** High-PSRR linear regulator supplying a battery-quiet rail ($10.00–$15.00).

References: [SS-20], [SS-21], [SS-22]

#### Personnel

Alex Bailey: SolidWorks, C/C++, MATLAB, LTSpice, soldering, Power
Electronics, Electromagnetics
Silas Garmon: AutoCAD, C/C++, VHDL IEEE Std 1076-2019, MATLAB, LTSpice, Power System Analysis, Allen-Bradley PLC, PLS-CADD
Mark Myers: SolidWorks, C/C++, MatLAB, LTSpice,  Allen-Bradley PLC,  Induction Specialized, Power Systems
Seth Strahan: Power Courses, System Analysis, Small and Large system power distribution through internship work
Christopher Wolk: C/C++, MATLAB, LTSpice, Power Systems, Controls Design, Allen-Bradley PLC.

Limitations: None of the Seal Team 2 members have expertise in either Embedded Systems or Telecommuncations. To address these gaps in understanding, we will contact Mohammad Moltafet and Tarek Elfouly.

The supervisor selected for this project is Dr. Charles Van Neste. His
research experience is primarily in wireless power transfer and
electromagnetic fields. Under him, research student Abigail Farris first
researched this science of fault detection through telluric currents.
His skills in supplying power through the soil and analyzing telluric
currents will prove most beneficial.

Dr. Storm Johnson is the instructor over this project. He earned his
doctorate under Dr. Van Neste and is therefore fully equipped to provide
insight to the finer aspects of our project akin to communication with
and expectation of Dr. Van Neste. He will also serve as quality control
and pacing for the duration of this project because of his previous
experience as head of senior design.

#### Timeline
Preface: We need to get together and discuss a timeline in more detail because the Gantt Chart we have is outdated, the midterm documents will include the updated Gantt Chart. 

gantt
    title Project Timeline
    dateFormat  YYYY-MM-DD
    
    section Planning
    Phase 1 :active, p1, 2026-06-01, 10d
    
    section Development
    Phase 2 :active, p2, after p1, 15d
    
    section Testing
    Phase 3 :crit, active, p3, after p2, 10d
    
    section Deployment
    Phase 4 :active, p4, after p3, 5d

### Specific Implications
Dr. Charles Van Neste is the customer for this project. The customer requires a system that can provide near real-time measurements of voltage differences in the ground and make those measurements accessible from a cellular device. Dr. Van Neste's research focuses heavily on energy generation and transmission, including wireless and quasi-wireless power transfer, high-frequency inverter design, electronic instrumentation, and electromagnetic interactions.

During this research, unexpected or cross-coupled voltage readings can make it difficult to determine whether a signal is produced by the experimental system or by another source in the surrounding environment. One possible source is naturally occurring or infrastructure-related telluric ground current, including current associated with nearby electrical substations and grounding systems. The purpose of this project is therefore to provide Dr. Van Neste with a dedicated telluric sensing system that can continuously monitor differences in ground potential. By comparing these measurements with experimental data, the customer will be better able to determine whether additional voltage is present in the ground and distinguish background ground-current effects from the signals produced by his wireless power-transfer experiments.


### Broader Implications, Ethics, and Responsibility as Engineers


Broader Implications:
- Power Grid Stability and Transformer Overheating: Geomagnetically Induced Currents (GICs) flow thorugh a neutral to ground connection. This DC offset causes half-cycle saturation, generates high harmonics, reactive power consumption, and local transformer overheating. A telluric sensor can serve as a good early warning for grid operators. 
- Public Safety and Fire Prevention: Ground faults and earth currents occasianlly iginite vegetation or damage electrcial equipment. Earlier detection timing can help to reduce these wildfires and outages.
- Worker Safety: Undetected faluts cause step and touch potential hazards for line crew and electricans. 
- Access and equity: Low cost, self-powered sensors could extend monitoring to rural or low resourced utilites that cannot afford large SCADA upgrades. 
- Economic Impact: The up front cost of the sensor would outweigh the long term cost of outages, equipment damage, and liability. 
- Pipeline Integrity and Enviromental Containment: Long steel oil and gas pipelines, telluric currents disrupt Cathodic Protection (CP) systems. They shift the potential in the soil in around the pipe that accelerates metal loss and pinhole corrosion. Monitoring the telluric noise can help catch pipeline integrity and oil containment due to this. 
- Cybersecurity and critical infrastructure: The sensor reports data over a network, a compromised device could cause false alarms or hide real data faults.

Ethics: 
- Honest Performance claims: Report detection limits, sensitivity, and failure modes accuratley, not just the best case results. Overstating reliablity on a safety device is a ethical issue. 
- Fail-Safes: If the power is lost, the battery degrades, or a component fails. The sensor should indicate its own failure rather than producing flase or weak reading.
- Codes of Ethics: Cite IEEE Code of Ethics, and the NSPE Code of Ethics.
- Data and Privacy: Logs that transmit across shared grid data should state who owns it and how its protected.
- Intellectual Property and Attributions: Give credit to prior work and individuals whom helped fairly. 
- Data Integrity vs. Alarm Fatigue: Sensor data must accurately decouple true earth fault conditions. Misclassifying a telluric wave as noise risks missing genuine dangerous ground fault currents.
- Affordability and Safety: Reducing cost should not come at the expense of essential safety and reliability. Balance affordability with dependable measurments. 

Responsibility as Engineers:
- Testing and Validation: Commit to documented testing under real conditions beofre claiming it works.
- Risk assessments: Identify hazards and how to mitigate them.
- Accountability: Engineering team review of team members individual work and subsytems. 
- Lifecycle and Future Process: Plan for maintenence, battery replacments, firmware updates, and disposal. 
- Knowing system and individual limits: State when the design needs review and know limits of the desgin; do not over-sell the product. 
- Accurate Reporting: Document the sensors detection range, accuracy, and limitations. A prototype is not a finsihed product and should not be adverties as so.
- Following Standards: Identify appicable electrical safety, grounding, and instrumentations standards. Determine additioanl testing and/or certifications would be needed before real-world deployment. 
- Protecting the Public: Prioritze public and worker safety through design. Consider consequences of missed detections, incorret readings, and procedures.


### References
\[1\] A. C. Farris, W. A. Stump, C. S. Johnson, R. Craven, and C. W. Van
Neste, "Grid-induced telluric currents for non-contact load monitoring
and fault detection," in *Proc. 2023 IEEE Energy Conversion Congress and
Exposition (ECCE)*, 2023, pp. 1248--1252, doi:
10.1109/ECCE53617.2023.10362635.

\[2\] “47 C.F.R. § 2.1046, Measurements required: RF power output,” Code of Federal Regulations, Title 47, Part 2. [Online]. Available: 
https://www.ecfr.gov/current/title-47/section-2.1046 . [Accessed: Oct. 9, 2026].

[3] D. Linden and T. B. Reddy, Handbook of Batteries, 3rd ed. New York: McGraw-Hill, 2002.

[4] “47 C.F.R. § 2.911, Application requirements,” Code of Federal Regulations, Title 47, Part 2. [Online]. Available: 
https://www.ecfr.gov/current/title-47/section-2.911 . [Accessed: Oct. 9, 2026].

[5] “47 C.F.R. § 2.1033, Application for certification,” Code of Federal Regulations, Title 47, Part 2. [Online]. Available: 
https://www.ecfr.gov/current/title-47/section-2.1033 . [Accessed: Oct. 9, 2026].

[6] “47 C.F.R. § 2.1057, Frequency spectrum to be investigated,” Code of Federal Regulations, Title 47, Part 2. [Online]. Available: 
https://www.ecfr.gov/current/title-47/section-2.1057 . [Accessed: Oct. 9, 2026].

\[7\] Telecommunication, Code of Federal Regulations, title 47, pt. 15, subpts. A–B, 2026. \[Online\]. Available: https://www.ecfr.gov/current/title-47/chapter-I/subchapter-B/part-15

\[8\] Prohibition on Certain Semiconductor Products and Services, Section 5949 of the James M. Inhofe National Defense Authorization Act for Fiscal Year 2023, Pub. L. 117-263, 136 Stat. 3165.

\[9\] NERC, “Cyber Security — Supply Chain Risk Management,” NERC Standard CIP-013-2, 2022. \[Online\]. Available: https://www.nerc.com/pa/Stand/Reliability%2520Standards/CIP-013-2.pdf

\[10\] "IEEE Standard for Testing Faulted Circuit Indicators," in IEEE Std 495-2025 (Revision of IEEE Std 495-2007) , vol., no., pp.1-26, 27 Feb. 2026, doi: 10.1109/IEEESTD.2026.11415435.
keywords: {IEEE Standards;Cables;Fault diagnosis;Circuit stability;Circuit testing;cable fault locator;fault indicators;faulted circuit indicator;FCI;IEEE 495 (TM)},

\[11\] "IEEE Standard Environmental and Testing Requirements for Communications Networking Devices Installed in Transmission and Distribution Facilities," in IEEE Std 1613.1-2013 , vol., no., pp.1-22, 20 Dec. 2013, doi: 10.1109/IEEESTD.2013.6687184.
keywords: {IEEE standards;Communications services;Telecommunication network management;Oscillators;Magnetic fields;Radiation effects;Hazards;Surge protection;Damping;common-mode disturbances;communications networking device;conducted RF;damped oscillatory magnetic fields;IEEE 1613.1(tm);immunity;power frequency magnetic fields;radiated;RFI hazards;surge},

\[12\] J. VIOX Editorial Team, “Understanding CAT Ratings: Essential Knowledge for Electrical Safety,” VIOX Electric, Dec. 23, 2025. \[Online\]. Available: https://viox.com/cat-ratings-explained-electrical-safety-guide/. \[Accessed: Oct. 8, 2026\].

[13] IEEE, IEEE Recommended Practice and Requirements for Harmonic Control in Electric Power Systems, IEEE Std 519-2022, 2022.

[14] IEEE, IEEE Recommended Practice on Surge Voltages in Low-Voltage AC Power Circuits, IEEE Std C62.41.2-2002, 2002.

[15] IEEE, IEEE Guide for Measuring Earth Resistivity, Ground Impedance, and Earth Surface Potentials of a Grounding System, IEEE Std 81-2012, 2012.

[16] IEEE, IEEE Recommended Practice for Grounding of Industrial and Commercial Power Systems, IEEE Std 142-2007, 2007.

[17] National Fire Protection Association, NFPA 855: Standard for the Installation of Stationary Energy Storage Systems, Quincy, MA: NFPA, 2026.

\[18\] Schweitzer Engineering Laboratories, “SEL-AR Overhead AutoRANGER Fault Indicator,” SEL. [Online]. Available: https://selinc.com/products/ar/. [Accessed: Oct. 9, 2026].

\[19\] Schweitzer Engineering Laboratories, “LINAM UGFI Underground Fault Indicator,” SEL. [Online]. Available: https://selinc.com/products/linam-ugfi/. [Accessed: Oct. 9, 2026].

\[20\] Dipl.-Ing. H. Horstmann GmbH, “Navigator LM | Navigator LM HV,” Horstmann. [Online]. Available: 
https://horstmanngmbh.com/en/products/overhead-faulted-circuit-indicators/without-remote-signal/navigator-lm-navigator-lm-hv/ . [Accessed: Oct. 10, 2026].

[21] IEEE, IEEE Standard for Information Technology—Telecommunications and Information Exchange between Systems—Local and Metropolitan Area Networks—Specific Requirements—Part 11: Wireless LAN Medium Access Control (MAC) and Physical Layer (PHY) Specifications, IEEE Std 802.11-2024, Apr. 2025. [Online]. Available: https://standards.ieee.org/ieee/1531/10548/.

[22] EPB, “How Far Will Your Wi-Fi Signal Reach?” EPB. [Online]. Available: https://epb.com/get-connected/gig-internet/how-far-will-your-wi-fi-signal-reach/. [Accessed: Oct. 9, 2026].

\[23\] PJRC, “Teensy USB Development Board,” PJRC. \[Online\]. Available: https://www.pjrc.com/teensy/ (accessed Oct. 8, 2026).

\[24\] PJRC, “Teensy Language Support,” PJRC. \[Online\]. Available: https://www.pjrc.com/teensy/languages.html (accessed Oct. 8, 2026).

\[25\] Raspberry Pi Ltd., “Buy a Raspberry Pi,” Raspberry Pi. \[Online\]. Available: https://www.raspberrypi.com/products/ (accessed Oct. 8, 2026).

\[26\] Nabto, “Getting Started with Raspberry Pi Pico Programming,” Nabto. \[Online\]. Available: https://www.nabto.com/raspberry-pi-pico-programming/ (accessed Oct. 8, 2026).

\[27\] Particle, “Cellular Data Guide,” Particle Documentation. \[Online\]. Available: https://docs.particle.io/getting-started/billing/cellular-data/. \[Accessed: Oct. 8, 2026\].

\[28\] STMicroelectronics, “STM32 wireless MCUs,” STMicroelectronics. \[Online\]. Available: 
https://www.st.com/en/microcontrollers-microprocessors/stm32-wireless-mcus.html#st-featured-products . \[Accessed: Oct. 9, 2026\]. 

\[29\] STMicroelectronics, “Microcontrollers & microprocessors,” ST Online Store. \[Online\]. Available: 
https://estore.st.com/en/products/microcontrollers-microprocessors.html?cat=22198&core=17024 . \[Accessed: Oct. 9, 2026\].

\[30\] Espressif Systems, “ESP32 Wi-Fi & Bluetooth SoC,” Espressif Systems. \[Online\]. Available: https://www.espressif.com/en/products/socs/esp32. \[Accessed: Oct. 9, 2026\].

\[31\] M. J. Comeau, S. Ueding, and M. Becken, “Long-term stability, noise, and temperature sensitivity of modular porous-pot electrodes designed for geophysical and geotechnical applications, and details of their construction,” Earth and Space Science, vol. 11, Art. no. e2023EA003327, 2024, doi: 10.1029/2023EA003327.

\[32\] EKM Metering Inc., “Large Watertight Enclosure with Hinged and Latching Lid – UL Listed – 6.7 in. × 10.63 in. × 4.33 in.,” EKM Metering. \[Online\]. Available: EKM watertight enclosure product page. \[Accessed: Oct. 2, 2026\].

\[33\] National Electrical Manufacturers Association, “NEMA enclosure types,” NEMA. \[Online\]. Available: https://www.nema.org/docs/default-source/products-document-library/nema-enclosure-types.pdf. \[Accessed: Oct. 5, 2026\].

[SS1] National Fire Protection Association, NFPA 70: National Electrical Code, Quincy, MA: NFPA, 2026.

[SS3] IEEE, IEEE Recommended Practice for Powering and Grounding Sensitive Electronic Equipment, IEEE Std 1100-2005, 2005.

[SS9] R. P. Deshpande, The Relay Protection of High Voltage Networks, 2nd ed. London: Institution of Engineering and Technology, 2020.

[MW.5] CASRAI Editorial Board, “Limit of Detection vs Limit of Quantitation: How to Calculate LOD and LOQ,” CASRAI, Aug. 26, 2026. [Online]. Available: https://casrai.org/guides/limit-of-detection-vs-limit-of-quantitation-calculation. [Accessed: Oct. 9, 2026].

[MW.6] P. L. Dordal, “1.2: Data Rate, Throughput and Bandwidth,” in An Introduction to Computer Networks, Engineering LibreTexts. [Online]. Available: https://eng.libretexts.org/Bookshelves/ComputerScience/Networks/AnIntroductiontoComputerNetworks%28Dordal%29/01%3AAnOverviewofNetworks/1.02%3ADataRateThroughputand_Bandwidth. [Accessed: Oct. 9, 2026].

### Statement of Contributions
Alexander Bailey: Introduction, Background, Personnel, References

Silas Garmon: Editing of Introduction, Personnel, Specifications, Constraints, Existing Solutions, References, Measures of Success, Formatting, Budget

Mark Myers: Editing of Introduction, Personnel, Specifications, Constraints, Existing Solutions, References, Specific Implications

Seth Strahan: Budget, Personnel, Specifications, Constraints, Existing Solutions, References, Ethics, Broader Implications, Responsibility as Engineers

Christopher Wolk: Budget, Specifications, Constraints, Existing Solutions, References

# Conceptual Design
## Introduction

EXCERPT OF ALEX'S WORK

Across the globe,  electrical power is supplied from source to recipient via power lines. These lines carry single and three phase power while often carrying telecommunication signals below on a separate section. If these power lines fail, it can result in loss of magnitude of power (brownout) or loss of power altogether (blackout). This can happen from a transformer blowing up, power lines grounding through trees, or power lines falling either into other lines or onto the ground. Any instance which results in unprompted behavior is called a fault. Currently, the main method in locating a fault is by mobilizing service workers and garnering information from customer calls \[1\]. This, as one can imagine, is quite costly both monetarily and temporally.

In light of this challenge, **Seal Team 2** shall develop an affordable, replicable prototype sensor which will relay information pertaining to the induced telluric currents. This information shall be sent across cell lines to the company supplying and managing the area's power. Such information will be primarily contained to fault detection for the scope of this prototype. This sensor will take information on the voltage and telluric current density surrounding the power pole, filter the data so that only the $60$, $180$, and $540$ Hz harmonics are considered, then process the data and signal a fault if necessary. This prototype will include electrodes, an analog filter, a PID controller, and a cellular communicator.
## Restating the Fully Formulated Problem
## Comparative Analysis of Potential Solutions

The following section provides Seal Team 2's comparative analysis of potential solutions for designing the telecommunication subsystem of the Telluric Current Sensor. This section will begin by determining and justifying the choice of cellular as the medium for which the data will be transmitted. This section will then compare 5 choices for a cellular hardware module as well as 5 choices for a connectivity/SIM module.

### Comparison of Communication Media

For the purposes of Seal Team 2's telcommunication subsystem: the following 4 communication media: LoRaWAN, Wi-Fi, Bluetooth, and Cellular will be weighed under four categories: general power consumption, range, accessibility, cost to determine the optimal media for the Telluric Current Sensor.

-LoRaWan Module: LoRaWAN, or Long Range Wide Area Networking, is a wireless platform and network catered towards the transmission of IoT device data. According to SemTech, the developer of the physical LoRa module Hardware, LoRa is optimized to deliver data over a long range with minimal power consumption [1][Semtech]. SemTech also boasts a wide range for their devices allowing for data transmission up to ~3km. As for accesibility, according to ttnmapper.org, a website that provides a heatmap of all public LoRaWAN Gateways on earth, Cookeville, Tenessee does not have any public LoRaWAN coverage (Refer to Figure MW.1) [2]. This would require Seal Team 2 to implement a private local gateway at the substation, which could be costly and require the use of the companies power supply, thereby nullifying the low power constraint imposed by the customer, Dr. Van Neste.

-WiFi Module: A Wi-Fi based design is another potential candidate for the design. Modules such as the ESP32-WROOM-32E house a Wi-Fi module and a microcontroller in one device. This could reduce component count and simplify the firmware development as compared to the other options. However, upon review of the datasheets of existing Wi-FI modules, the average power output during the active mode of Wi-Fi data transmission is much larger in comparison to the LoRaWan module (See Table 16: Current Consumption Depending on RF Modes from the ESP32-WROOM-32E datasheet [3]). Furthermore, locally housed Wi-Fi modules generally are designed to operate locally, and the >1km range specification from the customer exceeds these modules operating range.

-Bluetooth Module: In the typical use case, devices that use Bluetooth do not operate over a wide range. However, upon futher review, Seal Team 2 found Bluetooth Low Energy, which offers low-power communication over a longer range via forward-error-correction coding at the expense of data rate.[4][: Bluetooth low energy primer]. Bluetooth does emphasize that the practical range of the transmission still depends heavily on the radio configuration, antenna characteristics, receiver sensiticity, and therefore establishing a >1 km link could require specialized hardware that could be expensive. 

-Cellular Module: Cellular modules, LTE-M and NB-IoT technologies in particular, pose as strong candidates for the wireless communication module. Unlike the other three potential solutions, cellular communication does not ordinarily require the project team to intall and maintain a receiving gateway due to the existing cellular network infrastructure. Choosing a cellular module does, however, introduce the need for a connectivity/SIM service which introduces an additional periodic cost for the product. However, since our project sends minimal data, (three FFT amplitudes and a fault indicator), this payment can be minimized, *See Connecticity/SIM Analysis Subsection. Furthermore, the efficiency of a cellular transmission varies heavily on the choice of cellular technologies and generally the power output in the active mode is higher than that of the LoRa Module.

To determine the Wireless Communication media that will be chosen for the Telluric Ground Fault Indicator, the following decision matrix was made, assigning a quality point from 1-5 for each of the parameters: general power consumption, range, accessibility, cost to each of the four candidates per the analysis above. The quality points are determined as follows:
    1 - Fails to meet specifications and constraints.
    2 - Partially meets the specifications and constraints.
    3 - Can meet specifications and constraints under the condition that further hardware is implemented.
    4 - Meets all specifications and constraints in regular conditions.
    5 - Meets all specifications and constraints under all relevant conditions.

| Technology | Power consumption | Range | Accessibility | Cost |
| ---------- | ----------------- | ----- | ------------ | ---- |
| LoRaWAN | 5 | 4 | 2 | 4 |
| Wi-Fi | 2 | 2 | 4 | 4 |
| Bluetooth LE | 5 | 2 | 3 | 5 |
| Cellular (LTE-M/NB-IoT) | 3 | 5 | 5 | 3 |

By the above analysis, Seal Team 2 shall choose a cellular module (LTE-M/NB-IoT) as the communication media for the Telecommunication Subsystem.

### Comparison of the Cellular Hardware Module

The following 5 options were picked as potential candidates for the Cellular Hardware Module of the Telluric Current Sensor. These options were chosen based on their availability for NB-IoT or LTE-M transmission and will be weighed based on the following 5 parameters: Power consumption, Ease of MCU integration, Cellular capability/Flexibility, Cost effectiveness, Compactness.

-RAKwireless RAK5860 (LTE-M + NB-IoT): The RAK5860 module is cellular extension module designed by the WisBlock IoT dvelopment platform. The module utilizes a Quectel BG77 and can provide NB-IoT and LTE-M connectivity. When connected to the Wisblock Core, this system can operate as a microcontroller and cellular system, which would eliminate having to manage complicated RF PCB design. This choice of microcontroller, however, is completely restricted as any other microcontroller-to-RAK5860 module design would require an RF PCB design. The power consumption of the system in active mode is approximately 200 mA as per Table 44 in the BG95 Series QuecOpen Hardware Design document [5]. The module costs ~$30.00.

-M5Stack SIM7080G (LTE-M + NB-IoT): The M5Stack Unit CatM is another cellular connectivity module based on the SIMCom SIM7080G modem, which also supports the desired connectivities for low-bandwidth IoT applications. One noteable drawback of this device is its standby power consumption, which is rated at 49.4 mA [6]. This option costs ~$36.90

-Nordic Semiconductor nRF9160 (LTE-M + NB-IoT) The Nordic Semiconductor nRF9160 is another strong candidate for the Telluric Sensor System, particularly because the board boasts its low power cellular operation specifying a PSM floor current of 2.7$\mu$A. Furthermore, similar to the RAKwireless RAK5860 module + WisBlock Core, this board is a single MCU + modem unit, so there is no need for any complicated RF design. As such, this property also implies the tradeoff that the modem could not be connected to a separate microcontroller, if a more MCU option exists.

-Quectel BG95 Family (LTE-M: Cat M1 + NB-IoT: Cat NB2): Quectel BG95 Family is a collection of multiple different LTE-M/NB-IoT compatible cellular modules. The main advantage of these modules are that they operate with competitively low power consumption ratings, around ~200 mA in the active mode of operation and <1$mu$A in sleep mode [7]. This module, however cannot connect modularly with an MCU, and RF PCB design would be required.

-Quectel BG77 (LTE-M: Cat M1 + NB-IoT: Cat NB2): The Quectel BG77 is the standalone chip in the RAKWireless RAK5860. Similar to the BG95 modules, this module operates with conserably low power consumption. In fact, this module operates consistently at consistently lower power rates than any of the BG95 modules (*See [5]). Furthermore, the BG77 is more compact than the BG95, making it more favorable for the Telluric Current Sensor's size constraints. This module also cannot connect modularly to an MCU, and an RF PCB design would be required.

#### Determination of weights

The 5 modules above will be assigned quality points in each of the 5 categories listed above by the same metrics shown in the Comparison of Communication Media subsection. Due to the varying level of importance for satisfying the 5 parameters, the following weights will be assigned:

-Due to the low power specifications from Dr. Van Neste, power consumption will have a larger weight of 1.5.
-Due to the lack of expertise in RF PCB design, MCU integration will have a larger weight of 2.
-Due to the need for effective cellular transmission, Cellular capability/Flexibility will have a larger weight of 1.5.
-The cost of most of these boards telecommunication modules are well within the budget maximum of $1000, so cost will be weighed normally.
-The compactness of the subsystem is important to meet the restrictions of the physical housing system; compactness will be weighed as a 1.

| Module | Power × 1.5 | MCU integration × 2 | Cellular × 1.5 | Cost × 1 | Compactness × 1 | Total / 35 |
|---|---:|---:|---:|---:|---:|---:|
| RAKwireless RAK5860 | 6 | 8 | 6 | 4 | 4 | **28** |
| M5Stack SIM7080G Unit CatM | 3 | 10 | 6 | 4 | 3 | **26** |
| Nordic Semiconductor nRF9160 | 7.5 | 10 | 6 | 3 | 4 | **30.5** |
| Quectel BG95 Family | 6 | 4 | 7.5 | 5 | 4 | **26.5** |
| Quectel BG77 | 7.5 | 4 | 6 | 5 | 5 | **27.5** |

From the results of this comparative analysis Seal Team 2 shall choose the **Nordic Semiconductor nRF9160** as the Cellular Hardware Module of the Telecommunications subsystem of the Telluric Ground Sensor.

### Comparison of the Connectivity/SIM Module

According to the nRF9160 DK setup guide, an iBasis eSIM is preloaded with 10MB. Seal Team 2 shall, by default, choose this connectivity, as this eSIM supports both LTE-M and NB-IoT.

### Statement of the Telecommunication Subsystem Solution

From the analyses above, Seal Team 2 shall use the Nordic Semiconductor nRF9160 as a microcontroller + telecommunications unit to transmit the sensor data via Cellular LTE-M/NB-IoT connectivity to the recipient (customer, research participants, etc.).

## Comparative Analysis of Potential Solutions

This section evaluates potential power architectures, discusses key technical and operational design considerations, and outlines the primary factors influencing the final solution selection. It provides a structured evaluation to identify and justify the chosen power system architecture.

### 1. Design Considerations and Selection Factors

Designing a power supply system for a field-deployable telluric ground fault sensor requires balancing several engineering constraints. Because telluric sensors measure ultra-low-frequency, microvolt-level earth potential differences, traditional power design priorities must be ordered to protect instrumentation sensitivity:

- **Signal Integrity and Electrical Noise:** High-frequency switching ripple ($f_{\text{sw}} > 100$ kHz) from unshielded DC-DC converters can easily alias into the sensor's analog acquisition band (0.01 Hz to 100 Hz). The power architecture must support post-regulation ripple suppression to under 10 mV peak-to-peak, with target analog rails requiring less than 10 μV RMS noise [SS-22].
- **Galvanic Isolation:** High Ground Potential Rise (GPR) near utility substations during ground faults creates severe safety hazards and ground loops. The power source must maintain reinforced isolation (minimum 2 kV) to decouple the internal sensing ground from external grid potentials [SS-5],[SS-8],[SS-21],[SS-3].
- **Bench-to-Field Adaptability:** The system must seamlessly transition between long-term laboratory testing on utility AC power and standalone, transportable field deployments without requiring hardware redesign.
- **Implementation Complexity and Cost:** To keep the total Bill of Materials (BOM) within capstone budget constraints, the design must avoid unnecessary custom battery management system (BMS) development where certified commercial solutions exist [SS-20].
- **Regulatory and Safety Compliance:** All energy storage and distribution wiring must strictly adhere to NEC (NFPA 70) Articles 250, 706, and 725, as well as NFPA 855 safety parameters for lithium-based storage [SS-2].

### Comparative Analysis of Potential Solutions for the Microcontroller

The microcontrollers that are the main options to consider are the STM32U5, the TI MSP430, the Nordic nRF9161, the ESP32-S3, the Particle BSoM B404, the Raspberry Pi Pico 2, and Teensy 4.1. There is also a laptop option included as a control option. These options will be compared using select key parameters.

#### Parameters 
For one, this microcontroller must be efficient with power consumption. Since sampling data periodically increases the chances of a fault occuring and not being detected, continuous sampling would need to be used. Because of this, power efficiency is paramount, with a need for low current usage and sleep modes as apart of the system. For this reason, the power efficiency is weighed at 35%. An option receiving a score of 1 would be a device that draws current in the upper milliamps during active modes and lacking sleep mode options. On the other end, a system receiving a 5 would be a system with a robust sleep mode and drawing current in the microamp to sub-microamp range.

Longevity and robustness will be defined as ability to meet the IEEE 495-2025 standard of operating temperature of devices used for fault current indication rated at -40\degC to 85\deg C, and also whether the market for the option is intended for industrial use as opposed to educational or commercial use [SG11]. This is category is important because to market this project, it needs to meet industry standards. For this reason, longevity and robustness is weighed at 30%. An option receiving a score of 5 is a product that exceeds the desired ratings and is tailored for industrial applications, while an option receiving a 1 would be intended for personal/commercial use and failing to meet IEEE 495-2025 standards on operating temperatures.

Cost and practicality will be defined as a combination of the practicality of the option as a part of a device used in field testing as well as overall cost of the microcontroller. This category is not as important as parameters such as power efficiency or longevity and robustness, so it is weighed at 15%. An option receiving a 5 is one that is low cost, highly portable, and compact in size, while an option receiving a 1 is an option that can either cost significantly more than the other options or has considerable size differences from the other options.

Computational power will be defined as the microcontroller's ability to compute efficiently for the purposes it is designed to complete. Since the MCU's biggest computational task will be an FFT and sending information, this category does not need to be weighed as highly as the others, meaning that it will only be weighed at 10%. An option receiving a 5 is an option that can perform significantly higher than what is expected of it, and an option receiving a 1 is an option that would not be viable for this project.

Ease of Cellular Module Communication Configuration Set Up will be defined as the compatibility and relative ease of setting up the microcontroller with cellular communication. Because a separate modem is a viable option, this category will only be weighed at 10%. An option receiving a score of a 1 would be an option that is incompatible with cellular communication at the start or would have a steep configuration process, while an option receiving a 5 is an option that has the most direct set up process.

#### Power Efficiency
- The STM32U5 is apart of the ST's ultra-low-power line of microcontroller options. This component has a 19.5 μA/MHz Run mode and has options for a low-power background autonomous mode (LPBAM) which allows for peripherals to operate in Stop mode, which means the peripherals can operate without software running. With these capabilities, the STM32U5 serves as a strong contender in terms of power efficiency [SG1]. For this reason, the STM32U5 receives a 5 in this category.
- The TI MSP430 is a device that has a DMA controller that allows for movement of data without the CPU, meaning that data can move around the peripheral without exiting sleep mode. This device draws 0.35 µA when in Real-Time Clock (RTC) mode, and has an even lower current in shutdown mode of 0.02 µA [SG2]. For this reason, the TI MSP430 receives a 5 in this category.
- The Nordic nRF9161 is a device with adequate power efficiency, consuming under 3 mA during active modes and consumes in the microamp range during sleep mode [SG3]. Because of this, the nRF9161 receives a 4.
- The ESP32-S3 has sleep mode capabilities with sufficient power efficiency, but during active operations there is higher current drawing within the tens milliamp of range [SG4]. Therefore, a 3 is alloted in this option.
- The Particle BSoM B404 is similar to the ESP32-S3 in terms of power consumption. It also has low consumption during sleep modes, but higher active mode power consumptions in the tens of milliamps range, primarily because of the cellular module on the device [SG5]. Also similar to the ESP32-S3, this MCU will get a 3 in this category.
- The Raspberry Pi Pico 2 does not perform at the same level as the other MCUs listed, as its base low-power mode consumes more current than the others and draws milliamps during active operations [SG6]. For this reason, The Pico 2 will receive a 3. REVIEW THIS
- The Teensy 4.1 draws 100 mA of current at a clock rate of 600 MHz, which indicates that the Teensy 4.1 performs poorly at power efficiency [SG7]. For this reason, the Teensy 4.1 will receive a 1 in this category.

#### Longevity & Robustness
- The STM32U5 also has an exemplary operating rating of -40\deg C to 85\deg C, with even some variants operating up to 125\deg C, proving itself to be a more than sufficient candidate in terms of surviving industrial conditions [SG1]. For this reason, this MCU will receive a 5 in this parameter.
- The TI MSP430 also meets the IEEE 495-2025 standards of environmental ratings, with the ability to operate at temperatures of -40\deg C to 85\deg C, making it reliable for this project [SG2]. Thus, the TI MCU will receive a 4.
- Like the TI MCU, the Nordic MCU also meets the IEEE 495-2025 standards of environmental ratings, which indicates this MCU as being sufficient for this category [SG3]. Thus, it is alloted a 4.
- While the ESP32-S3 does meet the temperature ratigns of IEEE 495-2025, the modules it is paired with may not depending on the product, and this MCU is not intended for industiral compared to other MCUs like the STM32U5 and the TI MSP430 [SG4]. Becayse of this, the MCU will be alloted a 3. REVIEW THIS
- The BSoM B404 has a recommended operating range of -20\deg C to 65\deg C. Because of this, it would not perform in more extreme environments and does not meet IEEE 495-2025 standards [SG5]. Therefore, the score it receives is a 3.
- The Raspberry Pi Pico 2 operates from -20\deg C to 85\deg C, which indicates that this device also does not meet the requirements of IEEE 495-2025 [SG6]. Therefore, this board will receive a 2. Since this board is also not designed for industrial applications compared to something like the Particle MCU, this would also reflect in a lower score
-  The Teensy 4.1 does not meet temperature requirements with a range of 0\degC to 70\degC, and a low end operating temperature right at freezing indicates that this system would not perform well in an outdoors industrial setting, meaning that it should receive a 1. [SG8].

#### Cost & Practicality
- The STM32U5 is priced in the $10-$20 range and is a compact size, making it a viable option in terms of practicality. Thus, it receives a 4.
- The MSP430 is also priced in the $10-$20 range and is a compact size, making it a viable option in terms of practicality. This microcontroller also receives a 4.
- The cost for the nRF9161 is around $30 and it relatively larger than options like the STM32U5 and the TI MSP430, but it compensates this price difference by creating a greater simplication of the subsystems with the cellular module.
- The ESP32-S3 is priced similarly to the STM32U5 and the TI MSP430, making it another low cost option, giving it a 4.
- The Particle BSoM B404 is the most expensive option at approximately $60, but there is the added benefit that this option is the easiest to set This fact alleviates the cost of this device since the higher upfront cost can be counteracted by the Particle Free cellular plan in addition to the MCU coming with a cellular module, simplifying the subsystems of this project [SG9]. Considering all of this information, the Particle microcontroller will still receive a 3 rating.
- The Raspberry Pi Pico 2 is priced similarly to the STM32U5 and the TI MSP430, giving it a 4 rating.
- The Teensy 4.1 has a low price point like the other inexpensive models, but it is also a relatively larger device, making it less practical to implement.

#### Computational Performance
- The STM32U5 has a 32-bit ARM Cortex-M33 core running up to 160 MHz, providing sufficient performance for this design, giving it a 4 rating [SG1].
- The TI MSP430 is built on a 16-bit architecture, making it to be an inferior option to all other options in this list, giving it a 2 rating [SG2].
- The Nordic nRF9161 runs on a 32-bit ARM cores clocked around 64 MHz,giving it a rating of 4 [SG3].
- The ESP32-S3 features dual-core Xtensa LX7 processor up to 250 MHz, also giving it a rating of 4 [SG4].
- Like the Nordic microcontroller, the Particle BSoM B404 is built around 32-bit ARM cores, also giving it a 4 [SG5].
- The Raspberry Pi Pico 2 utilizes dual ARM Cortex-M33 running at 150 MHz, also giving it a 4 [SG6].
- The Teensy 4.1 has the superior architecture out of the microcontrollers with a processor running at 600 MHz, giving it a rating of a 5 [SG10].

#### Ease of Cellular Communication Configuration
- The STM32U5 does not come with a cellular module, so the modem would have to be a separate implementation, giving it a 2 rating.
- The TI MCU This MCU does not come with a cellular module, so the modem would have to be a separate implementation, also giving it a 2 rating.
- The Nordic MCU comes with a cellular module, but development is done within the Zephyr RTOS ecosystem, which would require further learning [SG3]. For these reasons, the Nordic MCU receives a 4.
- The ESP32-S3 does not come with a cellular module, so the modem would have to be a separate implementation, giving this microcontroller a 2 rating. 
- The Particle BSoM comes with a cellular module, and an entire cloud ecosystem, meaning that this option serves as the simplest option for communicating using cellular [SG5]. Thus, this option receives a 5 in this category.
- The Pico 2 does not have cellular compatibility, giving it a 1 rating.
- This Teensy 4.1 does not have cellular compatibilty, giving it a 1 rating.

#### Decision Matrix

The decision below demonstrates all of the scores each of the options get. Each of the scores are multiplied by their weights, and the overall score is the sum of each of these multiplied scores. A laptop option is also included in this table for the sake of having a control option, but was not discussed in the previous portions of this section. In summary, the laptop would not be robust or power-efficient enough and would be an impractical option to choose.

| Criteria & Weight | STM32U5 | TI MSP430 | Nordic nRF9161 | Particle BSoM | ESP32-S3 | Raspberry Pi Pico 2 | Teensy 4.1 | Laptop (Control) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Power Efficiency (0.35) | 5 | 5 | 4 | 3 | 3 | 3 | 1 | 1 |
| Longevity & Robustness (0.30) | 5 | 4 | 4 | 3 | 3 | 2 | 1 | 1 |
| Cost & Practicality (0.15) | 4 | 4 | 3 | 3 | 4 | 4 | 3 | 1 |
| Compute Performance (0.10) | 4 | 2 | 4 | 4 | 4 | 4 | 5 | 5 |
| Ease of Cellular Communication Configuration (0.10) | 2 | 2 | 4 | 5 | 2 | 1 | 1 | 1 |
| **Final Score** | **4.45** | **3.95** | **3.85** | **3.30** | **3.15** | **2.75** | **1.70** | **1.40** |

Therefore, based on the parameters needed for the microcontroller, the STM32U5 microcontrollers would be the most suitable option for this team.
### 2. Evaluation of Potential Solution Architectures

Seven distinct power system architectures were hypothesized and evaluated against the project constraints:

#### Option 1: Standalone Photovoltaic (PV) Solar with Hybrid Battery/Supercapacitor

- **Architecture:** A monocrystalline solar panel charges a LiFePO₄ battery via an MPPT controller, buffered by a parallel supercapacitor bank.
- **Evaluation:** Offers infinite off-grid autonomy and high isolation, but introduces high hardware complexity, weather dependency, and physical constraints for rapid transportability.

**References:** [SS-9]–[SS-14], [SS-16]

#### Option 2: Dual-Input Hybrid Architecture (Field Battery + Bench DC Jack)

- **Architecture:** A custom power distribution board utilizing an active power multiplexer to auto-switch between a field battery and a bench wall adapter.
- **Evaluation:** Excellent for dual environment use, but increases PCB layout complexity and introduces a conductive path to grid ground during bench testing.

**References:** [SS-3]

#### Option 3: Custom Rechargeable 18650/21700 Li-ion Pack with Discrete BMS

- **Architecture:** A multi-cell raw lithium-ion pack paired with a custom BMS and buck-boost regulation stage.
- **Evaluation:** Highly compact and isolated, but requires significant development time for custom safety circuitry, thermal protection, and spot-welded packaging.

**References:** [SS-2]

#### Option 4: Regulated AC-to-DC Wall Adapter with Isolated SMPS

- **Architecture:** A commercial AC-to-DC wall plug connected directly to linear regulators to provide steady power.
- **Evaluation:** Ideal for baseline noise characterization in the lab, but completely lacks outdoor field portability.

#### Option 5: Primary Industrial Lithium-Thionyl Chloride (LiSOCl₂) Pack

- **Architecture:** A non-rechargeable primary battery pack paired with a Hybrid Layer Capacitor (HLC) for multi-year deployment.
- **Evaluation:** Unmatched operating temperature range (-55 °C to +85 °C) and zero self-discharge, but expensive, non-rechargeable, and not concise for capstone development.

**References:** [SS-15], [SS-17]

#### Option 6: Modified Bauer 20 V Power Tool Battery with Custom Dock

- **Architecture:** A gutted Harbor Freight charger shell serving as a mechanical dock for Bauer 20 V tool packs, feeding a custom step-down buck converter and Under-Voltage Lockout (UVLO) circuit.
- **Evaluation:** Mechanically robust and easy to hot-swap, but requires significant manual wiring, custom UVLO protection, and heavy LC filtering to suppress raw switching noise.

**References:** [SS-7], [SS-18], [SS-19]

#### Option 7: Off-the-Shelf USB Power Bank with Isolated DC-DC and Low-Noise LDO

- **Architecture:** A commercial USB-C power bank providing regulated 5 V DC, feeding a board-mount isolated DC-DC converter module (e.g., RECOM/Murata 2 kV isolation) and an ultra-low-noise post-regulation LDO (e.g., LT3042).
- **Evaluation:** Combines certified safety, prebuilt BMS/charging, complete ground loop isolation, and quiet analog power rails in a compact, portable format.

**References:** [SS-2], [SS-3], [SS-20], [SS-21], [SS-22]

### 3. Decision Matrix Analysis

To select the optimal architecture, a weighted decision matrix was constructed. Each criterion was assigned a weight from 1 (lowest) to 5 (highest) based on its impact on overall project success. Signal Quality and Isolation received the highest weighting (5) due to the extreme sensitivity of telluric measurements.

**Power Source Decision Matrix**

| Evaluation Criteria | Weight | Solar + Supercapacitor | Dual-Input Hybrid | Li-ion Pack + BMS | Wall Adapter | Primary LiSOCl₂ | Modified Bauer 20 V | USB Power Bank + Isolation and LDO |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Financial: Initial Cost | 3 | 2 | 3 | 4 | 5 | 2 | 4 | 5 |
| Mechanical: Durability and Ruggedness | 4 | 3 | 4 | 3 | 2 | 5 | 5 | 4 |
| Usability: System Compatibility | 3 | 3 | 5 | 4 | 3 | 3 | 4 | 5 |
| Lifecycle: Expected Lifespan / Cycle Life | 3 | 5 | 4 | 3 | 5 | 1 | 3 | 4 |
| Signal Quality: Electrical Noise / PSRR | 5 | 4 | 3 | 4 | 2 | 5 | 2 | 5 |
| Isolation: Ground Loop Isolation | 5 | 5 | 2 | 5 | 1 | 5 | 5 | 5 |
| Logistics: Field Autonomy and Runtime | 4 | 5 | 2 | 3 | 1 | 5 | 3 | 3 |
| Build Effort: Design and Integration Simplicity | 3 | 1 | 2 | 4 | 5 | 4 | 2 | 4 |
| Environment: Cold Weather / Temperature Range | 3 | 3 | 3 | 2 | 4 | 5 | 3 | 3 |
| **Total Weighted Score** |  | **119** | **100** | **120** | **93** | **135** | **115** | **141** |

### 4. Final Solution Selection and Justification

Based on the decision matrix analysis, **Option 7: Off-the-Shelf USB Power Bank with Isolated DC-DC and Low-Noise LDO Post-Regulation** achieved the highest weighted score (141 / 165) and is selected as the primary power architecture for the transportable telluric ground fault sensor.

**Technical Justification for Option 7:**

- **Signal Quality for Microvolt Sensing:** By placing an ultra-high-PSRR LDO (>75 dB at 100 kHz) downstream of the isolated DC-DC converter, high-frequency switching noise is below 10 μV RMS, ensuring power supply ripple does not contaminate the sensor's analog signal [SS-21], [SS-7].
- **Complete Galvanic Isolation:** The integrated RECOM/Murata isolated DC-DC module provides a certified 2 kV DC galvanic barrier between the USB input ground and the internal instrument analog ground, preventing GPR damage and eliminating earth ground loops [SS-21], [SS-8], [SS-3].
- **Development Efficiency:** Utilizing a certified commercial USB-C power bank eliminates the safety risks and development overhead associated with designing custom lithium-ion charging and BMS protection circuits, fulfilling NFPA 855 safety goals out of the box [SS-20], [SS-2].
- **Seamless Dual-Environment Operation:** The sensor unit can be plugged into a standard 5 V wall adapter or laptop port during bench testing and calibration, and switched to a portable 20,000 mAh USB power bank for field deployments without requiring active multiplexing hardware [SS-20].

## High-Level Solution
### Hardware Block Diagram
### Operational Flow Chart
## Atomic Subsystem Specifications

### Telecommunications Subsystem Specifications:

Seal Team 2 shall use the Nordic Semiconductor nRF9160 as a microcontroller + telecommunications unit to output the sensor data via Cellular LTE-M/NB-IoT connectivity to the customer. 

The telecommunication subsystem shall transmit fault-event notifications and measurement records from the FFT amplitudes at 60Hz, 180Hz, and 540 Hz. The subsystem shall transmit at a rate of at least 1 time per hour with an immediate priority transmission for active fault indication as determined by the microcontroller.

The initial prototype of the system shall use the nRF9160 development platform and its LTE-M Capability. The on-board iBasis eSIM shall be used to establish cellular connectivity in the testing process. If testing proves the effectiveness of the subsystem, further conectivity options shall be addressed. LTE-M shall be the primary technology for the initial implementation.

The communication module shall establish network connectivity, determine whether transmission succeeded, and report connection or transmission errors to the processor as per the nRF9160 protocol.

Since the MCU only transmits three amplitudes and 1 boolean indicator during reporting, payload bandwidth is not a primary design constraint. However, energy consumed during network access, transmission and failed transmission attempts is a primary design constraint.

### MCU Specifications 

The microcontroller of our Telluric Current Sensor shall receive an output signal from the data acquisition module (DAQ) as an input signal and convert the analog signal to a digital signal via an analog-to-digital converter, also known as an ADC. From there, the microcontroller shall process the information via a Fast Fourier Transform, which converts the time-domain data to frequency domain data, allowing analysis of the voltage magnitude at the frequencies of various harmonics from the input signal. From there, the information that is prepared shall be sent to the transmitter as a serial output.

The sampling rate shall be frequent enough to ensure no aliasing occurs on the highest frequency of interest. Since the harmonics of importance are the 60 Hz, 180 Hz, and 540 Hz harmonics, this means that the Nyquist Sampling Frequency must be greater than or equal to twice the highest frequency. This indicates that the minimum frequency must be 1080 Hz. If the common standard is sampling at a rates given by powers of 2, the lowest power of 2 to obtain sufficient sampling for this frequency would be 2048 Hz. Therefore, the ADC shall sample at a rate of 2048 Hz.

Since power consumption should be minimized, the MCU should communicate with the user as little as possible. There are two potential scenarios that the MCU should transmit information: the first is immediately when a fault is detected. To ensure this occurs, there should be continuous sampling of the readings of the DAQ module. Periodic sampling would increase the risk of missing short faults, so continuous sampling is the only realistic option. The other scenario would be to transmit information on a regular basis for the purpose of data collection. To minimize the power consumed, this may be performed at an hourly rate. The MCU shall be able to sample data during a sleep mode using Direct Memory Access (DMA) and peripherals to further the minimization of data usage.

The Fast Fourier Transform shall be used to analyze the harmonics of the power system, covering a range of 0 Hz to 600 Hz to ensure that the three harmonics of greatest concern (1st, 3rd, and 9th) are reported and sent to the transmitter.
## Atomic Subsystem Specifications: Power Management and Conditioning Subsystem (PMCS)

### 1. Subsystem Overview and Functions

The Power Management and Conditioning Subsystem (PMCS) provides the foundational energy intake, safety isolation, and low-noise regulation required to power the telluric ground fault sensor.

**Core Functions:**
- **Energy Intake and Mating:** Accepts unconditioned 5 V DC power through a standard USB Type-C or Type-A port from an off-the-shelf portable power bank [SS-20].
- **Galvanic Isolation:** Establishes a reinforced isolation barrier rated to 2 kV to eliminate ground loops and prevent high Ground Potential Rise (GPR) in substation environments from propagating into sensitive digital and analog instrumentation [SS-3],[SS-8],[SS-21].
- **Low-Noise Regulation and Filtering:** Converts the isolated input into independent, regulated digital and ultra-low-noise analog power rails using high-PSRR linear post-regulation to suppress switching ripple [SS-22],[SS-7].
- **System Voltage Monitoring:** Scales and buffers input supply voltages into an analog feedback channel for microcontroller health monitoring.

### 2. Subsystem Interfaces

| Interface ID | Connected Subsystem | Signal Type | Signal Direction | Protocol / Standard | Data / Power Description |
|---|---|---|---|---|---|
| IF-PMCS-01 | External USB Power Bank | Power / Supply | Input | USB 2.0 / USB-C PD Specification | Unregulated 5.0 V DC supply voltage ($V_{\text{BUS}}$). |
| IF-PMCS-02 | Analog Front-End (AFE) | Power | Output | Analog Power Rail | Galvanically isolated +3.3 V analog rail with ultra-low noise (<10 μV RMS ripple). |
| IF-PMCS-03 | Microcontroller & Telemetry Unit | Power | Output | Digital Power Rail | Non-isolated or isolated +3.3 V digital power rail supporting peak transient loads up to 500 mA. |
| IF-PMCS-04 | Microcontroller ADC | Analog Signal | Output | Direct Analog Sensing | Scaled battery voltage monitor signal (0.0 V to 3.3 V), proportional to input charge level. |

**References:** [SS-20]

### 3. Subsystem Operation and User Interface

#### 3.1 User Interface Expectations

The PMCS provides visual status indicators mounted externally on the primary enclosure:

- **Power Status LED (green):** Indicates normal operation when the regulated +3.3 V output rails are within nominal tolerance.
- **Low Battery Warning LED (amber):** Illuminates when the incoming supply voltage from the power bank drops below 4.5 V DC.
- **Master Power Switch:** An inline toggle switch between the external power bank connection and internal circuitry that completely interrupts power during transport or storage.

#### 3.2 Subsystem Requirements

- **SRS-PMCS-001:** The PMCS shall accept standard 5 V DC power inputs from a commercial USB power bank or compatible auxiliary supply interface [SS-21],[SS-3].
- **SRS-PMCS-002:** The PMCS shall incorporate a galvanic isolation barrier between the external power input ground and the internal instrument analog ground to mitigate ground loops and transient coupling.
- **SRS-PMCS-003:** The PMCS shall supply stable DC voltage rails suitable for powering both digital microcontrollers and analog signal conditioning circuitry under standard operating loads [SS-7].
- **SRS-PMCS-004:** The PMCS power conditioning circuitry shall utilize passive or active filtering to reduce switching ripple and high-frequency noise on the analog supply rail [SS-22].
- **SRS-PMCS-005:** The PMCS shall utilize linear post-regulation with a high Power Supply Rejection Ratio (PSRR) to isolate sensitive measurement components from upstream supply fluctuations.
- **SRS-PMCS-006:** The PMCS shall provide a monitoring interface or scaled voltage signal to allow the system microcontroller to track incoming supply health or battery status.
- **SRS-PMCS-007:** All component placement, internal wiring methods, and enclosure designs within the PMCS shall comply with National Electrical Code (NEC) requirements, specifically NFPA 70 Articles 250, 706, and 725 [SS-1].

## Ethical, Professional, and Standards Considerations
## Resources
### Budget

Telecommunication/MCU Subsystem Bill of Materials:

Nordic Semiconductor nRF9160: $179.80
Alternative SIM option (TBD) (Potential options: 1NCE: $14/10yrs, Hologram: $1/mo)

### Division of Labor

Provided below is Seal Team 2's division of labor for the 5 subsystems of the Telluric Current Sensor:

**Seth Strahan - Power Subsystem:** Due to Seth Strahan's experience working with power system safety at ORNL and his extensive power system related courework at Tennessee Technological University, Seal Team 2 unanimously voted for Seth Strahan to take the specifications for the power subsystem and generate a solution in the Power Subsystem Detailed Design Document.

**Alex Bailey - DAQ Subsystem:** Due to Alex Bailey's extensive research history in Electromagnetic Fields, and, in particular, telluric currents, Seal Team 2 unanimously voted for Alex Bailey to take the specifications for the DAQ Subsystem and generate a solution in the DAQ Subsystem Detailed Design Document.

**Mark Myers - Weatherization Subsystem:** Due to Mark Myers' experience in manufacturing processes at his previous job, Aladdin, Seal Team 2 unanimously voted for Mark Myers to take the specifications for the Weatherization Subsystem and generate a solution in the Weatherization Subsystem Detailed Design Document.

**Silas Garmon - MCU Subsystem:** Due to Silas Garmons' coursework in computer science and microcomputer systems at Tennessee Technological University, Seal Team 2 unanimously voted for Silas Garmon to take the specifications for the MCU subsystem and generate a solution in the MCU Subsystem Detailed Design Document.

**Christopher Wolk - Telecommunications Subsystem:** Due to Christopher Wolk's proficiency in Signals and Systems and Electromagnetic Fields, Seal Team 2 unaninmously voted for Christopher Wolk to take the specifications for the MCU subsystem and generate a solution in the Telecommunication Subsystem Detailed Design Document.

### Timeline

## References

[MW.1] Semtech Corporation, “LoRa Technology Overview.” [Online]. Available: https://www.semtech.com/lora. [Accessed: Oct. 23, 2026].
[MW.2] “TTN Coverage Heatmap.” [Online]. Available: https://ttnmapper.org/heatmap/. [Accessed: Oct. 23, 2026].
[MW.3] Espressif Systems, “ESP32-WROOM-32E and ESP32-WROOM-32UE Datasheet.” [Online]. Available: https://documentation.espressif.com/
esp32-wroom-32e_esp32-wroom-32ue_datasheet_en.html. [Accessed: Oct. 23, 2026].
[MW.4] M. Woolley, “Bluetooth Low Energy Primer.” Bluetooth SIG. [Online]. Available: https://www.bluetooth.com/bluetooth-le-primer/. [Accessed: Oct. 23, 2026].
[MW.5] Quectel Wireless Solutions Co., Ltd., “BG95 Series QuecOpen Hardware Design,” Ver. 1.3. [Online]. Available: https://images.quectel.com/python/sites/2/2023/05/Quectel_BG95_Series_QuecOpen_Hardware_Design_V1.3.pdf. [Accessed: Oct. 23, 2026].
[MW.6] M5Stack, “Unit CatM.” [Online]. Available: https://docs.m5stack.com/en/unit/Unit_CatM. [Accessed: Oct. 23, 2026].
[MW.7] Quectel Wireless Solutions Co., Ltd., “BG95 Series LPWA Specification,” Ver. 2.0, Nov. 2024. [Online]. Available: https://developer.quectel.com/en/wp-content/uploads/sites/2/2024/11/Quectel_BG95_Series_LPWA_Specification_V2.0.pdf. [Accessed: Oct. 23, 2026].
## Contributions

1. [SS-1] National Fire Protection Association, NFPA 70: National Electrical Code, Quincy, MA: NFPA, 2026.
2. [SS-2] National Fire Protection Association, NFPA 855: Standard for the Installation of Stationary Energy Storage Systems, Quincy, MA: NFPA, 2026.
3. [SS-3] IEEE Recommended Practice for Powering and Grounding Sensitive Electronic Equipment, IEEE Std 1100-2005, 2005.
4. [SS-4] IEEE Standard for Interconnection and Interoperability of Distributed Energy Resources with Associated Electric Power Systems Interfaces, IEEE Std 1547-2018, 2018.
5. [SS-5] IEEE Guide for Protective Relaying Applications to Power Transformers, IEEE Std C37.91-2008, 2008.
6. [SS-6] D. Linden and T. B. Reddy, Handbook of Batteries, 3rd ed. New York: McGraw-Hill, 2002.
7. [SS-7] A. Barrado, R. Vázquez, J. Pleite, and E. Olias, "Low-frequency ripple in DC-DC converters: Causes and mitigation in sensitive electronic loads," IEEE Transactions on Power Electronics, vol. 23, no. 4, pp. 1952–1959, Jul. 2008.
8. [SS-8] R. P. Deshpande, The Relay Protection of High Voltage Networks, 2nd ed. London: Institution of Engineering and Technology, 2020.
9. [SS-9] Renogy, "Renogy 10W 12V Monocrystalline Solar Panel (RNG-SOL10W) Datasheet and Pricing," Renogy [Online]. Available: https://www.renogy.com/ (accessed Oct. 20, 2026).
10. [SS-10] PowerFilm, "PowerFilm MP3-373 Foldable Solar Panel Specifications," PowerFilm Solar [Online]. Available: https://www.powerfilmsolar.com/ (accessed Oct. 20, 2026).
11. [SS-11] Eco-Worthy, "Eco-Worthy 25W 12V Monocrystalline Module Specifications and Pricing," Eco-Worthy [Online]. Available: https://www.eco-worthy.com/ (accessed Oct. 20, 2026).
12. [SS-12] Analog Devices, LT3652 Monolithic Solar Power Battery Charger Datasheet, Analog Devices, Norwood, MA, Rev. E, 2020.
13. [SS-13] Texas Instruments, BQ25713 Buck-Boost Charge Controller IC Datasheet, Texas Instruments, Dallas, TX, Rev. C, 2021.
14. [SS-14] K2 Energy, 3.2V 18650 Cylindrical LiFePO₄ Cell Technical Specifications, K2 Energy, Henderson, NV, 2023.
15. [SS-15] Saft Batteries, Saft LS 33600 Primary Lithium-Thionyl Chloride D-Cell Datasheet, Saft, Paris, France, 2022.
16. [SS-16] Eaton, Bussmann Series HV 2.7V 100F Cylindrical Supercapacitor Datasheet, Eaton Corporation, Dublin, Ireland, 2021.
17. [SS-17] Tadiran Batteries, Tadiran Hybrid Layer Capacitor (HLC-1520) Technical Specifications, Tadiran Batteries, Lake Success, NY, 2022.
18. [SS-18] Harbor Freight Tools, "Bauer 20V Cordless Battery Charger," Harbor Freight [Online]. Available: https://www.harborfreight.com/20v-cordless-battery-charger-56780.html (accessed Oct. 20, 2026).
19. [SS-19] Harbor Freight Tools, "Bauer 20V 3.0 Ah Lithium-Ion Slide Battery," Harbor Freight [Online]. Available: https://www.harborfreight.com/ (accessed Oct. 20, 2026).
20. [SS-20] Anker, "Anker Power Bank (20,000mAh, 22.5W, Built-In USB-C Cable) Product Specifications and Pricing," Anker Innovations [Online]. Available: https://www.anker.com/ (accessed Oct. 20, 2026).
21. [SS-21] RECOM Power, ROE-0505S 1W SIP4 Isolated DC-DC Converter Datasheet, RECOM Power, Gmunden, Austria, 2022.
22. [SS-22] Analog Devices, LT3042 20V, 200mA, Ultralow Noise, Ultrahigh PSRR RF Linear Regulator Datasheet, Analog Devices, Norwood, MA, Rev. C, 2023.

[SG11] "IEEE Standard for Testing Faulted Circuit Indicators," in IEEE Std 495-2025 (Revision of IEEE Std 495-2007) , vol., no., pp.1-26, 27 Feb. 2026, doi: 10.1109/IEEESTD.2026.11415435.
keywords: {IEEE Standards;Cables;Fault diagnosis;Circuit stability;Circuit testing;cable fault locator;fault indicators;faulted circuit indicator;FCI;IEEE 495 (TM)},

[SG1] STMicroelectronics, “STM32U575xx: Ultra-low-power Arm® Cortex®-M33 32-bit MCU + TrustZone® + FPU, 240 DMIPS, up to 2 MB Flash memory, 786 KB SRAM,” Datasheet DS13737, Rev. 10, 2024. [Online]. Available: https://www.st.com/resource/en/datasheet/stm32u575ai.pdf . [Accessed: Oct. 20, 2026].

[SG2] Texas Instruments, “MSP430FR698x(1), MSP430FR598x(1) Mixed-Signal Microcontrollers,” Datasheet SLAS789D, Jun. 2014, revised Aug. 2018. [Online]. Available: https://www.ti.com/lit/ds/symlink/msp430fr6989.pdf . [Accessed: Oct. 21, 2026].

[SG3] Nordic Semiconductor, “nRF9161 Product Specification,” [Online]. Available: https://docs.nordicsemi.com/r/bundle/ps_nrf9161/page/nrf9161_html5_keyfeatures.html . [Accessed: Oct. 21, 2026].

[SG4] Espressif Systems, “ESP32-S3 Series Datasheet,” ver. 2.2, Mar. 5, 2026. [Online]. Available: https://documentation.espressif.com/esp32_s3_datasheet_en.pdf . [Accessed: Oct. 21, 2026].

[SG5] Particle, “B404X datasheet,” Particle Documentation. [Online]. Available: https://docs.particle.io/reference/datasheets/b-series/b404x-datasheet/. [Accessed: Oct. 21, 2026].

[SG6] Raspberry Pi Ltd., “Raspberry Pi Pico 2 Datasheet,” [Online]. Available: https://pip-assets.raspberrypi.com/categories/1005-raspberry-pi-pico-2/documents/RP-008299-DS-2-pico-2-datasheet.pdf. [Accessed: Oct. 21, 2026].

[SG7] PJRC, “Teensy® 4.1 Development Board,” PJRC. [Online]. Available: https://www.pjrc.com/store/teensy41.html#power. [Accessed: Oct. 21, 2026].

[SG8] NetBurner, “SOMRT1061 and Teensy 4.1 Comparison,” NetBurner. [Online]. Available: https://www.netburner.com/somrt1061-and-teensy-4-1-comparison/. [Accessed: Oct. 22, 2026].
[SG9] Particle, “Cellular Data Guide,” Particle Documentation. [Online]. Available: https://docs.particle.io/getting-started/billing/cellular-data/. [Accessed: Oct. 8, 2026].

[SG10] P. Kumar, “Teensy 4.1: The 600 MHz Microcontroller That Feels Like a Tiny Computer,” PiEV Core, Aug. 17, 2026. [Online]. Available: https://pievcore.com/insights/teensy-41-the-600-mhz-microcontroller-that-feels-like-a-tiny. [Accessed: Oct. 22, 2026].
