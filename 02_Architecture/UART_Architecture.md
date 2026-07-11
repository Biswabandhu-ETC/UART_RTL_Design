# UART Architecture

## 1. Introduction

This document presents the architecture of the Universal Asynchronous Receiver Transmitter (UART) designed using Verilog HDL. It defines the system architecture, functional modules, communication flow, interface specifications, and design assumptions prior to RTL implementation.

The objective of this document is to establish a well-defined hardware architecture that serves as the foundation for implementation, simulation, verification, and future enhancements.

---

# 2. System Overview

The UART is an asynchronous serial communication peripheral used to exchange data between two digital devices without sharing a common clock signal. Instead of transmitting multiple data bits simultaneously, UART converts parallel data into a serial bit stream for transmission and reconstructs the original parallel data at the receiver.

The first version of this project implements a standard **8N1 UART** operating at a fixed baud rate of **9600 bps** using a **50 MHz** system clock.

---

# 3. System Architecture

```text
                     +--------------------------+
                     |  Baud Rate Generator     |
                     +------------+-------------+
                                  |
               +------------------+------------------+
               |                                     |
               |                                     |
      +--------v---------+                 +---------v--------+
      | UART Transmitter |                 | UART Receiver    |
      +--------+---------+                 +---------+--------+
               |                                     ^
               |                                     |
               +-------------- TX Line --------------+
```

The UART architecture is composed of three primary hardware blocks.

| Hardware Block | Primary Function |
|----------------|------------------|
| Baud Rate Generator | Generates baud clock enable pulses for transmission and reception. |
| UART Transmitter | Converts 8-bit parallel data into serial data according to the UART protocol. |
| UART Receiver | Samples serial data and reconstructs the original 8-bit parallel data. |

---

# 4. Hardware Module Description

| Module | Description | Inputs | Outputs |
|---------|-------------|---------|----------|
| Baud Rate Generator | Generates transmission and reception enable pulses based on the system clock frequency. | clk | tx_enable, rx_enable |
| UART Transmitter | Serializes 8-bit parallel data and transmits one bit per baud interval. | clk, rst, wr_enb, enable, data_in | tx, busy |
| UART Receiver | Samples incoming serial data, reconstructs the original byte, and asserts the Ready signal after successful reception. | clk, rst, rx, clk_enable, ready_clear | ready, data_out |
| UART Top Module | Integrates all UART hardware modules and establishes the communication path. | System Signals | UART Outputs |
| Testbench | Generates simulation stimulus and verifies UART functionality. | Test Stimulus | Simulation Results |

---

# 5. Data Flow

```mermaid
flowchart LR
    CPUA[Processor / CPU]
    TX[UART Transmitter]
    LINE[Serial Communication Line]
    RX[UART Receiver]
    CPUB[Receiving Processor]

    CPUA -->|8-bit Parallel Data| TX
    TX -->|Serial Data| LINE
    LINE -->|Serial Data| RX
    RX -->|8-bit Parallel Data| CPUB
```

### Communication Sequence

| Step | Operation |
|------|-----------|
| 1 | Processor writes an 8-bit data byte to the UART transmitter. |
| 2 | The transmitter stores the data internally. |
| 3 | The baud rate generator provides transmit enable pulses. |
| 4 | The transmitter shifts one data bit onto the TX line every baud interval. |
| 5 | The receiver samples the incoming serial data. |
| 6 | After receiving all eight data bits and the stop bit, the receiver reconstructs the original byte. |
| 7 | The Ready signal is asserted to indicate valid received data. |

---

# 6. UART Frame Format

The first version of the UART implements the **8N1 communication format**.

### UART Frame Structure

| Field | Bits | Logic Level | Description |
|--------|------|-------------|-------------|
| Idle | — | Logic High (1) | Communication line remains high when no transmission is taking place. |
| Start Bit | 1 | Logic Low (0) | Indicates the beginning of a UART frame. |
| Data Bits | 8 | LSB First | Contains the transmitted data byte. |
| Parity Bit | 0 | Not Implemented | Reserved for future versions. |
| Stop Bit | 1 | Logic High (1) | Indicates the completion of data transmission. |

### Frame Representation

```text
+------+-------+-----------------------------------+------+
| Idle | Start |     D0 D1 D2 D3 D4 D5 D6 D7       | Stop |
+------+-------+-----------------------------------+------+
|  1   |   0   |        LSB ------------> MSB      |  1   |
+------+-------+-----------------------------------+------+
```

---

# 7. Interface Specification

## 7.1 Baud Rate Generator

| Signal | Direction | Description |
|---------|-----------|-------------|
| clk | Input | System Clock |
| tx_enable | Output | Transmit Baud Enable |
| rx_enable | Output | Receive Baud Enable |

### 7.2 UART Transmitter

| Signal | Direction | Description |
|---------|-----------|-------------|
| clk | Input | System Clock |
| rst | Input | Active High Reset |
| wr_enb | Input | Write Enable |
| enable | Input | Baud Enable |
| data_in | Input | 8-bit Parallel Data |
| tx | Output | Serial Output |
| busy | Output | Transmission Busy Indicator |

### 7.3 UART Receiver

| Signal | Direction | Description |
|---------|-----------|-------------|
| clk | Input | System Clock |
| rst | Input | Active High Reset |
| rx | Input | Serial Input |
| clk_enable | Input | Baud Enable |
| ready_clear | Input | Clears Ready Flag |
| ready | Output | Data Ready Indicator |
| data_out | Output | 8-bit Parallel Output |

---

# 8. Design Specifications

| Parameter | Value |
|------------|-------|
| Communication Protocol | UART |
| Communication Type | Asynchronous |
| Data Width | 8 Bits |
| Baud Rate | 9600 bps |
| System Clock | 50 MHz |
| Start Bits | 1 |
| Stop Bits | 1 |
| Parity | None |
| Transmission Order | Least Significant Bit First |
| Duplex Mode | Full Duplex |

---

# 9. Design Assumptions

| Assumption | Description |
|------------|-------------|
| Clock Frequency | Fixed at 50 MHz |
| Baud Rate | Fixed at 9600 bps |
| Data Width | Fixed 8 Bits |
| Parity | Disabled |
| Stop Bits | One Stop Bit |
| Hardware Flow Control | Not Implemented |
| FIFO | Not Implemented in Version 1 |

---

# 10. Future Enhancements

The architecture has been intentionally designed to support future expansion. Planned improvements include:

| Version | Enhancement |
|----------|-------------|
| V2 | Programmable Baud Rate Generator |
| V3 | Parity Generation and Checking |
| V4 | Configurable Stop Bits |
| V5 | Synchronous FIFO Integration |
| V6 | UART 16550 Compatible Register Set |
| V7 | APB Slave Interface |
| V8 | Interrupt Controller |
| V9 | Hardware Flow Control (RTS/CTS) |

---

# 11. Conclusion

This architecture document establishes the structural organization of the UART subsystem before RTL implementation. The modular design approach improves readability, simplifies verification, and provides a scalable foundation for integrating advanced features such as FIFO buffers, APB communication, interrupt handling, and UART 16550 compatibility in future development stages.
