---
title: "Mechatronics Capstone: Interactive Putting Game"
date: 2022-04-01
tags: ["Embedded", "Arduino", "Software", "Mechatronics", "Processing", "C++"]
summary: "Arcade-style interactive golf putting game developed as a junior-year Mechatronics capstone. Bridges physical hardware sensors and Arduino firmware with a custom Processing graphical interface for real-time telemetry, accuracy analytics, and competitive time-trial gameplay."
weight: 18
---

## Overview

As the junior-year Mechatronics capstone challenge, this project required engineering an arcade-style interactive putting game from the ground up. The core objective was to bridge physical hardware sensors and microcontroller firmware with a responsive desktop graphical user interface, creating a closed-loop interactive experience that combines precise electromechanical sensing with high-speed serial telemetry and competitive time-trial gameplay.

<div class="project-video"><iframe src="https://www.youtube.com/embed/QSfsfaazWGQ" title="Mechatronics Capstone — Interactive Putting Game Technical Presentation" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></div>
*Complete technical capstone presentation detailing circuit design, sensor debouncing, UART serial protocols, and finite state machines.*

## Arcade HUD and Real-Time Telemetry Matrix

The software interface delivers continuous, low-latency performance analytics during active gameplay sessions. The telemetry pipeline maps physical interaction events to real-time graphical updates:

| Gameplay Telemetry | Computation and Update Protocol | Functional Purpose |
| :--- | :--- | :--- |
| **Time-Trial Countdown** | Deterministic 1Hz timer decrement via MCU clock | Governs active round duration; triggers timeout state |
| **Cumulative Score** | Sensor interrupt trigger with 50ms software debounce | Tracks verified ball drops into target cup |
| **Putting Accuracy (%)** | Calculated ratio of made putts over total attempts | Delivers quantitative player performance metrics |
| **All-Time High Score** | Session persistent memory in Processing runtime | Provides arcade-style competitive replay incentive |

## Full-Stack Mechatronic Architecture

<div class="project-arch-grid">
  <div class="project-arch-card">
    <div class="project-arch-core">Hardware and Firmware Layer</div>
    <div class="project-arch-title">Arduino Sensor Acquisition (C++)</div>
    <ul class="project-arch-list">
      <li>Physical sensor array embedded within the putting cup target</li>
      <li>Interrupt-driven detection with software debouncing to reject noise</li>
      <li>Deterministic timing engine managing active round countdowns</li>
      <li>Serial UART packet encoder streaming state telemetry at 9600 baud</li>
    </ul>
  </div>
  <div class="project-arch-card">
    <div class="project-arch-core">Software and Visualization Layer</div>
    <div class="project-arch-title">Processing GUI and Game Engine</div>
    <ul class="project-arch-list">
      <li>Asynchronous serial listener parsing incoming telemetry packets</li>
      <li>Real-time HUD displaying timer, dynamic score, and accuracy metrics</li>
      <li>Multi-state machine governing Idle, Active Play, and Game-Over states</li>
      <li>Visual feedback animations signaling made putts and personal bests</li>
    </ul>
  </div>
</div>

## Circuit Design and Hardware-Software Serial Pipeline

### Electrical Interface and Noise Mitigation
The physical target utilizes an integrated sensor array mounted inside the putting cup. Because mechanical switches and optical beam breaks are susceptible to transient voltage spikes and contact chatter when impacted by a golf ball, robust hardware and firmware debouncing was mandatory. Pull-up resistors stabilize input pins against floating states, while a 50ms software debounce filter on the microcontroller ensures that high-velocity impacts or rolling rebounds register as exactly one verified goal event without double-triggering.

### UART Serial Communication Protocol
Communication between the embedded microcontroller and the host workstation runs over a reliable UART serial link at 9600 baud. The Arduino firmware encodes state changes, goal detections, and timer ticks into structured serial packets. On the host side, the Processing application runs an asynchronous event listener that parses these incoming data streams, updating local game state variables and driving the graphical rendering engine without dropped frames or input lag.

## Technical Insights and Mechatronic Takeaways

Engineering this interactive system highlighted key principles in electromechanical design and human-in-the-loop system integration:
- **Robustness in Physical Computing:** Bridging mechanical hardware with software requires anticipating real-world signal noise, bounce, and inconsistent user interactions through a combination of hardware conditioning and firmware filtering.
- **Asynchronous State Synchronization:** Maintaining seamless parity between embedded countdown timers and graphical HUD elements requires clean packet serialization and non-blocking event loops.
- **Interactive Feedback Loops:** Delivering immediate visual and auditory cues upon successful sensor triggering significantly elevates user engagement, transforming raw sensor data into an intuitive arcade experience.
