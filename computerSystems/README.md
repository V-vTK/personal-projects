# TKJ Final Project — Tamagotchi Virtual Pet

A final project for the **Computer Systems (TKJ)** course at the **University of Oulu**. An interactive virtual pet (Tamagotchi) running on a **TI CC2650 SensorTag**, using multiple sensors and actuators to create a responsive creature that reacts to its environment.

## Background & Motivation

This project was the final assignment for the Computer Systems course, where we built an embedded system running on **TI-RTOS** (Texas Instruments Real-Time Operating System) for the **CC2650 SensorTag** platform. The goal was to create an interactive Tamagotchi-like virtual pet that responds to real-world sensor data — temperature, light, motion, and air pressure — and communicates its state through audio (buzzer), visual (LEDs), and a UART-connected backend.

The device collects data from **4 different sensors** and responds to **physical button input and gyro**, creating a feedback loop between the physical environment and the virtual pet's behaviour.

## Hardware Platform

The project runs on a **TI CC2650 SensorTag**

### Sensors

| Sensor | Interface | What It Measures | Purpose |
|--------|-----------|-----------------|---------|
| **MPU9250** | I2C (dedicated bus) | 3-axis acceleration + 3-axis gyroscope | Movement/exercise detection, orientation validation for feeding, rotate-to-pet gesture |
| **TMP007** | I2C | Infrared temperature | Comfort state messages ("I'm cold!", "The temperature feels comfortable.", "It's too hot!") |
| **OPT3001** | I2C | Ambient light level | Sleep/wake state machine (lux thresholds for brightness, darkness, sleep) |
| **BMP280** | I2C | Air pressure | Detects atmospheric pressure changes to trigger petting |

### Actuators

| Actuator | Type | Purpose |
|----------|------|---------|
| **Buzzer** | PWM-based | Plays melodies — sad beep, happy beep, exercise beep, eat beep, lullaby, Imperial March |
| **Red LED** | GPIO | One-second flash for negative events (backend BEEP message) |
| **Green LED** | GPIO | One-second flash for positive events (PET/EXERCISE/EAT increment) |

### Input

| Input | Action |
|-------|--------|
| **Button 0 (Left)** | Feed the Tamagotchi — must be followed by tilting the device to validate |
| **Button 1 (Right)** | Power off — enters shutdown mode, wakes on next press |

## Architecture

The software is structured as a **TI-RTOS** application with two concurrent tasks:

### Task Structure

| Task | Stack | Priority | Responsibilities |
|------|-------|----------|-----------------|
| **`mainTask`** (sensor) | 2048 bytes | 2 | I2C sensor reads, state machine, sound effects, button callbacks |
| **`uart_task_fxn`** | 2048 bytes | 2 | UART communication to backend server, message queuing |

### Task Scheduling

The `mainTask` uses a **function-pointer-based scheduler** — not separate OS-level context switching. A `struct Task` holds an `interval` (in ticks) and a function pointer `void (*handler)()`. The main loop iterates over an array of these structs and calls each handler when `ticks % interval == 0`. This is a lightweight cooperative scheduler running entirely within a single TI-RTOS task.

| Task | Interval (ticks) | Interval (real time) |
|------|-----------------|---------------------|
| `pre_action_task` | 1 | 50ms — audio/visual feedback queued from callbacks |
| `read_MPU` | 1 | 50ms — accelerometer + gyroscope data |
| `read_TMP` | 20 | 1s — temperature sensor |
| `read_OPT` | 20 | 1s — ambient light sensor |
| `read_BMP` | 5 | 250ms — pressure sensor |

### State Machine

The Tamagotchi transitions between states based on sensor readings:

```
                    ┌─────────────────────────────┐
                    │   Awake, comfortable temp    │
                    │   (temp 25-34°C, lux < 1000) │
                    └──────────┬──────────────────┘
                               │
              ┌────────────────┼────────────────┐
              ▼                ▼                ▼
     ┌──────────────┐  ┌──────────────┐  ┌──────────────┐
     │ "I'm cold!"  │  │ "Comfortable"│  │ "Too hot!"   │
     │  temp < 25°C │  │  25-34°C     │  │  temp > 34°C │
     └──────────────┘  └──────────────┘  └──────────────┘
              │                │                │
              └────────────────┼────────────────┘
                               │
                    ┌──────────┴──────────┐
                    │  lux < 15 AND       │
                    │  sustained for 5s   │
                    └──────────┬──────────┘
                               ▼
                    ┌──────────────────────┐
                    │     Sleeping (ZZZ)   │
                    │  Plays lullaby on    │
                    │  transition, sends   │
                    │  PET:3 to backend    │
                    └──────────────────────┘
```

### Communication Protocol

The device communicates with a backend server over **UART at 9600 baud** (8N1). Messages are formatted as plaintext:

| Message | Format | Trigger |
|---------|--------|---------|
| EAT | `id:3042,EAT:1` | Button press + orientation validation |
| EXERCISE | `id:3042,EXERCISE:1` | Movement magnitude > threshold over 20 ticks |
| PET | `id:3042,PET:N` | Rotate gesture (+3), pressure change (+1), sleep start (+3) |
| State MSG | `id:3042,MSG1:...,MSG2:...` | Temperature + sleep state updates |

The device also listens for incoming `BEEP` commands. When it receives the pattern `3042,BEEP:Too late`, it triggers the **Imperial March** melody via the buzzer — indicating the Tamagotchi has run away.

## Key Features

### Feeding
Pressing Button 0 sets an `ate` flag. The device then expects the user to tilt it (detected via MPU9250 accelerometer X-axis < -0.9). If the tilt is detected, an `EAT:1` message is sent and the eat beep plays.

### Exercise Detection
The MPU accelerometer data is accumulated over 20 ticks (1 second). The root-sum-square of the average acceleration in each axis is calculated. If the total exceeds 1.0 m/s², the device triggers an exercise event with a corresponding beep.

### Sleep/Wake Cycle
The OPT3001 light sensor readings are evaluated every second. If lux < 15 for 5 consecutive seconds, the Tamagotchi goes to sleep — a lullaby plays, the backend is notified with `PET:3`, and the state changes to `"zzzZZZzzzZZzZZzzz"`. When light levels rise again, the pet wakes up.

### Rotate-to-Pet
When the device is rotated (Z-axis acceleration < -0.9 combined with significant gyroscope Z movement), the pet is petted using the history of past movements. This triggers a happy beep and increments care by 3.

### Pressure-Based Petting
BMP280 pressure readings are compared against a running average. A pressure spike > 0.5% above the average increments the pet counter, simulating the pet reacting to changes in atmospheric pressure.

### Musical Feedback

| Melody | Notes | When |
|--------|-------|------|
| **Sad beep** | G4 → E4 → C4 | Backend sends BEEP command |
| **Happy beep** | C6 → E6 → G6 | Rotate-to-pet gesture detected |
| **Exercise beep** | E6 → D#6 → E6 | Exercise threshold reached |
| **Eat beep** | E6 → F#6 → A6 | Feeding validated |
| **Lullaby** | Multiple notes | Going to sleep |
| **Imperial March** | Multiple notes | Running away (BEEP:Too late) |

## Technologies Used

- **Platform:** TI CC2650 SensorTag (ARM Cortex-M3)
- **RTOS:** TI-RTOS (TI's real-time operating system)
- **IDE/Toolchain:** TI Code Composer Studio with XDCtools
- **Sensors:** MPU9250, TMP007, OPT3001, BMP280 — all via I2C
- **Communication:** UART (9600 baud, 8N1)
- **Peripherals:** PWM buzzer, GPIO LEDs, GPIO buttons
- **Language:** C with TI-RTOS and DriverLib APIs

## Data Collection

Sensor data was collected during testing and stored in an Excel spreadsheet for analysis and validation of the state machine behaviour.

## Key Takeaways

- Gained hands-on experience with **embedded real-time systems** using TI-RTOS with concurrent tasks and tick-based scheduling.
- Implemented a **multi-sensor state machine** integrating temperature, light, motion, and pressure data.
- Developed **I2C communication** with 4 different sensor ICs, including a dedicated I2C bus for the MPU9250.
- Built a **PWM-based audio system** capable of playing multi-note melodies through a piezo buzzer.
- Implemented **UART communication** for bidirectional data exchange between the embedded device and a backend server.
- Designed a **gesture recognition system** using accelerometer and gyroscope data to detect rotation, tilt, and exercise.
- Practiced **power management** with a shutdown/wake button using GPIO interrupts and `PowerCC26XX`.
- Used **TI DriverLib** for low-level timer (GPT0) configuration for PWM output.

## Status

- **Completed:** Yes (course project finished and submitted)
- **Maintained:** No (archive — coursework reference)
- **Notes:** This project ran on real SensorTag hardware and was tested with live sensor data. The backend server was provided by the course organisers.

## My Contributions
>
> *All three group members worked together on the same PC in the lab — coding, testing, planning, and data collection were done collaboratively*
> 

## Links

- Uses [TI CC2650 SensorTag](https://www.ti.com/tool/CC2650STK)
- Uses [TI-RTOS](https://www.ti.com/tool/TI-RTOS)
- Uses [TI DriverLib](https://www.ti.com/tool/SW-TM4C-DRL)
- Course: Computer Systems (TKJ), University of Oulu

## Further Notes

This document was created by giving an AI access to the source code. The report was then edited and verified.