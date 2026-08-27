[README.md](https://github.com/user-attachments/files/31532535/README.md)
# Simple UART Transmitter (TX Only)

## Project Overview

This project implements a simple **UART (Universal Asynchronous Receiver/Transmitter) Transmitter** using Verilog HDL. The design converts an 8-bit parallel input into a serial output following the UART transmission format:

**Idle → Start Bit → 8 Data Bits → Stop Bit → Idle**

A Finite State Machine (FSM) controls the complete transmission process. This project demonstrates how digital VLSI design concepts such as sequential circuits, registers, counters, and FSMs are used in real-world communication systems.

---

## Objective

The objective of this project is to design and simulate a UART transmitter capable of converting 8-bit parallel data into serial data.

The project demonstrates:

- Parallel-to-serial data conversion
- Finite State Machine (FSM) design
- Sequential circuit design
- Register-based data storage
- Bit-by-bit data transmission
- Control signal generation
- Basic UART frame generation

This project connects **Digital VLSI Design with Communication Engineering**, demonstrating how digital hardware can implement communication protocols.

---

## What is UART?

UART stands for **Universal Asynchronous Receiver/Transmitter**. It is a widely used serial communication method in embedded systems and microcontroller-based applications.

A basic UART transmission frame contains:

1. An idle condition
2. A start bit
3. Eight data bits
4. A stop bit

The transmitter sends data serially through a single output line.

### UART Frame Structure

Idle → Start Bit → D0 D1 D2 D3 D4 D5 D6 D7 → Stop Bit → Idle

The `tx` line remains HIGH when idle, goes LOW for the start bit, transmits the data bits serially, and finally returns HIGH for the stop bit.

---

## Finite State Machine

The UART transmitter is controlled by a four-state FSM.

| State | Encoding | Function |
|---|---|---|
| IDLE | `00` | Waits for the `start` signal |
| START | `01` | Transmits the start bit (`0`) |
| DATA | `10` | Transmits the 8 data bits |
| STOP | `11` | Transmits the stop bit (`1`) |

The state transition sequence is:

**IDLE → START → DATA → STOP → IDLE**

When `start` is asserted, the FSM leaves the IDLE state and begins transmission. After all eight data bits are sent, the FSM enters the STOP state and finally returns to IDLE.

---

## Inputs and Outputs

### Inputs

| Signal | Description |
|---|---|
| `clk` | System clock |
| `reset` | Resets the transmitter |
| `start` | Initiates transmission |
| `data_in[7:0]` | 8-bit parallel input data |

### Outputs

| Signal | Description |
|---|---|
| `tx` | Serial UART output |
| `busy` | Indicates that transmission is active |

---

## Internal Design

### Data Register

The input data is stored internally before transmission begins. This ensures that the data remains stable while the transmitter sends one bit at a time.

### Bit Counter

A bit counter keeps track of the current data bit being transmitted. It progresses from bit `0` to bit `7`, ensuring that all eight bits are transmitted sequentially.

### State Register

The state register stores the current FSM state and controls the sequence of the UART transmission.

### Busy Signal

The `busy` signal becomes HIGH when the transmitter is actively sending a UART frame and becomes LOW after the transmission is complete.

---

## Simulation and Waveform Analysis

The design was simulated using **Icarus Verilog and EPWave**.

The simulation waveform demonstrates the complete UART transmission process and verifies the correct operation of the FSM, serial output, bit counter, and busy signal.

### Reset Operation

At the beginning of the simulation, `reset` is asserted. The transmitter returns to its initial IDLE state.

During reset:

- The transmitter is in the IDLE state.
- `tx` is HIGH (`1`).
- `busy` is LOW (`0`).

This represents the normal idle condition of a UART transmitter.

### Start Signal and Start Bit

After reset is released, the `start` signal is asserted to initiate transmission.

The waveform shows the FSM moving from the **IDLE state to the START state**. At this point, the `busy` signal becomes HIGH, indicating that a transmission is in progress.

During the START state, the `tx` output goes LOW (`0`). This LOW level represents the UART start bit and marks the beginning of the serial communication frame.

### Data Transmission

After the start bit, the FSM enters the DATA state.

The `bit_index` signal progresses from `0` to `7`, demonstrating that the transmitter processes all eight bits of the input data sequentially.

The `tx` waveform changes according to the stored input data, converting the parallel input into a serial bit stream. Each bit is transmitted during its corresponding transmission interval.

This is the core function of the UART transmitter: **parallel data is converted into serial data one bit at a time**.

### Stop Bit

After the eighth data bit has been transmitted, the FSM enters the STOP state.

The `tx` output returns HIGH (`1`), producing the UART stop bit. The stop bit marks the end of the UART data frame.

### Return to Idle

After transmitting the stop bit, the FSM returns to the IDLE state.

The waveform confirms that:

- `tx` remains HIGH.
- `busy` returns LOW.
- The FSM returns to the IDLE state.

This confirms that one complete UART frame has been successfully transmitted.

---

## Waveform Inference

The EPWave simulation verifies the UART transmission sequence in the following order:

**RESET → IDLE → START SIGNAL → START BIT → DATA BITS 0–7 → STOP BIT → IDLE**

The waveform can be interpreted as follows:

1. **Reset:** The transmitter is initialized and remains inactive.
2. **Idle:** `tx = 1` and `busy = 0`, indicating that no transmission is taking place.
3. **Start:** When the `start` signal is asserted, the FSM begins a new transmission and `busy` becomes HIGH.
4. **Start Bit:** `tx` becomes LOW (`0`) to indicate the beginning of the UART frame.
5. **Data State:** The `bit_index` progresses from `0` to `7`, while `tx` transmits the corresponding bits of the stored input data serially.
6. **Stop Bit:** After all eight data bits are transmitted, `tx` returns HIGH (`1`).
7. **Completion:** The FSM returns to IDLE and `busy` becomes LOW, confirming that the transmission is complete.

The progression of the `state` and `bit_index` signals confirms that the FSM controls the UART transmission correctly. The `busy` signal remains active throughout the transmission and is cleared only after the complete frame has been sent.

Therefore, the waveform successfully verifies the operation of the UART transmitter and confirms correct **parallel-to-serial data transmission**.

---

## Tools Used

- **Verilog HDL** — Hardware description and digital design
- **EDA Playground** — Online simulation environment
- **Icarus Verilog** — Verilog compiler and simulator
- **EPWave** — Waveform visualization and analysis

---

## EDA Playground Simulation

The complete project simulation and waveform can be accessed here:

https://edaplayground.com/x/v385

---

## Concepts Demonstrated

- Verilog HDL
- Finite State Machines
- Sequential digital circuits
- State encoding
- Registers
- Counters
- Parallel-to-serial conversion
- Serial communication
- UART protocol fundamentals
- Digital timing and control

---

## Real-World Applications

UART communication is widely used in:

- Microcontroller communication
- Embedded systems
- GPS modules
- Bluetooth modules
- Serial debugging interfaces
- Sensor modules
- Computer peripheral communication

The same fundamental concepts demonstrated in this project are used in practical digital communication hardware.

---

## Conclusion

A simple UART transmitter was successfully designed and simulated using Verilog HDL. The FSM correctly controlled the transmission sequence from the idle state through the start bit, eight data bits, and stop bit.

The simulation waveform confirms the correct progression of states, bit indexing, serial output, and busy control. The design successfully performs parallel-to-serial transmission and demonstrates how fundamental VLSI digital design concepts can be used to implement a real-world communication protocol.

This project serves as a practical bridge between **Digital VLSI Design and Communication Engineering**.
