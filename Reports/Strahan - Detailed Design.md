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
- SR-06 (Regulatory Compliance): Power subsystem component placement, wiring methods, and energy storage enclosures shall comply with applicable NEC Articles 250, 690, and 725 requirements, and energy storage shall use a battery pack with UL 2056 / UL 1642 certification and a hardware BMS.
    - Rationale: Certified lithium storage with a hardware BMS and code-aligned wiring mitigates thermal runaway, short-circuit, and shock hazards during unattended outdoor operation. 
