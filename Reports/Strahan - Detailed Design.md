# Detailed Design - Power Management and Conditioning Subsystem (PMCS)
---
The Power Management and Conditioning Subsystem (PMCS) is responsible for supplying stable, uninterrupted electrical energy to all operational components and peripherals of the Telluric Current Ground Fault Indicator (TC-GFI). Its primary function is to accept, isolate, condition, and distribute the electrical power required by the microcontroller, telecommunication module, data acquisition system (DAQ), and active analog front-end components. This subsystem ensures that each internal load receives power at the correct voltage level while maintaining exceptional electrical isolation and minimal noise to protect sensitive microvolt-level earth potential measurements.

At a high level, the PMCS accepts unconditioned power from an external 5 V DC source, such as the Voltaic Systems V75 "Always-On" rechargeable power bank, with auxiliary off-grid charging provided by the plug-in 6 W monocrystalline solar panel. Once energy is delivered to the internal power conditioning network, it is routed through a certified board-mount isolated DC-DC converter (RECOM/Murata 1 W SIP4) that establishes a reinforced 2 kV galvanic isolation barrier. This barrier is critical to eliminating earth ground loops and protecting downstream instrumentation from the high Ground Potential Rise (GPR) and transient surges experienced when operating in high-voltage substation environments.

Following isolation, the power network converts and conditions energy into independent regulated digital and ultra-low-noise analog power rails using high-PSRR linear post-regulators (Analog Devices LT3042). These rails power all necessary computational and sensing components, ensuring stable voltage conditions and suppressing high-frequency switching ripple below 10 µV RMS, even during heavy active LTE transmission load steps.

Rather than relying on fixed line power or direct high-voltage line attachment like conventional fault indicators, this architecture supports standalone, portable operation for non-contact telluric monitoring. Out in the field, the "Always-On" pack avoids low-current sleep shutoffs while pass-through solar charging helps sustain longer deployments; when testing, a standard USB wall adapter or host port can power the system.

Overall, the PMCS is the foundation that supports the accuracy, safety, and field reliability of the TC-GFI. By combining galvanic isolation with clean, regulated power rails, it allows the sensing front-end, ADCs, and wireless telemetry to work together reliably for ground fault detection. 

# Specifications and Constraints (MOST OF THIS NEEDS TO BE REFRENCED TO THE MIDTERM DOC)

---

The Power Management and Conditioning Subsystem (PMCS) is controlled by specifications and constraints that support safe, low noise, and dependable operation of the TC-GFI across filed and laboratory conditions. These requirements come from physical and instrumentation noise limits, substation grounding and surge hazards, battery characteristics, subsystem prerequisites, regulatory standards, project budget, and customer constraints. 

**1. Power Subsystem Specifications (TCS-PS) and Rationale:**
- TCS-PS1 (Local Power & Minimal Maintenance): The system shall be powered locally and require power system maintenance at a minimal (at most 4 battery changes per year).
    - Rationale: Remote field deployment around Cookeville, Tennessee makes frequent battery swaps impractical and drives recurring labor and battery replacement costs.
- TCS-PS2 (Surge Protection): The system shall incorporate surge protection devices complying with IEEE Std C62.41 to withstand high-voltage transient overvoltages and lightning-induced surges.
    - Rationale: Outdoor and substation operation exposes power entry to fast transients that could damage downstream regulation and sensing hardware.
- TCS-PS3 (Supercapacitor Charging via Telluric Currents — Voided): The original proposal included charging via telluric currents through a supercapacitor bank with no separate charging element.
    - Rationale: Negotiated as void with Dr. Van Neste due to complexity; replaced by the Voltaic V75 "Always-On" power bank and 6 W solar panel architecture.
- TCS-PS4 (Low-Noise Regulators & LDOs): Regulator and LDO outputs shall meet low-noise requirements so no measurable power supply noise leaks into the sensor's bandwidth.
    - Rationale: Telluric measurements rely on small voltage readings where supply ripple or switching harmonics can degrade harmonic FFT.

**2. Power Subsystem Metrics and Rationale:**
- SR-01 (Output Regulation): The power distribution board shall provide regulated DC outputs of 3.3 V and 5 V within ± 2% voltage regulation tolerance under all operating load steps.
    - Rationale: Rail regulation keeps digital logic, data acquisition stages, and conditioning circuits within reliable operating limits.
- SR-02 (Power Quality and Ripple): The power conditioning circuitry shall limit high-frequency switching ripple to less than 10 µV peak-to-peak to prevent noise contamination of the analog front-end.
    - Rationale: Low-frequency telluric signals are sensitive to switching noise coupling into the analog front-end.
- SR-03 (Galvanic Isolation): The power supply architecture shall maintain a reinforced isolation barrier rated for at least 2 kV between external power inputs and internal instrument grounds.
    -Rationale: Substation Ground Potential Rise (GPR) and fault currents can create dangerous potential shifts and ground loops if external and internal grounds are tied together.
- SR-04 (Autonomous Operation): The primary power architecture shall sustain continuous, unattended operation for at least 3 months (90 days) under normal field operation.
    - Rationale: Multi-month autonomy reduces field maintenance visits and aligns with maintenance goals.
- SR-05 (Transient Response): The regulation and post-regulation stages shall maintain voltage stability and limit transient sags to less than 5% during sudden microcontroller loads.
    - Rationale: Microcontroller wakeups and cellular transmission bursts create rapid current steps that could cause brownouts or resets without adequate transient response.
- SR-06 (Regulatory and Battery Safety Alignment): Power subsystem component placement, wiring methods, and energy storage integration shall use relevant NEC design guidance (Articles 250, 690, and 725) and use a commercial battery pack with an integrated hardware Battery Management System (BMS). While UL 2056 and UL 1642 were initially referenced to capture lithium cell and power-bank safety expectations, they are no longer used as requirements here because documentation on the selected Voltaic Systems V75 pack identifies UN 38.3, IEC 62133, CE, and FCC rather than UL 2056 or UL 1642.
    - Rationale: UL 2056 and UL 1642 were originally included during early specification planning as familiar safety benchmarks for a 72 Wh lithium power bank, but moving away from them keeps the specification tied to known hardware ratings (IEC 62133, UN 38.3, CE, FCC, and internal BMS protections) and practical capstone scope. The UL 1642 applies to individual lithium cells and UL 2056 applies to standalone power banks, not to custom external circuitry or an entire field measurement system; if the TC-GFI were ever commercialized, safety evaluation would extend beyond just the power bank it would evaluate the finished equipment assembly and utility/environmental requirements. Using NEC Articles 250, 690, and 725 as design references supports clean ground-reference separation, inline DC/solar overcurrent protection, and physical spacing across low-voltage and isolated regions.

**3. PMCS Subsystem Requirements and Rationale:**
- SRS-PMCS-001 (5 V Input Compatibility): The PMCS shall accept standard 5 V DC power inputs from the Voltaic V75 USB battery pack or a compatible USB supply (wall adapter or laptop port).
    - Rationale: Standard 5 V USB intake supports transition between field battery power and laboratory/bench testing supplies.
- SRS-PMCS-002 (Input-to-Analog Ground Isolation): The PMCS shall incorporate a galvanic isolation barrier between the external power input ground and the internal instrument analog ground.
    - Rationale: Separates external supply ground disturbances and ground loops from sensitive analog measurement reference nodes.
- SRS-PMCS-003 (Regulated DC Rails): The PMCS shall supply stable DC voltage rails suitable for powering digital microcontrollers and analog signal conditioning circuitry under standard operating loads.
    - Rationale: Downstream MCU, telemetry, and DAQ depend on clean, properly sized voltage rails to operate reliably.
- SRS-PMCS-004 (Ripple Filtering): The PMCS power conditioning circuitry shall utilize passive or active filtering to reduce switching ripple and high-frequency noise on the analog supply rail.
    - Rationale: Pre-conditioning helps strip high-frequency switching artifacts before sensitive post-regulation and analog sensing stages.
- SRS-PMCS-005 (High-PSRR Linear Post-Regulation): The PMCS shall utilize linear post-regulation with a high Power Supply Rejection Ratio (PSRR) to isolate sensitive measurement components from upstream supply fluctuations.
    - Rationale: High-PSRR post-regulation helps to contain the switching ripple down to microvolt levels on the analog rail.
- SRS-PMCS-006 (Supply Health Monitoring): The PMCS shall provide a monitoring interface to allow the system microcontroller to track incoming supply health or battery status.
    - Rationale: Enables battery/supply visibility and supports fail-safe status reporting rather than silent power degradation.
- SRS-PMCS-007 (NEC Compliance): All component placement, internal wiring methods, and enclosure designs within the PMCS shall comply with National Electrical Code (NEC) requirements, specifically NFPA 70 Articles 250, 690, and 725.
    - Rationale: Establishes safe grounding/bonding, solar PV circuit protection, and low-voltage circuit segregation.
- SRS-PMCS-008 (Pass-Through Solar Charging): The PMCS shall continue to supply regulated output rails without interruption or reset while the P106-S solar panel recharges the battery pack.
    - Rationale: Prevents measurement gaps or telemetry dropouts during daytime solar recharging.
- SRS-PMCS-009 (Subsystem Load Support): The PMCS shall support the load pulled by the Telecommunication, MCU, Microcontroller, and Data Acquisition systems.
    - Rationale: Ensures all active subsystems receive required current without starving or overloading the power stage.

**4. Power System Constraints:**
- Voltage Regulation & Noise: Distribution and regulation stages must hold tight DC tolerances with low ripple to prevent high-frequency noise from interfering with measurements.
  - Rationale: Protects microvolt-scale telluric signals and FFT harmonic accuracy.
- Transient & Surge Handling: The system must handle rapid load steps and protect against external voltage spikes or surges.
  - Rationale: Preserves hardware reliability during load transitions and outdoor transient events.
- Electrical Isolation: Analog and processing stages must maintain a $2\,\text{kV}$ galvanic isolation barrier to prevent high Ground Potential Rise (GPR), fault damage, and ground loops.
  - Rationale: Substation proximity creates elevated ground potential and loop risks.
- Power & Financial Budgets: Power draw must be minimized for remote unattended operation (targeting ~90-day runtime), and BOM cost must stay within subsystem/project limits ($1,000 system cap, $100/year operating target, with power maintenance budgeted within limits).
  - Rationale: Satisfies socio-economic and budget constraints and keeps the unit practical for rural utility cooperatives and research deployment.
- Environmental & Site Conditions: The system must operate across Cookeville, TN temperature extremes ($-30^\circ\text{C}$ to $41^\circ\text{C}$ per TCS-WS2, with enclosure thermal rise and the Voltaic V75's $0^\circ\text{C}$ to $45^\circ\text{C}$ charge and $-15^\circ\text{C}$ to $60^\circ\text{C}$ discharge cutoffs accounted for) and follow site orientation and safety rules.
  - Rationale: Seasonal weather swings and field installation conditions directly affect battery behavior, cold-weather solar charging limits, thermal margins, and signal quality.
- Legal, Thermal, and Chemical Constraints: Wiring, grounding, and energy storage integration shall apply relevant NEC design guidance (Articles 250, 690, and 725) and rely on a commercial battery pack with an integrated hardware BMS and documented IEC 62133 and UN 38.3 ratings rather than uncertified custom cells or formal UL 2056 / UL 1642 certification (which are not required for this capstone prototype; NEC Article 706 and NFPA 855 thresholds are noted as non-applicable due to the ~72 Wh pack size).
  - Rationale: Aligns the prototype with verifiable battery safety/transport ratings and practical electrical design references while mitigating lithium thermal, short-circuit, and electrical hazards.

**5. PMCS Codes and Standards Table:**
| Code / Standard | Role in PMCS Detailed Design | How the PMCS Designs to Fit It |
| :--- | :--- | :--- |
| **IEC 62133-2 & UN 38.3** | Battery safety & transport reference | Use pre-built Voltaic V75 pack with internal BMS; observe 0 °C to 45 °C charge and -15 °C to 60 °C discharge cutoffs. |
| **NEC Art. 690** | Solar/DC wiring & fuse guidance | Add 1.5 A inline overcurrent fusing, proper wire/connector ratings, and solar input limits (P106-S into V75). |
| **NEC Art. 250** | Grounding/bonding reference | Document `AGND` vs. `GND` single-point reference (no metal box chassis on ZQ-161407) and verify reference continuity. |
| **NEC Art. 725** | Low-voltage circuit separation | Keep low-voltage DC/AFE signals segregated from external/field wiring across the isolation boundary. |
| **IPC-2221 & IEC 60664-1** | PCB clearance & creepage spacing | Maintain a physical PCB isolation moat/slot between `GND` and `AGND` sized for the 2 kV isolation barrier and PD2 environment. |
| **NEMA 250 / IEC 60529** | Environmental & ingress protection | House the V75 battery pack and PMCS electronics inside a sealed NEMA 6P / IP68 enclosure (ZQ-161407) with weatherproof glands. |
| **IEEE Std 80** | Substation GPR / isolation reference | Provide 2 kV galvanic isolation across DC-DC and SPI signal paths to prevent GPR damage and ground loops. |
| **IEEE Std C62.41** | Surge waveform reference | Apply input surge/TVS clamping (`SMBJ5.0A`) and current limiting ahead of the isolation barrier; avoid calling it a certification. |
| **IEEE Std 1100** | Sensitive analog power/grounding | Use LC Pi-filtering and LT3042 high-PSRR LDO to keep analog rail noise under 10 µV RMS across the telluric band. |

#**Overview of Proposed Solution**
--- 
The PMCS for the TC-GFI delivers conditioned, galvanically isolated, and low noise DC power to both the digital and analog telluric measurement front-end while supporting multi month unattended field operations. Instead of using a custom high-voltage conversion or raw lithium cells, the proposed architecture pairs a commercial pass through solar/battery power source with a single board conditioning and isolating stage housed inside a sealed NEMA 6P/IP68 enclosure (ZQ-161407). 

**1. Architecture Strategy
To protect micro-volt telluric signals from the digital switching noise and prevent ground loops or Ground Potential Rise (GPR) near substation testing from in accurate measurements. The PMCS will use a split system for the isolated analog front-end and the non-isolated digital/telemetry side.
- **Energy Harvesting and Storage Front-End**: A Voltaic Systems P106-S 6W / 6V solar panel feeds pass-through charging into a Voltaic Systems V75 "Always-On" lithium-ion power bank (20,100 mAh / 72 Wh) with built in BMS (IEC 62133 / UN 38.3) Using an "Always-On" pack such as the V75 prevents low current shut off during microamp/milliamp sleep windows while keeping the TC-GFI withing budget and safety limits.
- **Primary Input Protection:** The V75's 5 V output enters the PMCS through a 1.5 A inline fuse, and SMBJ5.0A TVS diode for transient/surge clamping, and bulk decoupling (47 µF electrolytic & 0.1 µF ceramic) to stabilize load steps and feed the non-isolated digital/telemetry side (GND). 
