# Detailed Design - Power Management and Conditioning Subsystem (PMCS)
---
The Power Management and Conditioning Subsystem (PMCS) is responsible for supplying stable, uninterrupted electrical energy to all operational components and peripherals of the Telluric Current Ground Fault Indicator (TC-GFI). Its primary function is to accept, isolate, condition, and distribute the electrical power required by the microcontroller, telecommunication module, data acquisition system (DAQ), and active analog front-end components. This subsystem ensures that each internal load receives power at the correct voltage level while maintaining exceptional electrical isolation and minimal noise to protect sensitive microvolt-level earth potential measurements.

At a high level, the PMCS accepts unconditioned power from an external 5 V DC source, such as the Voltaic Systems V75 "Always-On" rechargeable power bank, with auxiliary off-grid charging provided by the plug-in 6 W monocrystalline solar panel. Once energy is delivered to the internal power conditioning network, it is routed through a certified board-mount isolated DC-DC converter (RECOM/Murata 1 W SIP4) that establishes a reinforced 2 kV galvanic isolation barrier. This barrier is critical to eliminating earth ground loops and protecting downstream instrumentation from the high Ground Potential Rise (GPR) and transient surges experienced when operating in high-voltage substation environments.

Following isolation, the power network converts and conditions energy into independent regulated digital and ultra-low-noise analog power rails using high-PSRR linear post-regulators (Analog Devices LT3042). These rails power all necessary computational and sensing components, ensuring stable voltage conditions and suppressing high-frequency switching ripple below 10 µV RMS, even during heavy active LTE transmission load steps.

Rather than relying on fixed line power or direct high-voltage line attachment like conventional fault indicators, this architecture supports standalone, portable operation for non-contact telluric monitoring. Out in the field, the "Always-On" pack avoids low-current sleep shutoffs while pass-through solar charging helps sustain longer deployments; when testing, a standard USB wall adapter or host port can power the system.

Overall, the PMCS is the foundation that supports the accuracy, safety, and field reliability of the TC-GFI. By combining galvanic isolation with clean, regulated power rails, it allows the sensing front-end, ADCs, and wireless telemetry to work together reliably for ground fault detection. 

# Specifications and Constraints 

---

