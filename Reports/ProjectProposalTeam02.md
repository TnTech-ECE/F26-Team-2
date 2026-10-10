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
A field ready telluric ground fault sensor needs a highly adaptable power architecture capable of operating in diverse enviroments ranging from remote, unassited terrain to high voltage substations. In either scenario, powering a sensitive microboard requires balancing a continous baseline consumption with high peak burst, all while maintaing power quality to prevent electrical noise from interferring with the ground measurments. To address these design challanges the power subsystem explores  multiple flexible architectures inlcuding off grid renewable energy to direcct power feed, governed by strict electrical and safety standards.

Equations

1. Voltage Regulation and Drop Analysis. To quantify how well your power distribution network maintains output stability under varying load steps (supporting your $\pm 2\%$ regulation requirement in SR-01):$$\%VR = \left( \frac{V_{\text{no-load}} - V_{\text{full-load}}}{V_{\text{full-load}}} \right) \times 100$$Where:$V_{\text{no-load}}$ = Output voltage of the regulator/distribution bus at zero load current.$V_{\text{full-load}}$ = Output voltage under maximum active load (e.g., active processing and telemetry burst).

2. Supercapacitor Transient Energy & Voltage Sag Mitigation. To mathematically back up how your supercapacitor bank prevents voltage sags during high-current cellular/radio transmission bursts (supporting SR-05):$$E = \frac{1}{2} C (V_{\text{initial}}^2 - V_{\text{min}}^2)$$Rearranged to solve for the allowable voltage drop ($\Delta V$) during a pulse current ($I_{\text{pulse}}$) lasting for time duration ($\Delta t$):$$\Delta V = \frac{I_{\text{pulse}} \cdot \Delta t}{C} + (I_{\text{pulse}} \cdot R_{\text{ESR}})$$Where:$C$ = Capacitance of the supercapacitor bank (Farads).$I_{\text{pulse}}$ = Peak current drawn during a telemetry/cellular burst (Amps).$\Delta t$ = Duration of the transmission pulse (Seconds).$R_{\text{ESR}}$ = Equivalent Series Resistance of the supercapacitor cell ($\Omega$).

3. Battery State-of-Charge & Autonomous Runtime EstimationTo justify your 24-hour (or multi-day) autonomous operation requirement (SR-04) based on your battery capacity and baseline current draw:$$T_{\text{runtime}} = \frac{C_{\text{batt}} \cdot DoD \cdot \eta_{\text{sys}}}{I_{\text{avg}}}$$Where:$C_{\text{batt}}$ = Battery capacity rating (Amp-hours, Ah).$DoD$ = Maximum allowable Depth of Discharge (e.g., $0.80$ for LiFePO₄ to preserve cycle life).$\eta_{\text{sys}}$ = Total power conversion efficiency of the DC-DC regulators (expressed as a decimal, e.g., $0.85$).$I_{\text{avg}}$ = Average continuous system current draw (Amps).

4. Switching Regulator Output Ripple VoltageTo mathematically demonstrate compliance with your strict power quality requirement of $<10\text{ mV}$ peak-to-peak ripple (SR-02):$$V_{\text{ripple}} = \frac{V_{\text{out}} \cdot (1 - D)}{8 \cdot L \cdot C_{\text{out}} \cdot f_{\text{sw}}^2}$$Where:$V_{\text{out}}$ = Regulated output DC voltage (e.g., 3.3V or 5V).$D$ = Duty cycle of the buck/buck-boost converter ($V_{\text{out}} / V_{\text{in}}$).$L$ = Inductance value of the power stage inductor (Henries).$C_{\text{out}}$ = Output filter capacitance (Farads).$f_{\text{sw}}$ = Switching frequency of the regulator (Hz).

#### Physical Subsystem
Many environmental issues must be considered when designing any project, but with this sensor, the surrounding conditions need to be studied and analyzed even more carefully. Since this system is going to be deployed in Cookeville, Tennessee, it will be exposed to rainfall, soil moisture, drastic temperature changes, humidity, and potentially other hazardous weather conditions. Any of these conditions could affect the performance of the system and, in some cases, make it inoperable.

The telluric ground sensor must maintain electrical contact with the soil while also protecting its electrical connections from the environmental hazards stated above. This creates a difficult design challenge because the sensor must interact directly with the ground even though the soil can contain different levels of moisture. If moisture reaches unintended electrical connections or sensitive components, it could lead to inaccurate readings, corrosion, or equipment failure. One possible solution would be to seal the entire system; however, this would be too aggressive because it would reduce the sensor’s ability to measure the electrical potential of the ground.

Additionally, environmental temperature must also be accounted for because changes in temperature can negatively impact  the electrical potential and resistance of an electrode. Therefore, the system must be designed to remain protected from outdoor conditions while still allowing the sensor to properly interact with the surrounding soil.

### Specifications and Constraints

#### Specifications

##### Telecommunication Subsystems Specifications:

*Maximized data rate:* Minimum data rate threshold shall be determined on a future date.

*Maximized Signal-To-Noise ratio:* According to the CASRAI document, *Limit of Detection vs Limit of Quantitation: How to Calculate LOD and LOQ**, the generally accepted minimum signal-to-noise ratio for quantitation of data is 10:1 \[2\]. This shall be *Seal Team 2's* choice for the minimum.

*Minimized transmission power consumption (including resting power consumption):* Maximum power consumption threshold shall be determined by the constraints of the power subsystem.

*Minimized Latency:* Maximum latency threshold shall be ~1s.

##### Microcontroller Specifications
When a line-to-line fault or a line-to-ground fault is detected by the sensor, a microcontroller shall wirelessly transmit this information to a device. The size of the data transmitted is dependent on the final level of the complexity of the system. At its base form, it is required to indicate when a fault is detected in a transmission line. Ideally, a triage system would be implemented for locating where a fault occurs in the system. The microcontroller shall be able to communicate with the sensors to interpret the data and determine when a fault occurs. If it is determined to convert the analog data to digital data, this may be done with using a Fast Fourier Transform algorithm on the microcontroller, which would increase the need for computational power on the microcontroller. The microcontroller shall also be optimized to consume the minimum amount of power needed to detect a fault. The amount of power consumption for the microcontroller is correlated with the computational demand on the device, so the amount of power that will be consumed with be dependent on the data processing. For conservation of power, a sleep mode may be established to reduce the power consumption of the device.

##### Power Subsystem Specifications
(Autonomous Operation): The primary off-grid power architecture (Solar/LiFePO₄/Supercapacitor or LiSOCl2LiSOCl2) shall sustain continuous, unattended operation for a minimum of 24 hours under nominal field load profiles[3].

(Regulatory Compliance): All power subsystem component placement, wiring methods, and energy storage enclosures shall comply with applicable NEC Articles 250, 690, 706, and 725 requirements.

(Surge & Transient Immunity): The power input stage shall incorporate surge protection devices complying with IEEE Std C62.41 to withstand high-voltage transient overvoltages and lightning-induced surges.

(Harmonic Control): The power conversion and switching circuitry shall limit current and voltage harmonic distortion to maintain compliance with IEEE Std 519, preventing conducted noise from contaminating sensitive telluric frequency bands.

(Grounding & Earth Potential): The system grounding and bonding architecture shall comply with IEEE Std 142 and support earth impedance verification per IEEE Std 81 to mitigate Ground Potential Rise (GPR) hazards in substation environments.

(Environmental & Thermal Operating Limits): The power and energy storage subsystems shall maintain safe operating parameters across an ambient temperature range of $-40^\circ\text{C}$ to $+65^\circ\text{C}$, incorporating safeguards against cold-weather charging and thermal runaway per NFPA 855.

(Portability): The complete power management and storage assembly shall be compact and lightweight enough to allow for single-person transport and deployment in remote terrain.

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

**Microcontroller Subsystem Constraints**
    
Since the microcontroller will be directly wired to the data acquisition unit, it would be regulated under Supbpart B of Title 47 of the FCC. Under Title 47 of the FCC, the microcontroller would be considered a Class A digital device, which is a device used for industrial and commercial use. Since it would also emits radio frequencies as a byproduct of the function of its clock mechanism, the microcontroller would be considered an unintentional radiator. Because of this, it is regulated as an unintentional radiator, it requires authorization such as the Supplier's Declaration of Conformity to ensure it abides the FCC before it can be marketed \[7\].

Effective December 23, 2027, there will be a procurement ban on products using semiconductors sourced from blacklisted entities due to Section 5949 of the James M. Inhofe National Defense Authorization Act for Fiscal Year 2023 \[8\]. This could be a negligible constraint since it would only affect the market for U.S. federal government.

There are additional regulations pertaining to ensuring cybersecurity of the components being connected to the power grid. The North American Electric Reliability Corporation, also known as NERC, requires that security procedures are implemented for the reliable operation and security of the Bulk Electrical System. Under Section B 1.2.5, it requires that any software used for the MCU is verified to comply with strict cyber security standards to ensure power grid safety \[9\].

Since the microcontroller is apart of a larger fault detection apparatus, it is required by the IEEE Guide for Testing Faulted Circuit Indicators (495-2025) in Section 4 to having an operating condition of -40&degC to 85&degC \[10\].

IEEE 1613 pertains to the testing and environmental requirements of communication devices with electric power apparatus applications. for  this system deals with fault detection, the microcontroller will experience voltage surges. To increase the practicality of the device, meeting Class 1 standards in IEEE 1613 allows the device to experience temporary error as long as the device has a self-recovery system \[9\].

There is also IEEE 1613.1 that is related to transmission and distribution applications. In this document, it defines Zone B as facility with equipment outside of an enclosed area. For our purposes, the MCU would need to be prepared for Zone B use, meaning that the environmental standards due to being outside of a boundary or fenced are \[11\].

IEC-61010-1 pertains to category ratings for safety requirements of equipment used for measurement, among other uses. Since the MCU will be used for monitoring power systems, it would need to be a CAT IV rating, which requires transient protection level up to 12 kilovolts \[12\]. Because of this requirement, protection of the microcontroller will be paramount.

**Power System Constraints**

*Voltage Regulation:* The power distribution and regualtion stages must maintain a tight DC voltage tolerances with minimal ripple to prevent high frequency noise[13].

*Transient and Surge Handling:* The system must absorb rapid load steps and isolate the system from external voltage spikes or lightning surges via proper supercapacitor  buffering[14].

*Electrical and Isolation:* The analog and processing unit must maintain galvanic isolation to prevent high Ground Potential Rise and faults from damaging downstream  circuitry or creating ground loops \[15\], \[16\].

*Power Budget:* Desinged for remote, unattended deployment the systems power must be minimized to ensure the hybrid storage can sustain operation[3].

*Financial:* The total Bill of Materials needs to reamin with limits while using reliable components. 

*Legal Constraints*

- Regulatory and Safety Codes: All internal wiring, power distribution, and grounding must strictly comply with NEC(NFPA 70) requirments specifically Article 250 (Grounding and Bonding), Article 690(solar PV Systems), Article 706(Energy Storage Systems), and Article 725 (Low-Voltage Control Circuits).
- Thermal and Chemical Limitations: Battery charging and discharging parameters must respect chemical limits (preventing lithium or thermal runaway under NFPA 855 guidelines)[17].
- Geographical and Site Restrictions: Field deployment requires correct physical oreintation and spacing of sensing probes realtive to local telluric vectors ro help with meaningful data collection.

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
To provide max adaptablity for different sites the system design outlines multiple unique power configurations regarding tradeoffs of cost, weight, longevity, and transient handling.

1) Standalone Photovoltaic Solar with Battery and Supercapacitor Bank
    - Architecture: A rugged monocrystalline solar panel charges a hybrid storage system through an MPPT charge controller. The storage combines battery for  off-grid energy storage with a parallel supercapacitor bank that absorbs high-frequency power transients, load steps, and voltage spikes.
    - Pros:
        - Long-term energy storage and capacity
        - Handles a wide range of climate conditions
        - Complete galvanic isolation from substation and grid potentials, preventing ground loops
        - Supercapacitor protects the battery from high peak currents and cold-weather voltage sags
    - Cons:
        - Requires physical space for panel mounting and adequate sun exposure
        - Weather dependent
        - Hardware complexity due to the multi-stage hybrid system

2) Dual-Input Hybrid Power Architecture (Field Source + Bench DC Jack)
    - Architecture: A power distribution board that accepts an external field source and an indoor backup adapter. An auto-switching power multiplexer selects between them.
    - Pros:
        - Helps between a transportable field sensor and one that is easy to test on a lab bench
        - Switches automatically to wall power indoors and to the field battery outdoors
        - Same hardware for development, calibration, and field use
    - Cons:
        - Higher circuit complexity and component count on the custom board
        - Requires careful PCB layout and protection to switch sources safely
        - Wall input adds a conductive grid connection during bench use

3) Rechargeable 18650/21700 Li-ion Pack with Integrated BMS
    - Architecture: A multi-cell 18650 or 21700 lithium-ion pack with a dedicated battery management system (BMS) and a low-quiescent-current buck-boost regulator that provides a clean 5V or 3.3V rail, followed by a low-noise LDO for the analog section.
    - Pros:
        - Smaller self supply that fits inside a small enclosure
        - Low cost and reliable for temporary deployments
        - Galvanically isolated from the grid, with no external footprint
    - Cons:
        - Finite energy capacity, requiring manual recharging or pack swaps
        - No long-term autonomous generation unless paired with solar
        - Cannot be charged safely below about 0 °C

4) Regulated AC-to-DC Wall Adapter with Isolated SMPS
    - Architecture: An industrial-grade wall adapter plugged into 120VAC, with high input to output galvanic isolation and reinforced insulation, followed by a low noise LDO.
    - Pros:
        - Primary laboratory and bench-testing supply for testing
        - Clean, uninterrupted power during development
        - Baseline noise characterization before field testing
    - Cons:
        - Wall outlet, with zero field portability
        - Introduces a conductive connection to the utility grid during bench testing

5) Primary Lithium Chloride Industrial Battry Pack 
    - Architecture: A non-recharable, high energy promary lithium battery pack paried with a hybrid capacitor to hangle high-current transmission spikes. Desinged with an ultra low self discharge rate so it can be "installed and forgot about."
    - Pros: 
        - capable of powering low-power sensor nodes for multiple years without maintenance or recharging
        - wide operating temperature range (-55 to +85 celcius), making it for outdoor or winter substation environments.
        - Low annual slef discharge rate 
    - Cons: 
        - Non rechargeable once depleted the pack must be replaced.
        - Requries careful circuit design to prevent inital voltage sags during transmission bursts.

#### Survey of Existing Power Solutions
- Standard Power Banks: Inexpensive and portable, but prone to low load shutoffs when sensor sleeps, lacks galvanic isolation.
- Unregulated Solar to Battery: Common in remote monitoring, but standard lead of lithium setups suffer from extreme cold and lack capactivity buffering for spikes.
- Tapped Substation Control Power: Highly reliable due to utility backups, but directly exposes electronics to high voltage GPR.

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
- Autonomous Uptime: Achieve continous 24 hour operation under a load
- Transient Voltage Stability: Testing through oscillating the supercapacitor layer sucessfully limits voltage sags during spikes.
- Power Quality and Noise Suppression: Verify that the output of the regulators and LDOs meet low noise requirments, ensuring no measurable power supply leaks into the sensors band.
- Source Checking: Demonstrate fault free switching between bench DC adapter and field power source without resetting system.
Mark:
- The system shall be designed to withstand and operate during outdoor temperatures of 0 to 44  degrees Celsius
- The system shall include an apparatus that attempts to mitigate  the effects of outdoor weather on electronics


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

Bill of Materials (BOM) & Component 

Options

1. Energy Generation: Solar Panels (Monocrystalline)Required for Option 1 (Standalone Solar PV).

Option A: Renogy 10W 12V Monocrystalline Solar Panel (RNG-SOL10W) — Compact, durable glass panel with an aluminum frame, ideal for mounting directly on a small NEMA enclosure lid ($25.00).

Option B: PowerFilm MP3-373 Foldable Solar Panel — Lightweight, rugged thin-film panel optimized for remote outdoor telemetry and easy deployment ($45.00).

Option C: Eco-Worthy 25W 12V Monocrystalline Module — Higher power rating to ensure full battery recovery and reliable charging even under overcast winter conditions ($32.00).

2. Charging & MPPT ControllersManages power transfer and optimizes energy harvest from the solar source.

Option A: Analog Devices LT3652 (Monolithic Solar Power Battery Charger) — Excellent step-down battery charger IC specifically tailored for MPPT operation with solar inputs for custom PCB integration ($12.00 eval/IC).

Option B: Texas Instruments BQ25713 (Buck-Boost Charge Controller IC) — High-performance IC supporting 1 to 4 cells with I2C control, ideal for advanced multi-chemistry management ($8.00).

Option C: Seeed Studio / DFRobot Solar Power Manager Micro — Ready-to-use breakout module featuring MPPT management and battery protection if you want to bypass surface-mount soldering for initial testing ($15.00).

3. Energy Storage Options (LiFePO₄ & Primary $\text{LiSOCl}_2$)Covers Options 1, 3, and 5 for secondary or primary storage.

Option A: K2 Energy / Generic 3.2V 18650 LiFePO₄ Cells — Cylindrical cells with exceptional thermal stability and long cycle life for self-contained packs ($6.00 / cell, Option 3).

Option B: Saft LS 33600 (Primary Lithium-Thionyl Chloride D-Cell) — Non-rechargeable, high-energy-density cell designed for multi-year "install-and-forget" utility deployments ($22.00, Option 5).

Option C: 12V 6Ah LiFePO₄ Mini-Pack with Integrated BMS — Drop-in sealed pack suitable for the solar-hybrid standalone setup ($35.00, Option 1).

4. Supercapacitors & Hybrid Layer Capacitors (HLC)Absorbs high-frequency transients, cold-weather sags, and telemetry transmission spikes.

Option A: Eaton / Bussmann HV Series (2.7V 100F Cylindrical Supercapacitor) — High capacitance for smoothing peak current loads and handling low-frequency energy buffering ($8.00).

Option B: Kemet / Maxwell Technologies (3.0V 50F Prismatic Supercapacitor Cell) — Low equivalent series resistance (ESR) cell suited for fast transient response and tight space constraints ($7.50).

Option C: Tadiran Hybrid Layer Capacitor (HLC-1520) — Specifically designed to pair with $\text{LiSOCl}_2$ cells to assist with high-current pulse demands during cellular/radio transmissions ($12.00).

5. DC-DC Regulators & Power Management ICsProvides clean 3.3V and 5V rails to the sensor electronics.

Option A: Texas Instruments TPS63060 (High-Efficiency Buck-Boost Converter) — Excellent for maintaining a stable output voltage when input voltage fluctuates above and below the target rail ($5.00).

Option B: Texas Instruments LMZ31704 (Power Module with Integrated Inductor) — Simplifies layout and reduces high-frequency switching noise for sensitive analog circuitry ($9.00).

Option C: Analog Devices / Linear Tech LT3042 (Ultra-Low-Noise, High-PSRR LDO) — Placed downstream of the switching regulator to strip away remaining ripple before power reaches the analog front-end ($4.00).

6. Auxiliary Interface & Protection HardwareSupports Option 2 (Dual-Input Multiplexing) and safety compliance.

Option A: Texas Instruments TPS2121 (Active Power Multiplexer IC) — Seamless auto-switching power multiplexer with priority selection between bench and field inputs ($6.00).

Option B: CUI Devices PJ-102AH (DC Barrel Jack Connector) — Standard indoor lab bench power connection port ($1.50).

Option C: Littelfuse 313 Series / Bussmann Waterproof Inline Fuse Holders — Essential overcurrent protection required by NEC Article 240 and 706 for battery and solar inputs ($3.00).

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
