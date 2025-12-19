# Pipelined Synchronous 8-Bit Carry Select Adder (CSLA)

**Author:** Aritro Sarkar  
**Technology:** 45nm CMOS (FreePDK45)  
**Tools:** Cadence Virtuoso  
**Target Frequency:** 4 GHz  

---

## Abstract

This project presents the design and evaluation of a **pipelined synchronous 8-bit Carry Select Adder (CSLA)** implemented in 45nm CMOS technology. The CSLA architecture improves arithmetic performance by computing sum results in parallel for different carry-in assumptions and selecting the correct output using multiplexers. To further increase throughput and achievable clock frequency, the design is pipelined at the primary inputs and outputs using synchronous D flip-flops. Post-simulation results show correct functionality at **4 GHz** with an average power consumption of **0.96 mW**. 

---

## Architecture Overview

### Block-Level Design

The 8-bit CSLA is divided into:
- One **least-significant 4-bit ripple carry adder (RCA)**  
- Two **most-significant 4-bit RCAs**, operating in parallel  

The lower 4-bit RCA generates:
- Sum outputs `S[3:0]`
- Carry-out `C4`, which serves as the **select signal**

The upper RCAs compute sums assuming:
- Carry-in = 0
- Carry-in = 1  

An array of **2:1 multiplexers** selects the correct upper sum bits `S[7:4]` and final carry-out `C8` based on `C4`. 

<img width="679" height="670" alt="image" src="https://github.com/user-attachments/assets/512e76bd-823a-469f-9ed2-07e9cfb2321a" />
<img width="689" height="374" alt="image" src="https://github.com/user-attachments/assets/8222f7e8-240d-4299-ad06-898f927e905a" />

---

### Pipelining Strategy

To enable high-frequency operation, the design is **pipelined at the primary inputs and outputs** using synchronous reset D flip-flops.

The **critical path** begins at the output of the input-stage flip-flop driving the carry-in, propagates through:
1. The least-significant 4-bit RCA  
2. The carry-select multiplexer logic resolving the MSB (`S7`)  
3. The input of the output-stage flip-flop  

This pipelining allows the CSLA to operate reliably at multi-GHz clock rates.
<img width="642" height="853" alt="image" src="https://github.com/user-attachments/assets/16d95fb2-ced6-4d0e-912a-bae66f77194c" />

---

## Circuit Components

### Synchronous D Flip-Flop

- Transmission-gate-based
- Master–slave configuration
- Synchronous reset
- Edge-triggered behavior via complementary clock phases

The master latch samples the input during one clock phase, while the slave latch transfers the stored value to the output on the opposite phase, ensuring correct edge-triggered operation. 

<img width="1583" height="625" alt="image" src="https://github.com/user-attachments/assets/7c6e0dbb-6bab-4426-be8a-1e2104b6294a" />

---

### 1-Bit Full Adder

- Implemented as a **mirror full adder**
- Fully complementary CMOS logic
- Carry equation:  
  `Cout = AB + (A + B)Cin`

Key advantages:
- At most **two series transistors** on the carry path
- Balanced rise/fall delays
- Reduced transistor count, leading to lower power and area

The sum output is buffered to ensure full rail-to-rail swing and sufficient drive strength. 

<img width="1390" height="864" alt="image" src="https://github.com/user-attachments/assets/50eb35e0-e152-41a7-89b1-13957013b15a" />

---

### 2:1 Multiplexer

- Implemented using **CMOS transmission gates**
- Controlled by complementary select signals (`S` and `S̅`)
- Full rail-to-rail signal transfer without threshold voltage degradation

The symmetric structure ensures balanced delay for both data paths, making it well-suited for large arithmetic designs.

<img width="1203" height="845" alt="image" src="https://github.com/user-attachments/assets/d6e4c9b2-4de1-4ff0-bd91-62e2f115c627" />

---

## Simulation Results

Functional verification was performed using **Cadence Virtuoso** waveform simulations.

Due to:
- Input/output pipelining
- Clock-to-Q delay
- Setup time
- Combinational logic delay  

the output sum is delayed by approximately **two clock cycles**. However, all simulated test cases produce **correct arithmetic results**, validating the CSLA functionality at high clock frequencies.

<img width="1374" height="335" alt="image" src="https://github.com/user-attachments/assets/c7ef1acc-2f50-4657-84e1-abbdbdd94d79" />
<img width="1382" height="343" alt="image" src="https://github.com/user-attachments/assets/a6d1db0a-4316-451b-a7ba-08bcc1d9f996" />

---

## Performance and Power Analysis

| Clock Frequency (GHz) | Average Power (mW) |
|----------------------|-------------------|
| 4                    | 0.96              |
| 6                    | 1.279             |
| 8                    | 1.601             |

As clock frequency increases, power consumption rises; however, the **frequency-to-power ratio improves**, indicating efficient scaling. The use of transmission-gate logic and mirror full adders helps limit transistor count, contributing to reduced average power consumption. :contentReference[oaicite:7]{index=7}

---

## Discussion

Pipelining enables the CSLA to operate at significantly higher clock frequencies, but the design remains constrained by setup/hold requirements and combinational delay. At very high frequencies (≈8 GHz), minor glitches begin to appear, indicating the limits of this pipeline depth and logic organization.

---

## Future Improvements

Potential enhancements include:
- Reducing redundant adder paths by incorporating **carry-bypass logic**
- Adding additional internal pipeline stages with careful resource balancing
- Optimizing transistor sizing to improve critical-path speed while reducing power and area on non-critical paths

---

## Conclusion

This project demonstrates a **high-performance pipelined 8-bit Carry Select Adder** implemented in 45nm CMOS technology. The design achieves correct functionality at **4 GHz** with low power consumption, validating the effectiveness of parallel carry-select logic combined with input/output pipelining. The architecture provides a solid foundation for further optimization in high-speed arithmetic circuits. :contentReference[oaicite:8]{index=8}
