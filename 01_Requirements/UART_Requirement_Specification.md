# UART Requirement Specification

**Project Name:** UART RTL Design

**Version:** 1.0

**Author:** Biswabandhu Sahoo

**Date:** July 2026

---

# 1. Project Overview

The objective of this project is to design a synthesizable Universal Asynchronous Receiver Transmitter (UART) using Verilog HDL. The UART converts parallel data into serial data during transmission and converts serial data back into parallel data during reception.

The design will be implemented using Xilinx Vivado and verified using simulation before FPGA implementation.

---

# 2. Problem Statement

Many embedded systems require reliable serial communication while minimizing the number of communication wires.

UART is one of the most widely used asynchronous serial communication protocols in embedded systems, microcontrollers, FPGAs, and System-on-Chip (SoC) devices.

This project focuses on implementing a UART controller completely from scratch.

---

# 3. Objectives

- Design a Baud Rate Generator
- Design a UART Transmitter
- Design a UART Receiver
- Integrate all modules
- Verify functionality using simulation
- Develop synthesizable RTL

---

# 4. Scope

Version 1 includes:

- Fixed Baud Rate
- 8-bit Data
- One Start Bit
- One Stop Bit
- No Parity

Future versions will include:

- FIFO Buffers
- UART 16550 Features
- APB Interface
- Interrupt Controller

---

# 5. Applications

- FPGA Boards
- Microcontrollers
- GPS Modules
- Bluetooth Modules
- GSM Modules
- Industrial Automation
- Automotive Electronics

---

# 6. UART Frame Format

Idle → Start Bit → 8 Data Bits → Stop Bit → Idle

---

# 7. Specifications

| Parameter | Value |
|-----------|-------|
| Clock Frequency | 50 MHz |
| Baud Rate | 9600 bps |
| Data Bits | 8 |
| Start Bit | 1 |
| Stop Bit | 1 |
| Parity | None |
| Communication | Full Duplex |

---

# 8. Functional Requirements

The UART shall:

- Transmit 8-bit data
- Receive 8-bit data
- Generate Start Bit
- Generate Stop Bit
- Assert Busy Signal
- Assert Ready Signal

---

# 9. Modules

- Baud Rate Generator
- UART Transmitter
- UART Receiver
- UART Top Module
- Testbench

---

# 10. Future Enhancements

- Programmable Baud Rate
- FIFO Buffers
- UART 16550
- APB Interface
- Interrupt Support
