# Seal Team 2 Project Proposal: Wireless Fault Current Indicator
## Introduction
Across the globe,  electrical power is supplied from source to recipient via power lines. These lines carry single and three phase power while often carrying telecommunication signals below on a separate section. If these power lines fail, it can result in loss of magnitude of power (brownout) or loss of power altogether (blackout). This can happen from a transformer blowing up, power lines grounding through trees, or power lines falling either into other lines or onto the ground. Any instance which results in unprompted behavior is called a fault. Currently, the main method in locating a fault is by mobilizing service workers and garnering information from customer calls \[1\]. This, as one can imagine, is quite costly both monetarily and temporally.

In light of this challenge, **Seal Team 2** shall develop an affordable, replicable prototype sensor which will relay information pertaining to the induced telluric currents. This information shall be transmitted wirelessly to the company supplying and managing the area's power. Such information will be primarily contained to fault detection for the scope of this prototype. This sensor will take information on the voltage and telluric current density surrounding the power pole, filter the data so that only the $60$, $180$, and $540$ Hz harmonics are considered, then process the data and signal a fault if necessary. This prototype will include electrodes, an MCU unit, a telcommunication module, and a local power unit. The entire system will be designed to operate effectively outdoors in all outdoor conditions at the site, as per the customers specifications (*See Specifications and Constraints).

### Scope

In this proposal, **Seal Team 2** will provide a comprehensive view of the problem and solution strategy for the design the Telluric Current Sensor. The Formulating the Problem section shall fence in the design aspects by clarifying who the stakeholders are, providing a background of relevant theory to the design, enumerating the specifications and constraints imposed on the design by the stakeholders and governing bodies, and finally declaring a fully formulated problem statement for the design of the Telluric Current Sensor. The document shall then provide a survey of existing solutions for current fault indication techniques. The document will then define and clarify **Seal Team 2's** measures of success by explaining the experiments and methodologies that will be used to meet the systems specifications and constraints. The resources section will propose an estimated budget for the system, provide a division of labor for the members of **Seal Team 2**, and provide a detailed timeline for the design of the sensor. The document will conclude by enumerating the specific and broader implications for the design of the Telluric Current Sensor.

## Formulating the Problem

### Background
#### Telluric Current Theory
Almost all current methods in distributing power involve transformers and power lines. The smallest of such power lines will hold two lines (a phase and a neutral) or four lines (3 phases and a neutral). Both examples are subsets of three phase power distribution lines which rely primarily on the balancing of all three phases. If power lines fall either by heavy winds, falling trees, or the pole itself falling, two major faults can occur: line-to-line faults or line-to-ground faults. Line-to-line (LL) faults cause an imbalance in the three phases by shorting two of them together. Line-to-ground (LG) faults cause an imbalance by grounding one of the phases. Other specific kinds of faults can be three line (LLL) faults two lines to ground (LLG) and others, but all of these fall in either category of LL or LG or both and will henceforth be referred to by their respective category.

First, consider the LL fault. This occurs when two or three phases have been faulted together. For ease of thought, a fault of lines A and B will be considered. Suppose a line A is faulted in a way which connects it to line B. This will result in the path: Source$\rightarrow$Transformer Winding A$\rightarrow$A$\rightarrow$B$\rightarrow$Transformer Winding B$\rightarrow$Source. Where the path through B and B's transformer winding is traveling in the reverse direction. This causes the line voltage $V_{AB}$ to be very small near the fault but remain very large at the transformer. Such a voltage difference, with the low impedance of wire, will result in immense current surges. Say lines A and B are components of a $480\,\text{V}$ 3-phase power system (US Standard) and an LL fault of lines A and B occurs. $V_{AB}$ nearest the transformer will be $\approx480\,\text{V}$ while near the fault almost $0$. If the combined impedance of the fault and power lines is $0.05\,\text{m}\Omega$, then the resulting current would be $\frac{480}{0.05}=9600\,\text{A}$. Such an amperage is far too high to be maintainable by a system which causes arcing, fires, mechanical torque, electromagnetic disruptions, etc.

Next, consider the LG fault. This occurs when one or more wires are faulted to ground. This results in the path: Source $\rightarrow$ A
$\rightarrow$ Fault $\rightarrow$ Ground $\rightarrow$ Source Neutral $\rightarrow$ Source. Notice that the path relies on the source's neutral node or other ground return scheme to create the loop. This means that if line A has an LG fault, lines B and C *will* be affected because their voltages are relative to the neutral line. Their phase to ground voltage can fluctuate due to the relevance to a now faulty ground. Current through B and C are not primarily affected, but line A's current takes a most intriguing path: through the earth.

Diverting briefly to discuss standard operation: all power lines that carry current inject a portion of it into the ground called grid-induced telluric currents (as opposed to naturally- or geomagnetic-induced telluric currents) \[1\]. For the remainder of this proposal, such currents will be referred to simply as telluric currents. These telluric currents are measurable and show the standard harmonics at 60, 180, and 540 Hz \[1\]. Because there is injected current which extends radially from the power pole, there is an electric field created with equipotential lines of concentric circles emanating from the pole. $$\vec{J}=\sigma\vec{E}:\vec{E}=-\nabla V$$ Research shows that this voltage can be measured by use of two electrodes and an amplifier \[1\]. Given the above relationship (where $\sigma$ is the conductivity of the medium) it's understood that the voltage gradient is proportional to the current density (point-form of Ohm's Law) which means that any voltage measured is proportional to current carried by the power lines.

Recall now that in both LL and LG faults there is a large surge of current on the order of at least hundreds of amps. This will cause the
telluric currents to spike, *especially in the case of LG faults*. LG faults, as a result, shall be the primary focus of this prototype but there may be possibility of uncovering a method to diagnose LL faults in tandem.

\"Grid-Induced Telluric Currents for Non-Contact Load Monitoring and Fault Detection\", by Abigail Farris \[1\], exhibits a prototype used momentarily to monitor the telluric currents produced by a substation. This document shows the use of two electrodes used to measure voltage (parallel to the radius of the voltage gradient) which was then amplified into a system and transformed to the frequency domain. The result of the Fast Fourier Transform (FFT) showed remarkable amplitudes in the 60, 180, and 540 Hz frequencies. Farris concluded in speculation that such monitoring may reveal evidence of ground faults and general load monitoring non-invasively.

Given that modern techniques are either invasive (through the use of component load monitoring), expensive (for a similar reason), or slow (by use of call-ins and field technicians searching for the fault), this method may provide a way to accurately pinpoint and relay information to whomever is heading the power distribution. For such a purpose, it would prove advantageous for this sensor to be able to transmit over long ranges and be self sufficient in power.

#### Telecommunication Subsystem
The following information provides a background of relevant telecommunication theory for the process of designing the Telluric Current Sensor. 

In general, a communication system can be described by the following general process: Let $x(t)$ be the raw physical signal from which information is being extracted. In the case of Seal Team 2's Ground Fault Sensor, this will be the telluric currents from the substations. $x(t)$ is then processed by a tranducer, which will convert the physical signal into a digital signal $x_2(t)$ that contains the information from $x(t)$. The information is then propagated through a medium, typically air, using some modulation technique, along a carrier wave of the form: $Acos(\omega t + \phi)$. As the modulated wave propagates across the medium, noise is introduced which attenuates and distorts the signal. The receiver takes this noise distorted signal and demodulates it to restore the information from the input signal.

##### Modulation

Modulation refers to the action of altering the properties of the carrier wave to efficiently carry the information ($x_2(t)$). Basic forms of modulation include AM (amplitude modulation) which varies the amplitude of the carrier signal according to $x_2(t)$ and FM (frequency modulation) which varies the frequency of the carrier signal due to $x_2(t)$. The type of modulation used in the system can minimize the effects of the noise through the medium. For digital modulation, the following techniques have been optimized for such minimization:

- ASK (Amplitude Shift Keying): Encodes data by changing the amplitude (strength) of a carrier wave while keeping its frequency constant.

- FSK (Frequency Shift Keying): Encodes data by switching between different carrier frequencies.

- PSK (Phase Shift Keying): Encodes data by changing the phase (timing position within a cycle) of a carrier wave.

- QPSK (Quadrature Phase Shift Keying): A type of PSK that uses four distinct phase states to encode 2 bits per symbol.

- QAM (Quadrature Amplitude Modulation): Encodes data by varying both the amplitude and phase of a carrier wave, allowing multiple bits per symbol.

- Chirp-Based Modulation: Encodes data using chirps—signals whose frequency continuously increases or decreases over time. It can offer robust communication in noisy environments and is used by technologies such as LoRa.

##### Relevant Metrics

When determining the communication system architecture to be used for the Telluric Current Sensor, the following parameters are worth considering.

-*Speed Metrics*
The following metrics are related to the speed of data transmission:
*Data rate:* is the speed for which data transfers from transmitter to receiver, measured in bits/s. *Symbol Rate / Baud Rate* is the number of symbols/characters transferred from transmitter to receiver. (Calculated as: $BaudRate = \frac{BitRate}{BitsPerSymbol}$). *Latency* is the time delay in milliseconds for data to travel from source to destination.

-*Accessibility Metrics*
The following metrics are related to a modems ability to transmit effectively:
-*Bandwidth*: Frequency range of the transmitted signal. For cellular transmission, and more generally for public networks, data must be transmitted along specified bands. Thus, a modem that can operate over a large bandwidth is preferrable. *Throughput*: The amount of real output that is succesfully tranferred to the receiver.

**Seal Team 2** shall use the above metrics and modulation techniques to determine the effectiveness of potential telecommunication hardware and connectivity options for the Telluric Current Sensor.

#### Power Subsystem
A field-ready telluric ground fault sensor needs a highly adaptable power architecture capable of operating in diverse environments ranging from remote, unassisted terrain to high-voltage substations. In either scenario, powering a sensitive micro board requires balancing continuous baseline consumption with high peak bursts, all while maintaining power quality to prevent electrical noise from interfering with the ground measurements. To address these design challenges, the power subsystem explores multiple flexible architectures ranging from off-grid renewable energy to direct power feeds, governed by strict electrical and safety standards [SS-1]-[SS-5].

##### Power Subsytem Relevant Metrics
- **SR-01 — Output Regulation:** The power distribution board shall provide regulated DC outputs of 3.3 V and 5 V with a voltage regulation tolerance of $\pm 2\%$ under all operating load steps.
- **SR-02 — Power Quality and Ripple:** The power conditioning circuitry shall limit high-frequency switching ripple to less than 10 mV peak-to-peak to prevent noise contamination of the analog acquisition front-end [SS-7].
- **SR-03 — Galvanic Isolation:** The power supply architecture shall maintain a reinforced isolation barrier rated for a minimum of 2 kV between external power inputs and internal instrument grounds [SS-3], [SS-21].
- **SR-04 — Autonomous Operation:** The primary power architecture shall sustain continuous, unattended operation for a minimum of 24 hours under nominal field load profiles.
- **SR-05 — Transient Response:** The regulation and post-regulation stages shall maintain voltage stability and limit transient sags to less than 5% during sudden microcontroller load steps.
- **SR-06 — Regulatory Compliance:** All power subsystem component placement, wiring methods, and energy storage enclosures shall comply with applicable NEC Articles 250, 706, and 725 requirements [SS-1] along with NFPA 855 safety guidelines [SS-2].

##### Equations

###### 1. Voltage Regulation and Drop Analysis 

$$
\%VR = \left(\frac{V_{\text{no-load}} - V_{\text{full-load}}}{V_{\text{full-load}}}\right) \times 100
$$

Where:
- $V_{\text{no-load}}$ is the output voltage at zero load current.
- $V_{\text{full-load}}$ is the output voltage under maximum active load.

###### 2. Supercapacitor Transient Energy and Voltage Sag Mitigation 

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

###### 3. Battery State of Charge and Autonomous Runtime 

$$
T_{\text{runtime}} = \frac{C_{\text{batt}} \cdot \text{DoD} \cdot \eta_{\text{sys}}}{I_{\text{avg}}}
$$

Where:
- $C_{\text{batt}}$ is battery capacity (Ah).
- $\text{DoD}$ is maximum depth of discharge (0.80 for LiFePO$_4$).
- $\eta_{\text{sys}}$ is power conversion efficiency (0.85).
- $I_{\text{avg}}$ is average continuous current (A).

###### 4. Switching Regulator Output Ripple Voltage

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

#### Stakeholders
In essence, the potential stakeholders for this project other than our customer, Dr. Charles Van Neste, would be groups that deal with managing fault currents, such as utility companies. In particular, a system that can detect faults in this manner would be beneficial for rural utility companies that have to send out fleets of people to search for a fault when it occurs. Since rural utility companies, also known as utility cooperatives, serve multiple counties, a telluric current sensor with fault monitoring would be beneficial since remote areas bring along greater difficulty to locating faults when they occur. Groups that would be invested in this sensor would want a sensor that can detect a fault current using non-invasive techniques, that can transmit the information of the system to a device owned by the stakeholders, and -if possible- having a triangulation system in place to locate the fault in a power system.

#### Specifications

The following section will enumerate and clarify the specifications provided by the customer, Dr. Van Neste, on behalf of all of the stakeholders (See Stakeholders subsection)

##### Telecommunications Subsystem Specifications
- **TCS-TS1** The customer, Dr. Charles Van Neste, specifies that the data must be transmitted wirelessly to a gateway accesible by any of the stakeholders that is at most 1 km away from the sensor.
- **TCS-TS2** The customer, Dr. Charles Van Neste, specifies that the power consumption of the transmitter must be minimized. 
- **TCS-TS3** The customer, Dr. Charles Van Neste, specifies that the entire communication module must be weatherized.
##### MCU Subsystem Specifications
- **TCS-MS1** The customer, Dr. Charles Van Neste, specifies that the Telluric Current Sensor may use the Particle Boron Microcontroller as the combined MCU and Telecommunication modules.
- **TCS-MS2** The customer, Dr. Charles Van Neste, specifies that the MCU must use a Fast Fourier Transform algorithm to extract the 60 Hz, 180 Hz, and 540 Hz frequency amplitudes from the sampled telluric current input.
- **TCS-MS3** The customer, Dr. Charles Van Neste, specifies that the MCU must detect abnormalities in the frequencies and determine if a fault has occured based on such data.
##### Weatherization Subsystem Specifications
- **TCS-WS1** The customer, Dr. Charles Van Neste, specifies that the entire system must be housed in a water proof and weather proof encasement.
- **TCS-WS2** The customer, Dr. Charles Van Neste, specifies that all components must be capable of operating under all of Cookeville, TN temperature ranges ($-30 - 41^{\circ}$ C). 
- **TCS-WS3** The customer, Dr. Charles Van Neste, specifies that the system must be able to operate under the extreme weather conditions in Cookeville: rain, snow, etc.
##### DAQ Subsystem Specifications
- **TCS-DS1** The customer, Dr. Charles Van Neste, specifies that the system must measure the voltage differences between two or more electrodes and input this data into the MCU via an ADC converter.
##### Power Subsystem Specifications
- **TCS-PS1** The customer, Dr. Charles Van Neste, specifies that the system must be powered locallly and efficiently and require minimal maintenance. (Battery replacements >1 month).
- **TCS-PS2** The customer, Dr. Charles Van Neste, specifies that the system must incorporate surge protection devices complying with IEEE Std C62.41 to withstand high-voltage transient overvoltages a lightning-induced surges.
- **TCS-PS3** The customer, Dr. Charles Van Neste, specifies that the system may be charged via the telluric currents from the substation through the use of a supercapacitor bank, thereby requiring no charging element.
##### PCB Specifications
- **TCS-PCB1** The customer, Dr. Charles Van Neste, specifies that each of the subsystems may be connected via a printed circuit board to make the commercialization of this project more effective.

##### Discussion
Upon negotiation with Dr. Charles Van Neste and extensive review of the TCS-MS1 specification, **Seal Team 2** decided to void TCS-MS1 in lieu of better options for microcontroller units that will be outlined in the Conceptual Design Document. Furthermore, due to the complexity of the supercapacitor bank mentioned in **TCS-PS3**, **Seal Team 2** negotiated with the customer to void this requirement. All other specifications will be considered for the design of the Telluric Current Sensor.

#### Constraints

##### Data Acquisition Subsystem Constraints

*Data acquisition electrode constraints:*
Since testing will be performed near high-tension lines, there are important safety considerations to be investigated. For one, there is a chance of pertubing underground utilties. To avoid this, OSHA 29 CFR 1926.651(b) requires that the utility owner is contacted before any excavating occurs, and Tennessee’s Underground Utility Damage Prevention Act (TCA 65-31-101) requires everyone who digs to contact Tennessee 811 at least three (3) working days’ notice before starting a project [SG1], [SG2]. The testing location should also be verified on what is permitted within the right-of-way, as per TVA’s Transmission Right of Way team [SG3]. To also prevent electric shock or similar injuries during installation, it is recommended by OSHA 1910.333(c)(6) to minimize the hazard through implementation of work practice such as handling techniques and insulation [SG4]. Beause of this, personal protective equipment and proper insulation should be diligently inspected, with these ratings given by 29 CFR 1926.97 and OSHA 29 CFR 1926.957, respectively [SG5], [SG6].

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

**Regulatory and Safety Codes:** All internal wiring, power distribution, and grounding must strictly comply with NEC (NFPA 70) requirements, specifically Article 250 (Grounding and Bonding), Article 690 (Solar PV Systems), Article 706 (Energy Storage Systems), and Article 725 (Low-Voltage Control Circuits) [SS-1].

**Thermal and Chemical Limitations:** Battery charging and discharging parameters must respect chemical limits (preventing lithium or thermal runaway under NFPA 855 guidelines) [SS-2].

 **Geographical and Site Restrictions:** Field deployment requires correct physical orientation and spacing of sensing probes relative to local telluric vectors to help with meaningful data collection.

###### Applicable Standards and Design Implementation

To ensure safe operation, signal integrity, and regulatory compliance across environments ranging from field sites to high-voltage substations, the power subsystem incorporates design criteria from the following key engineering codes and standards:

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

### Fully Formulated Problem Statement

**Seal Team 2** shall design a Telluric Current Sensor that obtains reliable current density information of the telluric currents emmitted from a substation while adhering to the **TCS-DS** specifications and DAQ subsystem constraints. The Sensor shall contain an MCU that captures the telluric current data digitally, extracts the 60 Hz, 180 Hz, and 540 Hz amplitudes from an FFT algorithm, determines the existence of a ground fault, and sends the data serially to a telecommunications module while adhering to the **TCS-MS** specifications and MCU subsystem constraints. The Sensor shall contain a telecommunications module to transmit the data across a distance of 1 km to a gateway accessible by all stakeholders while adhering to the **TCS-TS** specifications and telecommunication subsystem constraints. The Sensor shall be powered locally with minimal maintenance needs as per the **TCS-PS** specifications and power subsystem constraints. Finally, the sensor shall be housed in a waterproof and weatherproof encasement, and should be capable of operating in all outdoor conditions as per the **TCS-WS** specifications and Weatherization subsystem constraints.

## Survey of Existing Solutions
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

## Measures of Success
### Microcontroller Measures of Success
- Sufficient computing power to process the information received by sensor
- Withstand fault current events
- Minimal power consumption during detection
- Minimal power consumption during dormancy

### Power Subsystem Measures of Success
- **Autonomous Uptime:** Achieve continuous 24-hour operation under load.
- **Transient Voltage Stability:** Testing through oscillating the supercapacitor layer successfully limits voltage sags during spikes.
- **Power Quality and Noise Suppression:** Verify that the output of the regulators and LDOs meet low-noise requirements, ensuring no measurable power supply leaks into the sensor's bandwidth [SS-7],[SS-22].
- **Source Checking:** Demonstrate fault-free switching between the bench DC adapter and field power source without resetting the system.

## Resources

### Budget

#### Telecommunication Subsystem Budget

*LTE/NB-IoT Motem*: Cost: $8 - 150, depending on if the motem is connected to an MCU.

*Antenna*: largely dependent on communication module. $0 for already implemented antennas. $3-1000 for privately designed antenna to gateway systems. **Seal Team 2** shall choose an antenna <$50.

*SIM Provider* Cost: ~$0-$90/mo.

#### Microcontroller Budget

*MCU*: The microcontroller selected will range from $4 to $150 depending on the manufacturer selected from the previously listed options.

*Programming Software*: Any MCU obtained for this project should have free licensing whether through being free for all users or having free seats for student users.

#### Power Subsytem Budget

##### 1. Energy Generation: Monocrystalline Solar Panels

*Required for Option 1.*

- **Option A — Renogy 10 W 12 V Monocrystalline Solar Panel (RNG-SOL10W):** Compact, durable glass panel with an aluminum frame ($25.00).
- **Option B — PowerFilm MP3-373 Foldable Solar Panel:** Lightweight, rugged thin-film panel optimized for remote outdoor telemetry ($45.00).
- **Option C — Eco-Worthy 25 W 12 V Monocrystalline Module:** Higher power rating for reliable charging under overcast winter conditions ($32.00).

References: [SS-9], [SS-10], [SS-11]

##### 2. Charging and MPPT Controllers

- **Option A — Analog Devices LT3652 (Monolithic Solar Power Battery Charger):** Step-down battery charger IC tailored for MPPT operation ($12.00 eval/IC).
- **Option B — Texas Instruments BQ25713 (Buck-Boost Charge Controller IC):** High-performance IC supporting 1 to 4 cells with I²C control ($8.00).
- **Option C — Seeed Studio / DFRobot Solar Power Manager Micro:** Ready-to-use breakout module featuring MPPT management and protection ($15.00).

References: [SS-12], [SS-13]

##### 3. Energy Storage: LiFePO₄ and Primary LiSOCl₂

*Covers Options 1, 3, and 5.*

- **Option A — K2 Energy / Generic 3.2 V 18650 LiFePO₄ Cells:** Cylindrical cells with exceptional thermal stability ($6.00 per cell; Option 3).
- **Option B — Saft LS 33600 (Primary Lithium-Thionyl Chloride D-Cell):** Non-rechargeable, high-energy-density cell for multi-year utility deployments ($22.00; Option 5).
- **Option C — 12 V 6 Ah LiFePO₄ Mini-Pack with Integrated BMS:** Drop-in sealed pack for solar-hybrid setups ($35.00; Option 1).

References: [SS-14], [SS-15]

##### 4. Supercapacitors and Hybrid Layer Capacitors (HLC)

- **Option A — Eaton / Bussmann HV Series 2.7 V 100 F Cylindrical Supercapacitor:** High capacitance for smoothing peak loads ($8.00).
- **Option B — Kemet / Maxwell Technologies 3.0 V 50 F Prismatic Supercapacitor Cell:** Low ESR cell suited for fast transient response ($7.50).
- **Option C — Tadiran Hybrid Layer Capacitor (HLC-1520):** Designed to pair with LiSOCl₂ cells to assist with pulse demands ($12.00).

References: [SS-16], [SS-17]

##### 5. DC-DC Regulators and Power Management ICs

- **Option A — Texas Instruments TPS63060 (High-Efficiency Buck-Boost Converter):** Maintains stable output when input voltage fluctuates ($5.00).
- **Option B — Texas Instruments LMZ31704 (Power Module with Integrated Inductor):** Simplifies layout and reduces high-frequency switching noise ($9.00).
- **Option C — Analog Devices / Linear Technology LT3042 (Ultra-Low-Noise, High-PSRR LDO):** Placed downstream to reduce remaining ripple ($4.00).

References: [SS-22]

##### 6. Auxiliary Interface and Protection Hardware

*Supports Option 2.*

- **Option A — Texas Instruments TPS2121 (Active Power Multiplexer IC):** Auto-switching power multiplexer with priority selection ($6.00).
- **Option B — CUI Devices PJ-102AH (DC Barrel Jack Connector):** Standard indoor lab-bench power connection port ($1.50).
- **Option C — Littelfuse 313 Series / Bussmann Waterproof Inline Fuse Holders:** Overcurrent protection required by NEC Articles 240 and 706 ($3.00).

References: [SS-1]

##### 7. Cordless Drill Battery Setup: Harbor Freight Bauer 20 V System

*Supports Option 6.*

- **Option A — Harbor Freight Bauer 20 V Battery Pack (2.0 Ah to 5.0 Ah slide pack):** Store-bought rechargeable slide battery ($30.00–$60.00).
- **Option B — Modified Bauer 20 V Charger Shell and Dock:** Empty, hollowed-out charger case acting as a secure plastic holder ($15.00 donor charger).
- **Option C — Safety Cutoff and Voltage Regulator Board:** Circuit board stepping down high battery voltage while preventing over-discharge ($12.00).

References: [SS-18], [SS-19]

###### 8. Portable USB Power and Isolation Modules

*Supports Option 7.*

- **Option A — Commercial USB-C Power Bank (10,000 mAh–20,000 mAh):** Certified rechargeable lithium battery pack with built-in safety management ($20.00–$40.00).
- **Option B — RECOM / Murata 1 W Isolated DC-DC Converter Module:** Compact board-mount module providing galvanic isolation to eliminate ground loops ($8.00–$12.00).
- **Option C — Analog Devices / Linear Technology LT3042 Ultra-Low-Noise LDO Breakout Board:** High-PSRR linear regulator supplying a battery-quiet rail ($10.00–$15.00).

References: [SS-20], [SS-21], [SS-22]

### Personnel

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

### Timeline

![Gantt Chart](image-2.png)

## Specific Implications
Dr. Charles Van Neste is the customer for this project. The customer requires a system that can provide near real-time measurements of voltage differences in the ground and make those measurements accessible from a cellular device. Dr. Van Neste's research focuses heavily on energy generation and transmission, including wireless and quasi-wireless power transfer, high-frequency inverter design, electronic instrumentation, and electromagnetic interactions.

During this research, unexpected or cross-coupled voltage readings can make it difficult to determine whether a signal is produced by the experimental system or by another source in the surrounding environment. One possible source is naturally occurring or infrastructure-related telluric ground current, including current associated with nearby electrical substations and grounding systems. The purpose of this project is therefore to provide Dr. Van Neste with a dedicated telluric sensing system that can continuously monitor differences in ground potential. By comparing these measurements with experimental data, the customer will be better able to determine whether additional voltage is present in the ground and distinguish background ground-current effects from the signals produced by his wireless power-transfer experiments.


## Broader Implications, Ethics, and Responsibility as Engineers
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


## References
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

[SG1] Occupational Safety and Health Administration (OSHA), "Specific Excavation Requirements," 29 C.F.R. § 1926.651. [Online]. Available: https://www.osha.gov/laws-regs/regulations/standardnumber/1926/1926.651

[SG2] Tennessee 811, "Tennessee 811 Home Page." [Online]. Available: https://www.tenn811.com/

[SG3] Tennessee Valley Authority (TVA), "Who to Call for Help," Right-of-Way Maintenance. [Online]. Available: https://www.tva.com/energy/transmission/right-of-way-maintenance/who-to-call-for-help

[SG4] Occupational Safety and Health Administration (OSHA), "Selection and Use of Work Practices," 29 C.F.R. § 1910.333. [Online]. Available: https://www.osha.gov/laws-regs/regulations/standardnumber/1910/1910.333

[SG5] Occupational Safety and Health Administration (OSHA), "Electrical Protective Equipment," 29 C.F.R. § 1926.97. [Online]. Available: https://www.osha.gov/laws-regs/regulations/standardnumber/1926/1926.97

[SG6] Occupational Safety and Health Administration (OSHA), "Live-Line Tools," 29 C.F.R. § 1926.957. [Online]. Available: https://www.osha.gov/laws-regs/regulations/standardnumber/1926/1926.957

## Statement of Contributions
Alexander Bailey: Introduction, Background, Personnel, References

Silas Garmon: Editing of Introduction, Personnel, Specifications, Constraints, Existing Solutions, References, Measures of Success, Formatting, Budget

Mark Myers: Editing of Introduction, Personnel, Specifications, Constraints, References, Specific Implications

Seth Strahan: Budget, Personnel, Specifications, Constraints, References, Ethics, Broader Implications, Responsibility as Engineers

Christopher Wolk: Budget, Specifications, Constraints, References, Large Scale Editing

# Conceptual Design
## Introduction

Across the globe, electrical power is supplied from source to recipient via power lines. These lines carry single and three phase power while often carrying telecommunication signals below on a separate section. If these power lines fail, it can result in loss of magnitude of power (brownout) or loss of power altogether (blackout). This can happen from a transformer blowing up, power lines grounding through trees, or power lines falling either into other lines or onto the ground. Any instance which results in unprompted behavior is called a fault. Currently, the main method in locating a fault is by mobilizing service workers and garnering information from customer calls \[1\]. This, as one can imagine, is quite costly both monetarily and temporally.

In light of this challenge, **Seal Team 2** shall develop an affordable, replicable prototype sensor which will relay information pertaining to the induced telluric currents. This information shall be transmitted wirelessly to the company supplying and managing the area's power. Such information will be primarily contained to fault detection for the scope of this prototype. This sensor will take information on the voltage and telluric current density surrounding the power pole, filter the data so that only the $60$, $180$, and $540$ Hz harmonics are considered, then process the data and signal a fault if necessary. This prototype will include electrodes, an MCU unit, a telcommunication module, and a local power unit. The entire system will be designed to operate effectively outdoors in all outdoor conditions at the site, as per the customers specifications

### Scope

In this proposal, **Seal Team 2** will provide a comprehensive view of the problem and solution strategy for the design the Telluric Current Sensor. The Formulating the Problem section shall fence in the design aspects by clarifying who the stakeholders are, providing a background of relevant theory to the design, and enumerating the specifications and constraints imposed on the design by the stakeholders and governing bodies. The document shall then provide a survey of existing solutions for fault indication. The document will then define and clarify **Seal Team 2's** measures of success explaining the experiments and methodologies that will be used to meet the systems specifications and constraints. The resources section will propose an estimated budget for the system, provide a division of labor for the members of **Seal Team 2**, and provide a detailed timeline for the design of the sensor. The document will then explain the specific and broader implications for the design of the Telluric Current Sensor.

## Restating the Fully Formulated Problem

**Seal Team 2** shall design a Telluric Current Sensor that obtains reliable current density information of the telluric currents emmitted from a substation while adhering to the **TCS-DS** specifications and DAQ subsystem constraints. The Sensor shall contain an MCU that captures the telluric current data digitally, extracts the 60 Hz, 180 Hz, and 540 Hz amplitudes from an FFT algorithm, determines the existence of a ground fault, and sends the data serially to a telecommunications module while adhering to the **TCS-MS** specifications and MCU subsystem constraints. The Sensor shall contain a telecommunications module to transmit the data across a distance of 1 km to a gateway accessible by all stakeholders while adhering to the **TCS-TS** specifications and telecommunication subsystem constraints. The Sensor shall be powered locally with minimal maintenance needs as per the **TCS-PS** specifications and power subsystem constraints. Finally, the sensor shall be housed in a waterproof and weatherproof encasement, and should be capable of operating in all outdoor conditions as per the **TCS-WS** specifications and Weatherization subsystem constraints.

## Comparative Analysis of Potential Solutions

The following section provides Seal Team 2's comparative analysis of potential solutions for designing the telecommunication subsystem of the Telluric Current Sensor. This section will begin by determining and justifying the choice of cellular as the medium for which the data will be transmitted. This section will then compare 5 choices for a cellular hardware module as well as 5 choices for a connectivity/SIM module.

## Comparison of Electrodes


Firstly, the decision must be made on how the data will be collected.
Any method must measure the potential difference through the soil to
find the electric field. Because all of the ways to extract voltage from
the earth rely on implanted conductors, the only decision to be reached
is the shape and the material.

The shape, or burial configuration, is what determines the measurable
data points and the options considered were: delta, wye, collinear rods,
and parallel plates. The metrics for the decision matrix were: ease of
burial, data quality, and cost. Data quality is the sublime metric for
the shape because poor quality data is useless so it is weighted double
the others.

The delta configuration is formed by three electrodes in an equilateral
triangle; this would give us three voltage differences that form a loop
(helpful for Kirchoff's Equations). The wye configuration is the same as
the delta, but there is an electrode place in the center of the delta;
this would give us six voltage differences that form many closed loops.
The collinear rods would give us one voltage difference likewise for the
parallel plates. This shows that the data quality would be highest for
the wye configuration. for the parallel plates, there was uncertainty
whether or not the voltage difference may self-induce an electric field
across the plates, but it wasn't worth testing as it results in the same
data points as the rods. The ease of burial was determined by how many
implanted electrodes were needed (delta and wye being approximately
similar). The cost was related to the added expense of material and
circuity/processing power needed (which makes for the separation of
delta and wye). Overall, the wye configuration was shown to be superior
to the other options.

::: {#tab:placeholder}
                 Shape                 Ease of Burial (0.25)   Data Quality (0.5)   Cost (0.25)   Total
  ----------------------------------- ----------------------- -------------------- ------------- -------
           Delta ($\Delta$)                      4                     3                 4         3.5
                Wye (Y)                          4                     5                 3        4.25
   Collinear Rods ($\cdot\,\,\cdot$)             5                     2                 5         3.5
        Parallel Plates (\| \|)                  3                     1                 2        1.75

  : Decision matrix determining burial configuration of electrodes
:::

Next is to determine the material of the electrodes. Of course, it must
be conductive and withstand the environment well (because they cannot be
shielded so that they may conduct). Corrosion resistant metals include:
aluminum, copper, brass, bronze, stainless steel, galvanized steel and
titanium. Each of these develops a patina or surface oxide which changes
the conductivity but some corrode faster or to a greater detriment in
soil. Another factor to consider is the cost which will conceptually
incorporate lead time and supply capability. Lastly, resistivity will be
included to minimize the voltage drop across the electrode. Resistivity
won't be rated very highly because it's only a scalar shift. Cost and
patina will be weighed very high for obvious reasons but patina higher
still than cost. Overall, stainless stell is shown to be the superior
material. Patina [TT,CS,CO,CS2,FC]; Cost [FCE], Resitivity [AH]

::: {#tab:placeholder}
       Material       Patina (0.5)      Cost (0.4)        Resistivity (0.1)     Total
  ------------------ -------------- ------------------ ----------------------- -------
       Aluminum            1                4                     4              2.5
        Copper             2                3                     5              2.7
        Brass              2                3                     4              2.6
        Bronze             2                2                     4              2.2
   Stainless Steel         3                5                     3              3.8
   Galvanized Steel        1                5                     3              2.8
       Titanium            0                0                     3              0.3

  : Decision matrix determining material of electrodes
:::

### Comparison of Communication Media

For the purposes of Seal Team 2's telcommunication subsystem: the following 4 communication media: LoRaWAN, Wi-Fi, Bluetooth, and Cellular will be weighed under four categories: general power consumption, range, accessibility, cost to determine the optimal media for the Telluric Current Sensor.

-LoRaWan Module: LoRaWAN, or Long Range Wide Area Networking, is a wireless platform and network catered towards the transmission of IoT device data. According to SemTech, the developer of the physical LoRa module Hardware, LoRa is optimized to deliver data over a long range with minimal power consumption [1]. SemTech also boasts a wide range for their devices allowing for data transmission up to ~3km. As for accesibility, according to ttnmapper.org, a website that provides a heatmap of all public LoRaWAN Gateways on earth, Cookeville, Tenessee does not have any public LoRaWAN coverage (Refer to Figure MW.1) [2]. This would require Seal Team 2 to implement a private local gateway at the substation, which could be costly and require the use of the companies power supply, thereby nullifying the low power constraint imposed by the customer, Dr. Van Neste.

-WiFi Module: A Wi-Fi based design is another potential candidate for the design. Modules such as the ESP32-WROOM-32E house a Wi-Fi module and a microcontroller in one device. This could reduce component count and simplify the firmware development as compared to the other options. However, upon review of the datasheets of existing Wi-FI modules, the average power output during the active mode of Wi-Fi data transmission is much larger in comparison to the LoRaWan module (See Table 16: Current Consumption Depending on RF Modes from the ESP32-WROOM-32E datasheet [3]). Furthermore, locally housed Wi-Fi modules generally are designed to operate locally, and the >1km range specification from the customer exceeds these modules operating range.

-Bluetooth Module: In the typical use case, devices that use Bluetooth do not operate over a wide range. However, upon futher review, Seal Team 2 found Bluetooth Low Energy, which offers low-power communication over a longer range via forward-error-correction coding at the expense of data rate.[4]. Bluetooth does emphasize that the practical range of the transmission still depends heavily on the radio configuration, antenna characteristics, receiver sensiticity, and therefore establishing a >1 km link could require specialized hardware that could be expensive. 

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

## Comparative Analysis of Potential Solutions for Power Subsystem

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

Ease of Cellular Module Communication Configuration Set Up will be defined as the compatibility and relative ease of setting up the microcontroller with cellular communication. While having a separate modem is an option, this option would introduce complex RF PCB design that would significantly increase the difficulty of implementation of this system. This decision was made based upon coordination with the telecommunications subsystem. Because of this, this parameter will be weighed at 35%. An option receiving a score of a 1 would be an option that is incompatible with cellular communication at the start or would have a steep configuration process, while an option receiving a 5 is an option that has the most direct set up process.

Additionally, the microcontroller must be efficient with power consumption. Since sampling data periodically increases the chances of a fault occuring and not being detected, continuous sampling would need to be used. Because of this, power efficiency is paramount, with a need for low current usage and sleep modes as apart of the system. For this reason, the power efficiency is weighed at 25%. An option receiving a score of 1 would be a device that draws current in the upper milliamps during active modes and lacking sleep mode options. On the other end, a system receiving a 5 would be a system with a robust sleep mode and drawing current in the microamp to sub-microamp range.

Longevity and robustness will be defined as ability to meet the IEEE 495-2025 standard of operating temperature of devices used for fault current indication rated at -40\degC to 85\deg C, and also whether the market for the option is intended for industrial use as opposed to educational or commercial use [SG11]. This is category is important because to market this project, it needs to meet industry standards. For this reason, longevity and robustness is weighed at 20%. An option receiving a score of 5 is a product that exceeds the desired ratings and is tailored for industrial applications, while an option receiving a 1 would be intended for personal/commercial use and failing to meet IEEE 495-2025 standards on operating temperatures.

Cost and practicality will be defined as a combination of the practicality of the option as a part of a device used in field testing as well as overall cost of the microcontroller. This category is not as important as parameters such as power efficiency or longevity and robustness, so it is weighed at 15%. An option receiving a 5 is one that is low cost, highly portable, and compact in size, while an option receiving a 1 is an option that can either cost significantly more than the other options or has considerable size differences from the other options.

Computational power will be defined as the microcontroller's ability to compute efficiently for the purposes it is designed to complete. Since the MCU's biggest computational task will be an FFT and sending information, this category does not need to be weighed as highly as the others, meaning that it will only be weighed at 10%. An option receiving a 5 is an option that can perform significantly higher than what is expected of it, and an option receiving a 1 is an option that would not be viable for this project.

#### Ease of Cellular Communication Configuration
- The STM32U5 does not come with a cellular module, so the modem would have to be a separate implementation, giving it a 2 rating.
- The TI MCU This MCU does not come with a cellular module, so the modem would have to be a separate implementation, also giving it a 2 rating.
- The Nordic MCU comes with a cellular module, but development is done within the Zephyr RTOS ecosystem, which would require further learning [SG3]. For these reasons, the Nordic MCU receives a 4.
- The ESP32-S3 does not come with a cellular module, so the modem would have to be a separate implementation, giving this microcontroller a 2 rating. 
- The Particle BSoM comes with a cellular module, and an entire cloud ecosystem, meaning that this option serves as the simplest option for communicating using cellular [SG5]. Thus, this option receives a 5 in this category.
- The Pico 2 does not have cellular compatibility, giving it a 1 rating.
- This Teensy 4.1 does not have cellular compatibilty, giving it a 1 rating.

#### Power Efficiency
- The STM32U5 is apart of the ST's ultra-low-power line of microcontroller options. This component has a 19.5 μA/MHz Run mode and has options for a low-power background autonomous mode (LPBAM) which allows for peripherals to operate in Stop mode, which means the peripherals can operate without software running. With these capabilities, the STM32U5 serves as a strong contender in terms of power efficiency [SG1]. For this reason, the STM32U5 receives a 5 in this category.
- The TI MSP430 is a device that has a DMA controller that allows for movement of data without the CPU, meaning that data can move around the peripheral without exiting sleep mode. This device draws 0.35 µA when in Real-Time Clock (RTC) mode, and has an even lower current in shutdown mode of 0.02 µA [SG2]. For this reason, the TI MSP430 receives a 5 in this category.
- The Nordic nRF9161 is a device with adequate power efficiency, consuming under 3 mA during active modes and consumes in the microamp range during sleep mode [SG3]. Because of this, the nRF9161 receives a 4.
- The ESP32-S3 has sleep mode capabilities with sufficient power efficiency, but during active operations there is higher current drawing within the tens milliamp of range [SG4]. Therefore, a 3 is alloted in this option.
- The Particle BSoM B404 is similar to the ESP32-S3 in terms of power consumption. It also has low consumption during sleep modes, but higher active mode power consumptions in the tens of milliamps range, primarily because of the cellular module on the device [SG5]. Also similar to the ESP32-S3, this MCU will get a 3 in this category.
- The Raspberry Pi Pico 2 does not perform at the same level as the other MCUs listed, as its base low-power mode consumes more current than the others and draws milliamps during active operations [SG6]. For this reason, The Pico 2 will receive a 3. 
- The Teensy 4.1 draws 100 mA of current at a clock rate of 600 MHz, which indicates that the Teensy 4.1 performs poorly at power efficiency [SG7]. For this reason, the Teensy 4.1 will receive a 1 in this category.

#### Longevity & Robustness
- The STM32U5 also has an exemplary operating rating of -40\deg C to 85\deg C, with even some variants operating up to 125\deg C, proving itself to be a more than sufficient candidate in terms of surviving industrial conditions [SG1]. For this reason, this MCU will receive a 5 in this parameter.
- The TI MSP430 also meets the IEEE 495-2025 standards of environmental ratings, with the ability to operate at temperatures of -40\deg C to 85\deg C, making it reliable for this project [SG2]. Thus, the TI MCU will receive a 4.
- Like the TI MCU, the Nordic MCU also meets the IEEE 495-2025 standards of environmental ratings, which indicates this MCU as being sufficient for this category [SG3]. Thus, it is alloted a 4.
- While the ESP32-S3 does meet the temperature ratigns of IEEE 495-2025, the modules it is paired with may not depending on the product, and this MCU is not intended for industiral compared to other MCUs like the STM32U5 and the TI MSP430 [SG4]. Becayse of this, the MCU will be alloted a 3.
- The BSoM B404 has a recommended operating range of -20\deg C to 65\deg C. Because of this, it would not perform in more extreme environments and does not meet IEEE 495-2025 standards [SG5]. Therefore, the score it receives is a 3.
- The Raspberry Pi Pico 2 operates from -20\deg C to 85\deg C, which indicates that this device also does not meet the requirements of IEEE 495-2025 [SG6]. Therefore, this board will receive a 2. Since this board is also not designed for industrial applications compared to something like the Particle MCU, this would also reflect in a lower score of 2.
-  The Teensy 4.1 does not meet temperature requirements with a range of 0\degC to 70\degC, and a low end operating temperature right at freezing indicates that this system would not perform well in an outdoors industrial setting, meaning that it should receive a 1. [SG8].

#### Cost & Practicality
- The STM32U5 is priced in the $10-$20 range and is a compact size, making it a viable option in terms of practicality. Thus, it receives a 4.
- The MSP430 is also priced in the $10-$20 range and is a compact size, making it a viable option in terms of practicality. This microcontroller also receives a 4.
- The cost for the nRF9161 is around $30 and it relatively larger than options like the STM32U5 and the TI MSP430, but it compensates this price difference by creating a greater simplication of the subsystems with the cellular module. Therefore, ths nRF9161 receives a 3.
- The ESP32-S3 is priced similarly to the STM32U5 and the TI MSP430, making it another low cost option, giving it a 4.
- The Particle BSoM B404 is the most expensive option at approximately $60, but there is the added benefit that this option is the easiest to set This fact alleviates the cost of this device since the higher upfront cost can be counteracted by the Particle Free cellular plan in addition to the MCU coming with a cellular module, simplifying the subsystems of this project [SG9]. Considering all of this information, the Particle microcontroller will still receive a 3 rating.
- The Raspberry Pi Pico 2 is priced similarly to the STM32U5 and the TI MSP430, giving it a 4 rating.
- The Teensy 4.1 has a low price point like the other inexpensive models, but it is also a relatively larger device, making it less practical to implement. Therefore, the Teensy 4.1 earns a 3 rating.

#### Computational Performance
- The STM32U5 has a 32-bit ARM Cortex-M33 core running up to 160 MHz, providing sufficient performance for this design, giving it a 4 rating [SG1].
- The TI MSP430 is built on a 16-bit architecture, making it to be an inferior option to all other options in this list, giving it a 2 rating [SG2].
- The Nordic nRF9161 runs on a 32-bit ARM cores clocked around 64 MHz,giving it a rating of 4 [SG3].
- The ESP32-S3 features dual-core Xtensa LX7 processor up to 250 MHz, also giving it a rating of 4 [SG4].
- Like the Nordic microcontroller, the Particle BSoM B404 is built around 32-bit ARM cores, also giving it a 4 [SG5].
- The Raspberry Pi Pico 2 utilizes dual ARM Cortex-M33 running at 150 MHz, also giving it a 4 [SG6].
- The Teensy 4.1 has the superior architecture out of the microcontrollers with a processor running at 600 MHz, giving it a rating of a 5 [SG10].

#### Decision Matrix

The decision below demonstrates all of the scores each of the options get. Each of the scores are multiplied by their weights, and the overall score is the sum of each of these multiplied scores. A laptop option is also included in this table for the sake of having a control option, but was not discussed in the previous portions of this section. In summary, the laptop would not be robust or power-efficient enough and would be an impractical option to choose.

| Criteria & Weight | Nordic nRF9161 | STM32U5 | TI MSP430 | Particle BSoM | ESP32-S3 | Raspberry Pi Pico 2 | Teensy 4.1 | Laptop (Control) |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Cellular Setup Ease (0.35) | 4 | 2 | 2 | 5 | 2 | 1 | 1 | 1 |
| Power Efficiency (0.25) | 4 | 5 | 5 | 3 | 3 | 3 | 1 | 1 |
| Robustness / Longevity (0.20) | 4 | 5 | 4 | 3 | 3 | 2 | 1 | 1 |
| Cost & Practicality (0.15) | 3 | 4 | 4 | 3 | 4 | 4 | 3 | 1 |
| Compute Performance (0.10) | 4 | 4 | 2 | 4 | 4 | 4 | 5 | 5 |
| **Final Score** | **3.85** | **3.70** | **3.45** | **3.55** | **3.15** | **2.65** | **1.70** | **1.00** |

Therefore, based on the parameters needed for the microcontroller, the Nordic nRF9161 microcontrollers would be the most suitable option for this team.

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

### Comparative Analysis of Weatherization Solutions

The weatherproofing subsystem is responsible for protecting the internal electrical components of the telluric ground sensing system from environmental exposure during outdoor operation. The subsystem must protect the electronics from rainfall, soil moisture, humidity, dust, standing water, and other environmental hazards that could cause corrosion, electrical shorts, inaccurate measurements, or equipment failure.
The comparative analysis placed the greatest emphasis on water resistance and overall outdoor protection because of the expected operating conditions of the system. Each enclosure was evaluated based on whether it had been tested for water exposure, including submersion where applicable, and whether its design provided secure protection against environmental intrusion and tampering. Size was the next most heavily weighted factor because the enclosure must be large enough to house all required components while still remaining practical for Seal Team 2 to deploy in the field. Cost was also given significant weight because the project budget is limited and ultimately became one of the deciding factors in the final enclosure selection. The final criterion, given the lowest weight, was ease of modification. This included how easily the enclosure could be adapted for the project through the addition of cable glands, mounting hardware, and other required penetrations. Ease of modification was also tied closely to cost and risk, since improper modifications could damage the enclosure or reduce its resistance to water intrusion.

NEMA Model: ZQ-161407
The ZQ-161407 is a gray polycarbonate electrical enclosure designed for harsh outdoor environments. The enclosure is rated NEMA 6P and IP68 and has been specifically tested for protection against water intrusion in submersible conditions. It is available with either a standard screw-close cover or a screw-close cover with tamper-resistant screws [MM4]. A rubber gasket outlines the cover and works with a thick, raised rib to help maintain a watertight seal. The enclosure also includes 32 internal mounting bosses, providing flexibility for component installation and placement within the system. Its external dimensions are approximately 16 in. × 14 in. × 8.2 in. DIN rails, cable glands, and other internal mounting hardware would need to be purchased separately.

Due to the ZQ-161407 being rated NEMA 6P, it received full points for water protection because the enclosure is designed for harsh wet environments and submersion conditions[MM1]. Additionally, the enclosure provides strong outdoor environmental protection, which makes it well suited for the expected deployment conditions of the telluric sensing system. The size of the enclosure is slightly smaller than ideal, but it should still provide enough space to house the required components without making field installation unnecessarily difficult.

Cost is what most clearly set the ZQ-161407 apart from the other options. The enclosure is cost-effective enough that, if Seal Team 2 incorrectly modifies the enclosure and compromises its waterproofing capability, the project budget would still allow for the purchase of a replacement unit. In addition, the parts required for modification, such as cable glands and mounting hardware, are relatively inexpensive and readily available. These factors reduce both the financial risk and the overall difficulty associated with modifying the enclosure for the final system.

#### Analysis of Each Option and Scoring Scale Parameters
Scoring Scale: 5 = Excellent, 4 = Good, 3 = Acceptable, 2 = Poor, 1 = Very Poor.

NEMA Model:
H181610HCF-6P-P10
The H181610HCF-6P-P10 is part of the Integra Premium Series of enclosures and is typically used for outdoor wall-mounted applications. Its construction is very similar to the ZQ-161407, as it is also made from gray polycarbonate and is designed for harsh outdoor environments.[MM2] The enclosure uses a hinged cover secured by four screws, and it also includes a clear cover that allows Seal Team 2 to visually inspect the internal components without opening the enclosure. The enclosure dimensions are approximately 18 in. × 16 in. × 10 in. DIN rails, mounting panels, and other internal hardware are not included with the base enclosure and would need to be purchased separately.

Like the ZQ-161407, the H181610HCF-6P-P10 is rated NEMA 6P, which gives it a high level of protection against water intrusion and outdoor environmental exposure. Because of this, it received full points for both water protection and weather resistance. The enclosure also offers the option of tamper-resistant hardware, which provides an additional level of protection against unwanted access. Where the H181610HCF-6P-P10 lost points was in size and cost. Although it remains within the project size requirements, its larger dimensions would make installation and deployment more difficult than the ZQ-161407 and would provide more internal space than Seal Team 2 is expected to need.

The most significant disadvantage of the H181610HCF-6P-P10 is its cost. The enclosure itself is approximately $223, and this price does not include additional items such as DIN rails, mounting panels, or water-resistant cable glands. These added costs would place greater strain on Seal Team 2’s project budget. In addition, if the enclosure were incorrectly modified and its waterproofing capabilities were compromised, purchasing a replacement would be significantly more difficult than replacing the lower-cost ZQ-161407. Although the H181610HCF-6P-P10 is easier and safer to modify than the custom PVC housing, its higher enclosure and accessory costs make it less practical for the project.

Personal PVC Housing
The Personal PVC Housing is a concept based on research conducted by Delft University of Technology and the University of Münster. The referenced design used a porous-pot electrode constructed with a thick-walled plastic tube and a custom modular enclosure system. Threaded plastic components were used to house the wiring and other internal electrode components, allowing the research team to disassemble, clean, and maintain the system with relative ease. The researchers also incorporated several rubber O-rings to protect important openings from water intrusion while still allowing the sensing portion of the electrode to maintain electrical contact with the surrounding environment.[MM3]

The Personal PVC Housing is the most unique and customizable option considered; however, it would also introduce the greatest amount of risk for Seal Team 2. Its resistance to water intrusion would depend almost entirely on the quality of the team’s fabrication and installation, with no manufacturer certification verifying the completed enclosure. Unlike the H181610HCF-6P-P10 and ZQ-161407, which carry established NEMA ratings for water and outdoor protection, the Personal PVC Housing would not have a recognized environmental rating or verified performance under submerged conditions. For this reason, it received significantly lower scores in the water and outdoor protection categories.

The Personal PVC Housing did receive full points for size because Seal Team 2 could design the enclosure around the exact dimensions required by the system. It also received one of the highest scores for cost because PVC pipe and fittings are relatively inexpensive, readily available, and could potentially include reusable or recycled materials. However, fabrication and modification would require substantially more effort from the team. Seal Team 2 would be responsible for creating all openings, threaded sections, cable penetrations, and sealing interfaces while ensuring that each modification remained watertight. Because no prefabricated enclosure or manufacturer-tested sealing system would be available, the custom PVC option would require extensive fabrication and water

#### Decision Matrix
| Available Products | Water Protection | Temperature / Outdoor Protection | Size | Cost | Ease / Risk of Modification | Total Evaluation |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| ZQ-161407 | 5.00 | 5.00 | 3.75 | 3.33 | 5.00 | 22.08 |
| H181610HCF-6P-P10 | 5.00 | 5.00 | 3.33 | 1.67 | 3.00 | 18.00 |
| Personal PVC Housing | 2.50 | 2.50 | 5.00 | 5.00 | 1.00 | 16.00 |

Based on the comparative analysis, the ZQ-161407 was selected as the enclosure that gives Seal Team 2 the strongest overall path toward a successful and protective electrical housing. It provides the best balance between environmental protection, reliability, size, and, most importantly, cost. The H181610HCF-6P-P10 offers many of the same protective qualities as the ZQ-161407 and even includes a clear cover that would allow the team to visually inspect the internal electrical system. However, its significantly higher cost creates too great of a barrier for the project.

The Personal PVC Housing is the most innovative and customizable option considered, but it does not provide enough verified protection against outdoor conditions, making it the weakest overall choice. Its performance would depend heavily on Seal Team 2’s fabrication and sealing methods. Although the Personal PVC Housing may still be useful during the testing and prototyping process, the ZQ-161407 provides the best overall combination of protection, reliability, and cost for Seal Team 2.

## High-Level Solution

For the Telluric Current Sensor, a wye-configuration of stainless steel rods is used to collect voltage readings from the electric field in the soil. This information is then converted into digital information using an ADC. From there, the data is converted into magnitudes and frequencies using a Fast Fourier Transform algorithm using a Nordic nRF9161 microcontroller that will send serial information to the cellular module on the device that will then transmit the information to the desired parties. This sensor will be weatherized using a ZQ-161407 enclosure and will be powered by an off-the-shelf USB power bank with isolated DC-DC and low-noise LDO post-regulation.

### Hardware Block Diagram

![Hardware Block Diagram](image-5.png)

### Operational Flow Chart

![Operational Flow Chart](image-4.png)

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

Ethics: 
- Honest Performance claims: Report detection limits, sensitivity, and failure modes accuratley, not just the best case results. Overstating reliablity on a safety device is a ethical issue. 
- Fail-Safes: If the power is lost, the battery degrades, or a component fails. The sensor should indicate its own failure rather than producing flase or weak reading.
- Codes of Ethics: Cite IEEE Code of Ethics, and the NSPE Code of Ethics.
- Data and Privacy: Logs that transmit across shared grid data should state who owns it and how its protected.
- Intellectual Property and Attributions: Give credit to prior work and individuals whom helped fairly. 
- Data Integrity vs. Alarm Fatigue: Sensor data must accurately decouple true earth fault conditions. Misclassifying a telluric wave as noise risks missing genuine dangerous ground fault currents.
- Affordability and Safety: Reducing cost should not come at the expense of essential safety and reliability. Balance affordability with dependable measurments. 

*Data acquisition electrode constraints:*
Since testing will be performed near high-tension lines, there are important safety considerations to be investigated. For one, there is a chance of pertubing underground utilties. To avoid this, OSHA 29 CFR 1926.651(b) requires that the utility owner is contacted before any excavating occurs, and Tennessee’s Underground Utility Damage Prevention Act (TCA 65-31-101) requires everyone who digs to contact Tennessee 811 at least three (3) working days’ notice before starting a project [SG1], [SG2]. The testing location should also be verified on what is permitted within the right-of-way, as per TVA’s Transmission Right of Way team [SG3]. To also prevent electric shock or similar injuries during installation, it is recommended by OSHA 1910.333(c)(6) to minimize the hazard through implementation of work practice such as handling techniques and insulation [SG4]. Beause of this, personal protective equipment and proper insulation should be diligently inspected, with these ratings given by 29 CFR 1926.97 and OSHA 29 CFR 1926.957, respectively [SG5], [SG6].

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

**Regulatory and Safety Codes:** All internal wiring, power distribution, and grounding must strictly comply with NEC (NFPA 70) requirements, specifically Article 250 (Grounding and Bonding), Article 690 (Solar PV Systems), Article 706 (Energy Storage Systems), and Article 725 (Low-Voltage Control Circuits) [SS-1].

**Thermal and Chemical Limitations:** Battery charging and discharging parameters must respect chemical limits (preventing lithium or thermal runaway under NFPA 855 guidelines) [SS-2].

 **Geographical and Site Restrictions:** Field deployment requires correct physical orientation and spacing of sensing probes relative to local telluric vectors to help with meaningful data collection.

###### Applicable Standards and Design Implementation

To ensure safe operation, signal integrity, and regulatory compliance across environments ranging from field sites to high-voltage substations, the power subsystem incorporates design criteria from the following key engineering codes and standards:

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

## Resources
### Budget

Upon conversing with the rest of Seal Team 2, it has been established that weatherproofing will be granted  a max of  40% of the budget. ($400 of a 1000$ budget) This is deemed to be fair due to the fact that the weatherproofing needs external materials to create a safe, functional, and reliable weatherproofing system. 
Breakdown of Max Spending Potential:
•	$200 of the NEMA rated box
•	$100 of Installation products and appliances
•	$100 PVC equipment and miscellaneous spending on modifications

Telecommunication/MCU Subsystem Bill of Materials:

Nordic Semiconductor nRF9160: $179.80
Alternative SIM option (TBD) (Potential options: 1NCE: $14/10yrs, Hologram: $1/mo)

Power System Bill of Materials:

-Analog Devices / Linear Technology LT3042 (Ultra-Low-Noise, High-PSRR LDO):** Placed downstream to reduce remaining ripple ($4.00).
-Littelfuse 313 Series / Bussmann Waterproof Inline Fuse Holders:** Overcurrent protection required by NEC Articles 240 and 706 ($3.00).
-Anker, "Anker Power Bank (20,000mAh, 22.5W, Built-In USB-C Cable) ($60.00).
-RECOM / Murata 1 W Isolated DC-DC Converter Module:** Compact board-mount module providing galvanic isolation to eliminate ground loops ($8.00–$12.00).


### Division of Labor
Provided below is Seal Team 2's division of labor for the 5 subsystems of the Telluric Current Sensor:

**Seth Strahan - Power Subsystem:** Due to Seth Strahan's experience working with power system safety at ORNL and his extensive power system related courework at Tennessee Technological University, Seal Team 2 unanimously voted for Seth Strahan to take the specifications for the power subsystem and generate a solution in the Power Subsystem Detailed Design Document.

**Alex Bailey - DAQ Subsystem:** Due to Alex Bailey's extensive research history in Electromagnetic Fields, and, in particular, telluric currents, Seal Team 2 unanimously voted for Alex Bailey to take the specifications for the DAQ Subsystem and generate a solution in the DAQ Subsystem Detailed Design Document.

**Mark Myers - Weatherization Subsystem:** Due to Mark Myers' experience in manufacturing processes at his previous job, Aladdin Temp-Rite, Seal Team 2 unanimously voted for Mark Myers to take the specifications for the Weatherization Subsystem and generate a solution in the Weatherization Subsystem Detailed Design Document.

**Silas Garmon - MCU Subsystem:** Due to Silas Garmons' coursework in computer science and microcomputer systems at Tennessee Technological University, Seal Team 2 unanimously voted for Silas Garmon to take the specifications for the MCU subsystem and generate a solution in the MCU Subsystem Detailed Design Document.

**Christopher Wolk - Telecommunications Subsystem:** Due to Christopher Wolk's proficiency in Signals and Systems and Electromagnetic Fields, Seal Team 2 unaninmously voted for Christopher Wolk to take the specifications for the MCU subsystem and generate a solution in the Telecommunication Subsystem Detailed Design Document.

### Timeline

![Gantt Chart Again](image-3.png)

## References

[MW.1] Semtech Corporation, “LoRa Technology Overview.” [Online]. Available: https://www.semtech.com/lora. [Accessed: Oct. 23, 2026].

[MW.2] “TTN Coverage Heatmap.” [Online]. Available: https://ttnmapper.org/heatmap/. [Accessed: Oct. 23, 2026].


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
22. [SS-22] Analog Devices, LT3042 20V, 200mA, Ultralow Noise, Ultrahigh PSRR RF Linear Regulator Datasheet, Analog Devices, Norwood, MA, Rev. C, 2023.[MW.3] Espressif Systems, “ESP32-WROOM-32E and ESP32-WROOM-32UE Datasheet.” [Online]. Available: https://documentation.espressif.com/

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
esp32-wroom-32e_esp32-wroom-32ue_dateasheet_en.html. [Accessed: Oct. 23, 2026].

CS. [SS-13:] 
Texas Instruments, BQ25713 Buck-Boost Charge Controller IC Datasheet, Texas Instruments, Dallas, TX, Rev. C, 2021.

[Christopher Wolk:G6] Raspberry Pi Ltd., “Raspberry Pi Pico 2 Datasheet,” [Online]. Available: https://pip-assets.raspberrypi.com/categories/1005-raspberry-pi-pico-2/documents/RP-008299-DS-2-pico-2-datasheet.pdf. [Accessed: Oct. 21, 2026].

[SG7] PJRC, “Teensy® 4.1 Development Board,” PJRC. [Online]. Available: https://www.pjrc.com/store/teensy41.html#power. [Accessed: Oct. 21, 2026].

[SG8] NetBurner, “SOMRT1061 and Teensy 4.1 Comparison,” NetBurner. [Online]. Available: https://www.netburner.com/somrt1061-and-teensy-4-1-comparison/. [Accessed: Oct. 22, 2026].
[SG9] Particle, “Cellular Data Guide,” Particle Documentation. [Online]. Available: https://docs.particle.io/getting-started/billing/cellular-data/. [Accessed: Oct. 8, 2026].

[SG10] P. Kumar, “Teensy 4.1: The 600 MHz Microcontroller That Feels Like a Tiny Computer,” PiEV Core, Aug. 17, 2026. [Online]. Available: https://pievcore.com/insights/teensy-41-the-600-mhz-microcontroller-that-feels-like-a-tiny. [Accessed: Oct. 22, 2026].

\[TT\] T. Tamai, "Growth of oxide films on the surface of Cu contact and
its effect on the contact resistance property," *Electronics and
Communications in Japan (Part II: Electronics)*, vol. 72, no. 7, pp.
87--93, 1989, doi: [10.1002/ecjb.4420720710
](https://doi.org/10.1002/ecjb.4420720710).

\[CS\] "The chemistry of copper patination," *Corrosion Science*, 1998.
\[Online\]. Available:
[https://www.sciencedirect.com/science/article/pii/S0010938X98000936
](https://www.sciencedirect.com/science/article/pii/S0010938X98000936).

\[CO\] C.-O. A. Olsson and D. Landolt, "Passive films on stainless
steels---Chemistry, structure and growth," *Electrochimica Acta*, vol.
48, no. 9, pp. 1093--1104, 2003, doi: [10.1016/S0013-4686(02)00841-1
](https://doi.org/10.1016/S0013-4686(02)00841-1).

\[CS2\] "Effect of alloying elements on the contact resistance and the
passivation behaviour of stainless steels," *Corrosion Science*, vol.
44, no. 4, pp. 635--655, 2002, doi: [10.1016/S0010-938X(01)00107-X
](https://doi.org/10.1016/S0010-938X(01)00107-X).

\[FC\] F. H. Constable, "Electrical conductivity of copper oxide films
showing interference colours," *Nature*, vol. 136, p. 517, Sep. 1935,
doi: [10.1038/136517a0
](https://doi.org/10.1038/136517a0).

\[FCE\] "Metal Price Chart --- \$/lb Today," *FabCostEstimator*. Accessed:
Oct. 20, 2026. \[Online\]. Available:
[https://fabcostestimator.com/metal-prices
](https://fabcostestimator.com/metal-prices)

\[AH\] A. Helmenstine, "Table of Electrical Resistivity and
Conductivity," *Science Notes*. Accessed: Oct. 20, 2026. \[Online\].
Available:
[https://sciencenotes.org/table-of-electrical-resistivity-and-conductivity/
](https://sciencenotes.org/table-of-electrical-resistivity-and-conductivity/)


[def]: <Operational Flow Chart.png> 

## Contributions

Alexander Bailey: Comparative Analysis of Solutions, Operational Flow Chart, Atomic Subsystem Specifications, References
Silas Garmon: Comparative Analysis of Solutions, Atomic Subsystem Specifications, High Level Solution, Ethical Considerations, References
Mark Myers: Comparative Analysis of Solutions, Atomic Subsystems Specifications, Budget, References
Seth Strahan: Comparative Analysis of Solutions, Atomic Subsystem Specifications, Budget, References
Chrisopher Wolk: Fully Formulated Problem, Hardware Block Diagram, Atomic Subsystem Specifications, Comparative Analysis of Solutions, Budget, Division of Labor, References
