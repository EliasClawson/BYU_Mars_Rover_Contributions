# BYU Mars Rover Contributions

This repository documents my technical contributions to the BYU Mars Rover project, specifically focusing on the design and implementation of long-range communication systems and closed-loop antenna controls.

![Mars Rover Overview](RoverOldWide.jpg)

## Project Overview

As a Communications Engineer for the team, my primary objective was ensuring robust, high-bandwidth signal integrity between the base station and the rover over several miles of varied terrain. This project required a deep integration of hardware design, signal processing, and real-time control logic.

## Key Technical Contributions

### 1. Closed-Loop Antenna Tracking
Developed and implemented a tracking system that utilized real-time **RSSI (Received Signal Strength Indicator)** feedback. This system allowed for automated antenna orientation, ensuring the highest gain was maintained regardless of the rover's relative position or orientation to the base station.

### 2. High-Frequency Signal Sampling
Designed 900 MHz antenna arrays and implemented signal sampling algorithms to monitor link quality. This involved:
* Minimizing packet loss over long-distance telemetry.
* Implementing automated fail-safes for autonomous navigation when signal thresholds dropped.
* Optimizing data throughput for real-time video feeds and sensor telemetry.

### 3. Integrated Control Logic
Collaborated on the embedded logic required to sync rover mobility with communication constraints. I refactored legacy communication protocols to reduce latency by 50%, enabling more responsive remote operation during mission-critical tasks.

## Technical Stack
* **Hardware:** 900 MHz Radio Modules, High-Gain Directional Antennas.
* **Software:** C++, MATLAB (for signal modeling and link margin analysis).
* **Protocols:** Custom RF packet structures, RSSI-based control loops.

## Impact
These contributions directly supported the rover's performance in national competitions, maintaining consistent video transmission from multiple cameras and telemetry stability across high-interference environments.
