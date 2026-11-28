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
| **NEC Art. 250** | Grounding/bonding reference | Document AGND vs. GND single-point reference (no metal box chassis on ZQ-161407) and verify reference continuity. |
| **NEC Art. 725** | Low-voltage circuit separation | Keep low-voltage DC/AFE signals segregated from external/field wiring across the isolation boundary. |
| **IPC-2221 & IEC 60664-1** | PCB clearance & creepage spacing | Maintain a physical PCB isolation moat/slot between GND and AGND sized for the 2 kV isolation barrier and PD2 environment. |
| **NEMA 250 / IEC 60529** | Environmental & ingress protection | House the V75 battery pack and PMCS electronics inside a sealed NEMA 6P / IP68 enclosure (ZQ-161407) with weatherproof glands. |
| **IEEE Std 80** | Substation GPR / isolation reference | Provide 2 kV galvanic isolation across DC-DC and SPI signal paths to prevent GPR damage and ground loops. |
| **IEEE Std C62.41** | Surge waveform reference | Apply input surge/TVS clamping (SMBJ6.0A) and current limiting ahead of the isolation barrier; avoid calling it a certification. |
| **IEEE Std 1100** | Sensitive analog power/grounding | Use LC Pi-filtering and LT3042 high-PSRR LDO to keep analog rail noise under 10 µV RMS across the telluric band. |

# **Overview of Proposed Solution**
--- 
The PMCS for the TC-GFI delivers conditioned, galvanically isolated, and low noise DC power to both the digital and analog telluric measurement front-end while supporting multi month unattended field operations. Instead of using a custom high-voltage conversion or raw lithium cells, the proposed architecture pairs a commercial pass through solar/battery power source with a single board conditioning and isolating stage housed inside a sealed NEMA 6P/IP68 enclosure (ZQ-161407). 

**1. Architecture Strategy**
To protect micro-volt telluric signals from the digital switching noise and prevent ground loops or Ground Potential Rise (GPR) near substation testing from inaccurate measurements. The PMCS will use a split system for the isolated analog front-end and the non-isolated digital/telemetry side.
- **Energy Harvesting and Storage Front-End**: A Voltaic Systems P106-S 6W / 6V solar panel feeds pass-through charging into a Voltaic Systems V75 "Always-On" lithium-ion power bank (20,100 mAh / 72 Wh) with built in BMS (IEC 62133 / UN 38.3). Using an "Always-On" pack such as the V75 prevents low current shut off during microamp/milliamp sleep windows while keeping the TC-GFI within budget and safety limits.
- **Primary Input Protection:** Power exits the V75's USB-A "Always-On" port using a 20 AWG bare-wire pigtail harness through a master toggle switch and a 1.5 A inline fuse into a 2-pin PCB screw terminal. Than into a SMBJ6.0A TVS diode for transient/surge clamping and bulk decoupling (47 µF electrolytic & 0.1 µF ceramic) to stabilize load steps and feed the non-isolated digital (GND).
- **2 kV Galvanic Isolation Barrier:** A RECOM/Murata 1 W 5V-to-5V isolated DC-DC converter with a digital isolator across SPI signal lines establishes a 2 kV galvanic isolation barrier separating non-isolated (GND) from isolated analog (AGND) across a physical PCB slot sized using IPC-2221 and IEC 60664-1.
- **Ultra-Low-Noise Analog Rail:** On the isolated AGND side, the 5 V rail passes through an LC Pi-filter (ferrite bead + 10 µF MLCC) into two parallel Analog Devices LT3042 ultra-low-noise, high-PSRR LDOs (< 10 µV RMS ripple) one with a 33 kΩ RSET resistor for the 3.3 V rail (MCP3460R ADCs, AFE, and isolator secondary) and one with a 25 kΩ RSET resistor for the 2.5 V rail (AD7352 ADCs).
- **Battery / Supply:** A 100 kΩ / 100 kΩ resistor voltage divider will be used to scale the supply voltage into the nRF9151 SAADC input so the system can log battery health and detect low charge or cold weather charging pauses (0 °C to 45 °C charge cutoff; -15 °C to 60 °C discharge cutoff on the V75).

**2. Why the Architecture fits the TC-GFI:**
- **Noise and Signal Integrity:** Separating GND and AGND and post regulating with the LT3042 after LC filtering isolates the sensitive FFT measurement circuits from digital switching activity.
- **Substation and Ground Loops:** Galvanic Isolation across both power and SPI paths breaks earth loops and lessens GPR coupling between field ground potentials.
- **Field Practicality:** The V75 + P106-S combination provides high capacity (90-day target runtime on solar assisted charging under duty-cycled operation) with minimal complexity, while the cold weather solar charging limits below 0 °C and discharge cutoffs below -15 °C.

**3. Power Budget**
#### Component Power Budget (Referred to 5.0 V V75 Bus)

| Subsystem / Component | Domain | Rail | Active Current (5 V Bus) | Active Power | Sleep Current (5 V Bus) | Case A: Continuous (24/7) | Case B: Duty-Cycled (10% Active) |
| :--- | :--- | :--- | :---: | :---: | :---: | :---: | :---: |
| **nRF9151 DK (MCU)** | GND | 5.0 V | 15.0 mA | 75.0 mW | 1.0 mA* | 1.800 Wh/day | 0.287 Wh/day |
| **nRF9151 DK (LTE Burst)** | GND | 5.0 V | 300.0 mA | 1,500.0 mW | 0.0 mA | 0.600 Wh/day | 0.300 Wh/day |
| **SPI Digital Isolator (Primary)** | GND | 5.0 V | 2.0 mA | 10.0 mW | 0.45 mA | 0.240 Wh/day | 0.072 Wh/day |
| **Battery Divider (100 kΩ / 100 kΩ) + SMBJ6.0A** | GND | 5.0 V | 0.045 mA | 0.225 mW | 0.045 mA | 0.005 Wh/day | 0.005 Wh/day |
| **Status / Battery Health LEDs** | GND | 5.0 V | Negligible*** | Negligible*** | 0.0 mA | Negligible | Negligible |
| **Isolated Analog Domain (DC-DC, 2× LT3042, 3× MCP3460R, 2× AD7352, AFE, Isolator Sec.)** | AGND | 5.0 V in | 51.4 mA | 257.1 mW | 0.0 mA** | 6.171 Wh/day | 0.617 Wh/day |
| **Total Daily Energy Demand** | **Both** | **5.0 V** | **68.5 mA (base)** | **342.3 mW (base)** | **1.5 mA** | **8.82 Wh/day** | **1.28 Wh/day** |

*\*nRF9151 SiP PSM floor is 2.7 µA; 1.0 mA conservatively budgets DK board overhead.*  
*\*\*\*LEDs draw ~1–2 mA only when pulsed (0.5% duty cycle) or activated via momentary test button, contributing < 0.003 Wh/day.*  
*\*\*\*\*Case A assumes 24 hr/day continuous sensing + 24 min/day (0.4 hr) LTE-M TX. Case B assumes 2.4 hr/day active sensing + 12 min/day (0.2 hr) LTE-M TX + 21.4 hr/day sleep.*

---

#### Solar vs. Non-Solar Runtime

**Energy Baseline:** Voltaic V75 usable capacity = 72.36 Wh × 85% boost efficiency = **61.5 Wh** (≈ 12.3 Ah at 5 V). Voltaic P106-S (6 W) solar harvest assumes 65% end-to-end efficiency (17.55 Wh/day spring–fall; 9.75 Wh/day clear winter; 1.56 Wh/day heavy overcast).

| Operating Scenario / Metric | Case A: Continuous 24/7 (`8.82 Wh/day`) | Case B: Duty-Cycled 10% (`1.28 Wh/day`) |
| :--- | :---: | :---: |
| **Non-Solar Battery Only Runtime** | **7.0 days** | **48.0 days** |
| **Solar-Assisted: Spring–Fall Net Balance (17.55 Wh/day)** | 8.73 Wh/day (Surplus) | 16.27 Wh/day (Surplus) |
| **Solar-Assisted: Clear Winter Net Balance (9.75 Wh/day, > 0 °C)** | 0.93 Wh/day (Surplus) | 8.47 Wh/day (Surplus) |
| **Solar-Assisted: Normal Year-Round Timeline** | **Indefinite (Spring–Fall); Winter-Limited** | **Indefinite (90+ days)** |
| **Heavy Overcast Survival (1.56 Wh/day)** | **8.5 days** (7.26 Wh/day deficit) | **Indefinite** (0.28 Wh/day surplus) |
| **Continuous Sub-0 °C Freeze Survival (0 Wh/day)** | **7.0 days** (8.82 Wh/day deficit) | **48.0 days** (1.28 Wh/day deficit) |
| **5 Day Freeze Battery Drain** | 44.1 Wh (71.7% of usable pack) | 6.4 Wh (10.4% of usable pack) |
| **Winter Recovery Time (After 5 Day Freeze)** | **47.4 clear winter days** | **0.8 clear winter days (< 1 day)** |

# **System Architecture**
---

The overall architecture of the Power Management and Conditioning Subsystem (PMCS) is as follows:
1. Solar energy is harvested by a Voltaic Systems P106-S 6 W / 6 V solar panel and fed through an IP68 weatherproof cable gland directly into the dedicated solar input of the V75 battery pack.
2. Energy is stored in a Voltaic Systems V75 "Always-on" lithium-ion power bank (20.100 mAh / 72.36 Wh; 61.5 Wh usable at 5V) with built in BMS and pass-through charging.
3. The 5 V DC output exits one of the V75's USB-A "Always-On" ports using a 20 AWG bare-wire pigtail harness, through a master toggle switch and 1.5 A inline fuse into a 2-pin PCB screw terminal. Then goes through a low-leakage SMBJ6.0A TVS diode and 47 µF + 0.1 µF bulk decoupling to power the non-isolated digital (GND), including the Nordic nRF9151 DK and battery divider.
4. A 2 kV galvanic isolation barrier formed by a RECOM/Murata 1 W 5V-to-5V isolated DC-DC converter (3 kV-rated variant), SPI digital isolators , and a physical copper-free PCB moat/slot sized per IPC-2221 and IEC 60664-1 (PD2) this separates noisy digital GND from quiet analog AGND.
5. The isolated AGND side, an LC Pi-filter and two parallel Analog Devices LT3042 ultra-low-noise LDOs regulate the rail to clean 3.3 V and 2.5 V outputs (< 10 µV RMS) for the ADCs (3× MCP3460R, 2× AD7352), analog front-end, and isolator secondary, with electronics inside a sealed NEMA 6P / IP68 enclosure (ZQ-161407).

**1.Solar Harvesting:**
To support multi-month unattended field deployment, the V75 is paired with the Voltaic Systems P106-S 6 W / 6 V monocrystalline solar panel. The panel produces a maximum power voltage of 6.5 V, an open-circuit voltage of 7.67 V, a peak current of 960 mA, and a short-circuit current of 1.02 A, fitting directly within the V75's 5–6 V / 2 A dedicated solar input rating without needing an external charge controller. Under Cookeville, TN weather conditions (assuming 65% harvest efficiency), the panel has roughly 17.55 Wh/day in spring through fall and 9.75 Wh/day on clear winter days—exceeding both continuous (6.42 Wh/day) and duty-cycled (1.04 Wh/day) system demand whenever temperatures remain above the V75's 0 °C charge cutoff. The solar feed enters the enclosure through a weatherproof cable gland straight to the V75's solar input.
(Image of solar panel)

**2.Energy Storage:**
PMCS uses the Voltaic Systems V75 Always-On USB Battery Pack (20,100 mAh, 72.36 Wh nominal). Accounting for internal 3.6 V-to-5 V boost conversion efficiency (~85%), the pack provides 61.5 Wh of usable energy at the 5 V output bus. Unlike standard consumer power banks that shut down when load current drops into the microamp or milliamp range, the V75's Always-On mode keeps the 5 V bus active during deep-sleep while supporting pass-through solar charging. Its built in hardware BMS is documented to IEC 62133 and UN 38.3 ratings provide over-charge, over-discharge, over-current, short-circuit, and thermal protections, enforcing charging cutoffs at 0 °C to 45 °C and discharge cutoffs at -15 °C to 60 °C. Each 5 V output port is rated up to 2.0 A, providing room above the TC-GFI's ~0.77 A worst-case peak draw.
(image of V75)

**3.Enclosure and Primary Input Protection:**
Both the V75 battery pack and the PMCS PCB are housed inside a sealed ZQ-161407 non-metallic polycarbonate enclosure rated NEMA 6P / IP68 (per NEMA 250 / IEC 60529) to protect against rain, dust, and standing water. Because the enclosure is non-metallic, there is no metal chassis to bond to earth ground; so, grounding focuses strictly on internal GND vs. AGND domain separation.

Where the 5 V power feed from the V75 enters the PMCS PCB, three protective and stabilizing elements sit ahead of all downstream loads:

- USB-A Always-On Pigtail, Master Toggle Switch, & Screw Terminal: A 20 AWG USB-A male to 2-pin bare-wire pigtail connects the V75's USB-A Always-On port through an IP68-rated 3–5 A DC SPST master toggle switch and inline fuse holder into a 2-pin PCB screw terminal. Using 20 AWG wire minimizes voltage drop during 500 mA LTE-M transmit bursts.

- 1.5 A Inline Fuse: Sized at roughly 150–200% of the ~0.77 A worst-case simultaneous peak current to prevent tripping during cellular transmit bursts or startup spikes, while staying safely below the V75's 2.0 A port limit so hard faults blow clean.
(image of inline fuse)

- Low-Leakage SMBJ6.0A TVS Diode: Placed downstream of the fuse across +5 V and GND (cathode to +5 V, anode to GND) to clamp ESD and inductive transients (600 W peak pulse rating, 6.67 V minimum breakdown). Selecting a 6.0 V standoff device rather than a 5.0 V part avoids continuous reverse-leakage drain when the V75 outputs 5.05–5.20 V under light load, preserving the 90-day power budget.
(Image of TVS Diode(

- Bulk Decoupling (47 µF Electrolytic + 0.1 µF Ceramic): Placed immediately after the protection stage to act as a local charge reservoir during sudden 300–500 mA nRF9151 LTE-M transmit bursts (limiting transient voltage sag to < 5%) while shunting high-frequency digital switching noise to GND.
(image of box and capacitors)

**4,5.Isolated Split-Rail PCB:**
To prevent cellular switching noise, earth ground loops, and substation Ground Potential Rise (GPR, per IEEE Std 80) from corrupting microvolt-scale telluric measurements, the PMCS PCB is physically partitioned into two electrically separate domains with zero shared copper:

- Non-Isolated Digital (GND): Feeds the protected 5 V bus directly to the Nordic nRF9151 DK (VIN 5V, rated 4.0–5.5 V), the primary side of the isolated DC-DC converter, and the primary side of the SPI digital isolators. A 100 kΩ / 100 kΩ resistor voltage divider scales the 5 V supply rail by half (2.5 V , drawing only 25 µA continuously) into the nRF9151's SAADC pin so it can monitor battery health.

- 2 kV Galvanic Isolation Barrier: A RECOM or Murata 1 W 5V-to-5V isolated DC-DC converter ( 3 kV DC-rated variant for comfortable margin above the 2 kV requirement) transfers power magnetically across the barrier, while ADuM-series digital isolators carry the SPI bus (SCK, MOSI, MISO, CS) between the nRF9151 and the ADCs. On the PCB layout, all GND copper is confined to one half and all AGND copper to the other, separated across every layer by a 6.3–8.0 mm copper-free isolation moat (with an optional milled air slot) sized per IPC-2221 and IEC 60664-1 for a Pollution Degree 2 (PD2) environment.

- Ultra-Low-Noise Analog (AGND): On the secondary side of the isolated DC-DC converter, switching ripple and high-frequency spikes pass through an LC Pi-filter consisting of a series ferrite bead with two 10 µF MLCC capacitors (chosen over the 4.7 µF minimum to maintain capacitance under 5 V DC bias). The filtered 5 V rail feeds an Analog Devices LT3042 ultra-low-noise, high-PSRR LDO (79 dB PSRR at 1 MHz). A single 33 kΩ RSET resistor tied from the SET pin to AGND programs the internal 100 µA precision current source to output 3.3 V ($V_{\text{OUT}} = 100\,\mu\text{A} \times 33\,\text{k}\Omega = 3.3\,\text{V}$), paired with a 4.7 µF CSET bypass capacitor to drive output noise down to ~0.8 µV RMS (well below the < 10 µV RMS telluric band target). At ~20 mA active analog load, the LT3042 dissipates only ~44 mW, requiring no heatsink. This clean 3.3 V rail powers the ADR441 2.5 V voltage reference, AD7352 and MCP3460R ADCs, analog front-end amplifiers, and the secondary side of the SPI digital isolators.(Suggested Figure: Isolated Split-Rail PCB Partitioning / Block Diagram)

# **Interface with Other Subsystems**

---


| Interface ID | Signal / Rail | Source | Destination | Ground Domain | Nominal Level / Spec | Purpose & Notes |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **IF-PMCS-01** | Solar Input | Voltaic P106-S Panel | Voltaic V75 Solar Port | Isolated input / GND | 6.5 V Vmp / 7.67 V Voc, 0.96 A max | Enters ZQ-161407 via weatherproof gland; managed by V75 BMS (0–45 °C charge window). |
| **IF-PMCS-02** | Primary 5 V Bus | Voltaic V75 USB-A Always-On Port | PMCS PCB Screw Terminal | GND | 5.0 V DC, 2.0 A port limit | 20 AWG USB-A bare-wire pigtail routed through master toggle switch and 1.5 A inline fuse, followed by SMBJ6.0A TVS and 47 µF + 0.1 µF decoupling. |
| **IF-PMCS-03** | Digital 5 V Rail (+5V_DIG) | PMCS Protected 5 V | Nordic nRF9151 DK (VIN 5V) | GND | 5.0 V DC (4.0–5.5 V allowed), 500 mA peak | Powers MCU, DSP, and LTE-M/GNSS cellular modem bursts via board headers (with test jumper). |
| **IF-PMCS-04** | Battery Sensor | 100 kΩ / 100 kΩ Divider | nRF9151 DK SAADC Pin | GND | 2.5 V (0–3.0 V range), 25 µA | Continuous supply voltage monitoring on non-isolated digital side. |
| **IF-PMCS-05** | Isolated Power Transfer | Primary 5 V (GND) | Isolated 5 V (AGND) | GND → AGND | 5 V to 5 V, 1 W max, ≥ 2 kV isolation | RECOM/Murata 3 kV-rated isolated DC-DC converter spanning the 6.3–8.0 mm PCB moat. |
| **IF-PMCS-06** | Isolated SPI Bus | nRF9151 DK (GND) | 3× MCP3460R & 2× AD7352 (AGND) | GND ↔ AGND | SCK, MOSI, MISO, CS (≥ 2 kV iso) | Routed through ADuM-series digital isolators across PCB moat; handles level shifting if DK uses 1.8 V logic. |
| **IF-PMCS-07** | Clean Analog 3.3 V (+3.3V_ANA) | PMCS LT3042 #1 (33 kΩ RSET) | 3× MCP3460R, AFE amps, AD7352 VDRIVE, Isolator Sec. | AGND | 3.3 V DC ±1%, 11.2 mA active, < 10 µV RMS | Post-regulated after LC Pi-filter; includes test jumper for independent bring-up. |
| **IF-PMCS-08** | Clean Analog 2.5 V (+2.5V_ANA) | PMCS LT3042 #2 (25 kΩ RSET) | 2× AD7352 (VDD) | AGND | 2.5 V DC ±1%, ~20.8 mA active, < 10 µV RMS | Dedicated low-noise supply within AD7352 2.25–2.75 V VDD spec; includes test jumper. |
| **IF-PMCS-9** | Status / Battery Health LEDs | nRF9151 DK GPIOs | External / Panel LEDs | GND | 1.8 V / 3.0 V logic, ~1–2 mA per LED | Power Status / Low-Battery LEDs (pulsed or press-to-test to protect power budget); isolated from AGND. |

#**Schematic / PCB / Flowchart / LTSpice?**

---


#**BOM**

---


#**Analysis:**

---

This section evaluates the Power Management and Conditioning Subsystem (PMCS) against customer specifications (TCS-PS, TCS-WS, TCS-F), subsystem requirements (SR-01 through SR-06), and electrical standards (IEEE Std 80, IEEE Std 1100, IEEE Std C62.41, IEC 60664-1, and NFPA 70).

### 1. Analog Power Quality, Ripple Effects, and Noise (SR-02, TCS-PS4, IEEE Std 1100)
To prevent switching harmonics from leaking into the telluric FFT band, the analog rails (+3.3V_ANALOG and +2.5V_ANALOG) must maintain switching ripple below 10 µV p-p and band noise below 10 µV RMS.

#### Switching Ripple Suppression (LC Pi-Filter + LT3042 PSRR)
The 1 W isolated DC-DC converter (RECOM/Murata 5V-to-5V) produces a output ripple of V_ripple = 50 mV p-p around its 100 kHz switching frequency, along with high-frequency switching spikes (> 1 MHz).
- **Pre-Filtering:** The passive LC Pi-filter (ferrite bead + 10 µF MLCCs) reduces megahertz-range switching where linear regulator PSRR naturally fades.
- **LDO Post-Regulation:** Downstream, both LT3042 LDOs provide at least 75 dB of PSRR at 100 kHz. The residual switching ripple reaching the ADCs is:
  - V_ripple = V_ripple,in × 10^(-PSRR / 20)
  - V_ripple,out = 50 mV × 10^(-75 / 20) = 50 mV × 0.0001778 = 8.89 µV p-p
- **Result:** Meets the < 10 µV p-p limit of SR-02.

#### LDO Output Noise (0.80 µV RMS) and CSET Sizing
The LT3042 uses an internal 100 µA reference current source (I_noise = 6 nA RMS) into RSET:
- **Bypassing the SET Pin:** Adding CSET = 4.7 µF in parallel with RSET forms a low-pass filter with a -3 dB corner frequency of f_c = 1 / (2π × 33 kΩ × 4.7 µF) = 1.03 Hz on the 3.3 V LDO (and 1.35 Hz with 25 kΩ on the 2.5 V LDO). Because both corners sit below 10 Hz, reference current noise and resistor thermal noise are shunted directly to AGND.
- **Total Integrated Noise (10 Hz – 100 kHz):** Integrating the remaining buffer white noise (2 nV/√Hz × √100,000 Hz = 0.63 µV RMS) in with low-frequency 1/f flicker noise (0.49 µV RMS) yields:
  - V_RMS,total = √(0.63² + 0.49²) = 0.80 µV RMS
- **Startup Trade-Off:** Using CSET = 4.7 µF increases LDO startup ramp time to 360–550 ms (without fast start-up), which has no impact on a continuously powered or 10%-duty-cycle field sensor.
- **Rail Splitting Advantage:** Separating the 2× AD7352 core supplies (+2.5V_ANALOG, 2.25–2.75 V spec) onto a dedicated LT3042 limits high-speed SAR ADC switching currents from the +3.3V_ANALOG rail used by the 3× MCP3460R ADCs and the analog front-end.

---

### 2. Voltage Regulation and Decoupling Analysis (`SR-01`, `SR-05`)
- **Analog Rail Regulation (±2% Limit):** The LT3042 specifies ±1% SET-pin current accuracy and < 1 mV load regulation. Paired with RSET resistors (33 kΩ and 25 kΩ), total DC output error remains within ±1.1% across load and temperature, comfortably satisfying SR-01 (±2%).
- **Digital Rail Decoupling & Load-Step Math (< 5% / 250 mV Sag Limit):** The nRF9151 DK draws up to 300–500 mA during LTE-M transmit bursts. Routing +5V_DIGITAL directly from the Voltaic V75 (2.0 A port rating) rather than through the 200 mA isolated DC-DC converter prevents brownouts. Modeling a 300 mA LTE-M current step over a 20 µs converter (Δt) response window across the 47 µF bulk capacitor (C):
  - ΔV = (I_step × Δt) / C = (0.300 A × 20 µs) / 47 µF = 128 mV (2.56% of 5.0 V)
- **Result:** Remains well below the 5% (250 mV) transient sag limit of SR-05, while the parallel 0.1 µF MLCC shunts high-frequency (MHz) switching transients locally.

---

### 3. Thermal Analysis and Component Derating (`TCS-WS2`)
Inside the sealed ZQ-161407 polycarbonate enclosure under peak Tennessee summer sun (41 °C), internal enclosure temperature is modeled at T_A = 50 °C. Using the LT3042 thermal resistance (θ_JA = 41 °C/W, max junction limit T_J,max = 125 °C):

- **LT3042 #1 (+3.3V_ANALOG):** Supplies 11.2 mA active load + 2.0 mA quiescent current from the 5.0 V isolated rail:
  - P_D1 = (5.0 V - 3.3 V) × 0.0112 A + (5.0 V × 0.002 A) = 19.0 mW + 10.0 mW = 29.0 mW
  - T_J1 = 50 °C + (0.029 W × 41 °C/W) = 51.2 °C (73.8 °C safety margin)
- **LT3042 #2 (+2.5V_ANA):** Supplies 20.8 mA active load (2× AD7352) + 2.0 mA quiescent current from the 5.0 V isolated rail:
  - P_D2 = (5.0 V - 2.5 V) × 0.0208 A + (5.0 V × 0.002 A) = 52.0 mW + 10.0 mW = 62.0 mW
  - T_J2 = 50 °C + (0.062 W × 41 °C/W) = 52.5 °C (72.5 °C safety margin)
- **Isolated DC-DC Converter Derating:** Total isolated output load is 180.0 mW on a 1,000 mW (1 W) module (**18% utilization**), leaving 82% power headroom so the converter operates at an ambient temperature with zero forced airflow required.

---

### 4. Galvanic Isolation, Input Protection, and Standards (`SR-03`, `SR-06`, `TCS-PS2`)
- **2 kV Galvanic Isolation Barrier (IEEE Std 80, IEC 60664-1, NEC Art. 725):** Both power (3 kV rated RECOM/Murata isolated DC-DC) and SPI data lines (≥ 2.5 kV digital isolators) bridge a dedicated 6.3–8.0 mm copper free PCB isolation moat separating GND and AGND. This exceeds the 6.3 mm clearance target for NEC Art. 725 and IEC 60664-1 creepage/clearance requirements, blocking substation Ground Potential Rise (GPR) and earth ground loops.
- **Inline Fuse Sizing (1.5 A, TCS-PCB1):** An off board 3–5 A DC master toggle switch and 1.5 A inline fuse sit in the enclosure harness between the V75 and PCB, keeping primary protection off the PCB per TCS-PCB1:
  - I_fuse = 1.5 A × 0.75 = 1.125 A
  - Satisfies I_peak (0.50 A) < I_fuse,derated (1.125 A) < I_V75,max (2.0 A) providing > 2× room above peak LTE bursts to prevent trips while below the V75's 2.0 A port limit.
- **TVS Diode Selection & Clamping (SMBJ6.0A, IEEE Std C62.41):** A 5.0 V TVS (SMBJ5.0A) sits too close to the V75's nominal 5.0–5.2 V bus and risks reverse-leakage current that wastes battery energy. Selecting the SMBJ6.0A (600 W peak rating) satisfies the protection window:
  - Vmax (5.2 V) < V (6.0 V) < Vmin (6.67 V) 
  - V_C = 10.3 V (at I = 58.3 A, 10/1000 µs surge) < Vmax of downstream regulators (20 V on LT3042), clamping transients before they damage electronics.
- **Energy Storage Safety (SR-06, NEC Art. 706):** Using the Voltaic V75 pre-certified UL 2056, UL 1642, IEC 62133, and UN 38.3 cell and BMS protections (overcharge, over-discharge, short-circuit, and thermal cutoff), eliminating custom lithium-pack thermal runaway risk.

---

### 5. Solar Harvest, Autonomy Math, and Winter Tradeoffs (`SR-04`, `TCS-PS1`, `TCS-WS2`)

#### Solar Panel Harvest Math (Voltaic P106-S, 6 W)
Daily harvested energy is modeled as E_solar = P_panel × PSH × η_sys, using P_panel = 6 W, conservative charging/MPPT efficiency η_sys = 0.65, and Cookeville, TN Peak Sun Hours (PSH):
- **Spring–Fall (4.5 PSH):** E_solar = 6 W × 4.5 h × 0.65 = 17.55 Wh/day
- **Clear Winter (2.5 PSH, > 0 °C):** E_solar = 6 W × 2.5 h × 0.65 = 9.75 Wh/day
- **Heavy Overcast (0.4 PSH):** E_solar = 6 W × 0.4 h × 0.65 = 1.56 Wh/day

#### Case A vs. Case B Daily Energy and Runtime Math
Using the Voltaic V75 usable capacity E_batt = 72.36 Wh × 0.85 = 61.5 Wh and daily demand E_day = Σ(P_i × t_i):
- **Case A (Continuous 24/7):** 24 h active base (0.3423 W) + 0.4 h LTE(1.5 W):
  - E_day = (0.3423 W × 24 h) + (1.5 W × 0.4 h) = 8.22 + 0.60 = 8.82 Wh/day
  - **Zero Solar Runtime:** T_batt = 61.5 Wh / 8.82 Wh/day = 7.0 days
  - **Winter Tradeoff:** Achieves indefinite spring–fall operation (+8.73 Wh/day surplus), but drops to a +0.93 Wh/day clear-winter surplus. A 5-day sub-0 °C freeze drains 5 × 8.82 = 44.1 Wh (71.7% of usable pack) and requires 44.1 / 0.93 = 47.4 clear winter days to recover making Case A unreliable for year-round 90-day unattended operation on a 6 W panel.
- **Case B (Duty-Cycled 10%):** 2.4 h active base (0.3423 W) + 21.4 h sleep base (0.0075 W) + 0.2 h LTE (1.5 W):
  - E_day = (0.3423 W × 2.4 h) + (0.0075 W × 21.4 h) + (1.5 W × 0.2 h) = 0.822 + 0.160 + 0.300 = 1.28 Wh/day
  - **Zero Solar Runtime:** T_batt,B = 61.5 Wh / 1.28 Wh/day = 48.0 days
  - **Winter Tradeoff:** Maintains a surplus even under heavy overcast skies (+0.28 Wh/day), loses only 5 × 1.28 = 6.4 Wh (10.4% of pack) across a 5-day freeze, and recovers in 6.4 / 8.47 = 0.8 clear winter days, satisfying the 90-day autonomy target (SR-04 / TCS-PS1).

#### Cold-Weather BMS Boundary and UI Power Budget
- **Sub-0 °C / Sub--15 °C Limits (TCS-WS2):** The V75 BMS blocks charging below 0 °C and halts discharge below -15 °C. While Cookeville rarely reaches -15 °C, the customer's extreme temperature spec extends to -30 °C. Documenting this identifies extreme sub -15 °C operation as a known commercial-pack limitation.
- **Battery Monitoring & Status LEDs:** Because the V75 outputs a boost-regulated 5.0 V rail, the 100 kΩ / 100 kΩ divider (25 µA continuous draw) functions primarily as a brownout / cutoff detector (< 4.5 V) rather than state of charge. Driving the status LEDs from nRF9151 GPIOs on GND via a momentary press-to-test button.

---

### 6. Requirements Verification Summary

| Requirement / Spec | Target Metric | Analytical Result | Status |
| :--- | :--- | :--- | :---: |
| **SR-01 (Regulation)** | ±2% DC tolerance | ±1.1% (LT3042 + 0.1% RSET) | **Pass** |
| **SR-02 / TCS-PS4 (Noise)** | < 10 µV p-p ripple / < 10 µV RMS | 8.89 µV p-p ripple; 0.80 µV RMS noise | **Pass** |
| **SR-03 (Isolation)** | ≥ 2 kV galvanic barrier | 3 kV DC-DC + ≥ 2.5 kV SPI iso + 6.3–8.0 mm moat | **Pass** |
| **SR-04 / TCS-PS1 (Autonomy)** | ≥ 90 days unattended | Indefinite (90+ days) in Case B; 48 days zero-solar | **Pass (Case B)** |
| **SR-05 (Transients)** | < 5% (250 mV) sag on load steps | 128 mV (2.56%) sag with 47 µF bulk cap | **Pass** |
| **SR-06 / TCS-PS2 (Safety/Surge)** | IEC 62133, UN 38.3, NEC, IEEE C62.41 | Certified V75 + 1.5 A fuse + SMBJ6.0A (V_C = 10.3 V) | **Pass** |
| **TCS-WS2 (Thermal)** | -30 °C to +41 °C | PCB T_J ≤ 52.5 °C at 50 °C box; V75 cuts off at -15 °C | **Conditional*** |
