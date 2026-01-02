# Automotive CAN-Based Car Dashboard System (PIC18F4580)

Multi-ECU automotive dashboard system implemented on the PIC18F4580 using the CAN bus (11-bit standard IDs). This bare-metal Embedded C project demonstrates exchanging vehicle data (RPM, Speed, Gear, Indicators) across ECUs and rendering the final output on a single dashboard board (CLCD + LEDs).

Repository: Anu-raj-dot/-Car-Dashboard-System  
Description: CAN-based automotive dashboard using PIC18F4580 and Embedded C, implementing bare-metal drivers and real-time CAN communication to display speed, RPM, indicator and gear data on CLCD.

---

## Table of Contents
- Project Overview
- Features
- Hardware & Peripherals
- CAN Bus: IDs
- Build & Flash (MPLAB X + TinyBootloader)
- Running & Testing
- Troubleshooting
- Possible Improvements / Future Work
- License
- Contact

---

## Project Overview
This project implements a small automotive network with multiple ECUs communicating over a CAN bus. Individual ECUs read local inputs (potentiometers for analog speed/RPM, switches for gear/indicators), package the data into CAN frames, and transmit them. A dashboard ECU receives frames, unpacks values, and updates a character LCD (CLCD) and LEDs to show speed, RPM, current gear, and turn indicators.

Primary learning goals:
- Setup and use of on-chip CAN peripheral (11-bit IDs)
- Implementing bare-metal drivers (ADC, GPIO, Timers, CLCD)
- Designing deterministic, low-latency message handling and display updates
- Inter-ECU testing (loopback and wired CAN network)

---

## Features
- Multi-ECU architecture (sensor ECUs → dashboard ECU)
- ADC-based reading for speed and RPM (potentiometer as test input)
- Switch-based gear and turn-indicator input
- CAN packing/unpacking with filters & masks
- CLCD display and LED indicators for output
- Bare-metal firmware (no RTOS) written in Embedded C for PIC18F4580
- Configurable update rates and basic debouncing

---

## Hardware & Peripherals
- MCU: PIC18F4580
- Display: Character LCD (CLCD) — e.g., 16x2 or 20x4
- Inputs:
  - Potentiometers as analog sources for Speed and RPM (ADC)
  - Mechanical switches for Gear selection and Turn Indicators
- Output: CLCD + LEDs for indicators
- Tools:
  - MPLAB X IDE
  - XC8 Compiler
  - TinyBootloader (for bootloader-based flashing) or PICkit/ICD tool for programming

---

## CAN Bus: IDs
This project uses standard 11-bit CAN IDs. The example mapping used in the firmware:

- 0x02 — Speed ECU → Data: speed (scaled)
- 0x00 — RPM ECU   → Data: rpm (scaled)
- 0x01 & 0x03 — Gear/Indicators ECU → Data: gear, left/right indicator states
- Dashboard ECU listens to those IDs and updates the display.

---

## Build & Flash (MPLAB X + XC8)
1. Prerequisites:
   - MPLAB X IDE (supported version)
   - XC8 Compiler (matching the project settings)
   - TinyBootloader tools or PICkit/ICD programmer

2. Build:
   - Open the MPLAB X project in this repo.
   - Select the correct device (PIC18F4580) and the active configuration.
   - Ensure the XC8 toolchain path is set in MPLAB X.
   - Build (Project → Clean and Build Project). A .hex file will be generated in the `dist/` folder.

3. Flash using TinyBootloader:
   - If using the bootloader included in the project, put the device in bootloader mode (per bootloader instructions).
   - Use the TinyBootloader GUI/CLI to upload the generated .hex file.
   - Alternatively, use a PICkit/ICD programmer (Hardware tool → Program Target in MPLAB X).

4. Configuration bits:
   - The project contains configuration bit settings in code. Verify oscillator, watchdog timer, brown-out reset, and code-protect settings before flashing.

---

## Running & Testing
1. Basic single-board (loopback) test:
   - Some code supports loopback mode (CAN internal test) — enable to verify packaging/unpacking without external transceiver.
   - Observe CLCD updates when rotating potentiometers and toggling switches.

2. Multi-ECU wired test:
   - Connect nodes using twisted pair CAN_H and CAN_L lines.
   - Verify CAN traffic with a CLCD updates.

3. Debugging tips:
   - Add serial/logging (if available) or use status LEDs to indicate CAN RX/TX and error states.
   - If bus errors occur, check transceiver power/ground and termination resistors.
   - Ensure no two ECUs transmit continuously — only send at configured update rates.

---

## Troubleshooting
- No CAN communication:
  - Ensure IDs are not filtered out by the receiver's masks/filters.
- Display flicker:
  - Ensure dashboard updates are rate-limited (double-buffering or update interval).
  - Debounce mechanical switches and avoid heavy work inside ISRs.
- Incorrect scaling:
  - Verify ADC reference voltage and scaling math. Compare raw ADC values against expected ranges.
- Bus collisions:
  - Ensure each ECU uses the CAN peripheral arbitration correctly and that your TX routines wait for TX buffer availability.

---

## Possible Improvements / Future Work
- Move to 29-bit extended IDs for larger networks (if required)
- Add CANopen / J1939-like higher layer protocol
- Add diagnostic and fault codes on the bus
- Implement message timestamps and message counters to detect lost updates
- Use an RTOS for complex dashboards or for prioritizing tasks
- Add PC-side CAN logger/visualizer

---

## License
MIT License — see LICENSE file in this repo (if added).

---

## Contact
Author: Anu-raj-dot  
Project: Automotive CAN-Based Car Dashboard System | PIC18F4580

---

## Live Demo

![ecu0](https://github.com/user-attachments/assets/5adc3298-791d-4c6b-a3b4-c9be91facb64)
![ecu1](https://github.com/user-attachments/assets/fa3bbabf-3e1c-4027-829c-64fdd02bf8b2)
![ecu3](https://github.com/user-attachments/assets/ce57216f-dc98-4ff8-9842-cd32b723659b)


https://github.com/user-attachments/assets/701dd519-daf5-4a0b-ad37-261d9bc9b450

