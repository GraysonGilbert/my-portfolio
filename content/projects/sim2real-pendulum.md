---
title: "Sim2Real Transfer: Robust RL Control of a Furuta Pendulum"
date: 2026-05-01
tags: ["Robotics", "Machine Learning", "Embedded", "C++", "Python"]
summary: "Trained a PPO control policy in a custom MuJoCo Gymnasium environment and achieved zero-shot transfer onto an ESP32 microcontroller — balancing continuously for over an hour on physical hardware with no additional tuning."
cover:
  image: "images/projects/sim2real/sim2real.jpg"
  image: "images/projects/sim2real/sim2real-thumbnail.png"
  alt: "Sim2Real Furuta Pendulum — physical hardware and MuJoCo simulation"
  hiddenInSingle: false
  hiddenInSingle: true
weight: 1
---

Bridging the gap between high-fidelity physics simulation and real-world robotic hardware is a notoriously difficult challenge. This project demonstrates the successful **zero-shot sim-to-real transfer** of a deep reinforcement learning policy to control a highly non-linear, 2-DoF underactuated Furuta pendulum — executing swing-up maneuvers and maintaining continuous balance for **over one hour** on physical hardware without a single line of hardware-specific tuning.

<div class="project-stats">
  <div class="project-stat">
    <span class="project-stat-value">2-DoF</span>
    <span class="project-stat-label">Underactuated System</span>
  </div>
  <div class="project-stat">
    <span class="project-stat-value">100 Hz</span>
    <span class="project-stat-label">Embedded Control Loop</span>
  </div>
  <div class="project-stat">
    <span class="project-stat-value">&gt;1 Hour</span>
    <span class="project-stat-label">Continuous Balance</span>
  </div>
  <div class="project-stat">
    <span class="project-stat-value">Zero-Shot</span>
    <span class="project-stat-label">No Hardware Tuning</span>
  </div>
</div>

<div class="project-video-short-wrapper">
  <div class="project-video-short">
    <iframe
      src="https://www.youtube.com/embed/GyXinH8Eepk"
      title="Sim2Real Furuta Pendulum — swing-up demo"
      frameborder="0"
      allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
      allowfullscreen>
    </iframe>
  </div>
</div>

---

## Mechanical Design

The physical system was designed entirely from scratch in Fusion 360 and manufactured via FDM 3D printing. The Furuta pendulum configuration consists of a motorized horizontal arm (rotor) and a free-swinging vertical pendulum link. All mechanical components were designed to be lightweight while maintaining the rigid joint geometry required for accurate dynamic modeling.

<div class="project-figure">
  <img src="/images/projects/sim2real/exploded-assembly.jpg" alt="Exploded CAD assembly view of the Furuta pendulum" />
  <img src="/images/projects/sim2real/exploded-assembly.png" alt="Exploded CAD assembly view of the Furuta pendulum" />
  <p class="project-caption">Exploded assembly view — the full pendulum mechanism with motor mount, rotor arm, and pendulum link.</p>
</div>

---

## Simulation Development & Calibration

A high-fidelity MuJoCo physics simulation was the foundation of the entire project. Getting the sim-to-real transfer to work required the simulation to be **meticulously calibrated** to match the physical system — not just visually, but dynamically.

Two isolated calibration experiments were designed to independently characterize and tune the key physical parameters before any RL training began.

### Pendulum Drop Test

The pendulum link was released from a fixed angle and the free-swing decay was recorded. The simulation's inertia and damping parameters were iteratively adjusted until the simulated drop response matched the physical hardware response.

<div class="project-compare-grid">
  <div class="project-compare-item">
    <span class="project-compare-label">Before Calibration</span>
    <img src="/images/projects/sim2real/initial_pendulum_drop_test.jpg" alt="Initial pendulum drop test — simulation vs hardware mismatch" />
  </div>
  <div class="project-compare-item">
    <span class="project-compare-label">After Calibration</span>
    <img src="/images/projects/sim2real/tuned_pendulum_drop_test.jpg" alt="Tuned pendulum drop test — simulation matches hardware" />
  </div>
</div>

### Rotor Friction Test

The rotor arm was driven at a fixed command and released, isolating the motor's friction and back-EMF characteristics. This independently constrained the motor torque model in the simulation.

<div class="project-compare-grid">
  <div class="project-compare-item">
    <span class="project-compare-label">Before Calibration</span>
    <img src="/images/projects/sim2real/inital_rotor_isolation_test.jpg" alt="Initial rotor isolation test — friction model mismatch" />
  </div>
  <div class="project-compare-item">
    <span class="project-compare-label">After Calibration</span>
    <img src="/images/projects/sim2real/tuned_rotor_isolation_test.jpg" alt="Tuned rotor isolation test — friction model matched" />
  </div>
</div>

---

## Reinforcement Learning


### Training Methodology

A **Proximal Policy Optimization (PPO)** agent was trained in a custom Gymnasium environment wrapping the calibrated MuJoCo simulation. Several techniques were combined to maximize real-world robustness:

**Curriculum Learning** — A multi-phase reward structure progressively taught the agent distinct sub-behaviors in sequence: stabilizing near the top, catching the pendulum from near-upright positions, and finally executing full swing-up maneuvers from rest. Training was gated so the agent could not advance to the next phase until the current one was reliably solved.

**Domain Randomization** — On every episode reset, physical parameters were sampled randomly within calibrated bounds: pendulum mass, motor torque limits, and joint damping coefficients. This forced the policy to generalize across the range of hardware variation rather than overfit to a single nominal model.

**Noise & Latency Injection** — Manual velocity noise was added to all observations, and action latency buffers were used to simulate communication delays between the inference engine and the motor controller — directly mimicking the timing characteristics of the embedded system.

<div class="project-figure">
  <img src="/images/projects/sim2real/tensor_board_training.jpg" alt="TensorBoard training curves — PPO policy reward over training steps" />
  <p class="project-caption">TensorBoard training curves showing episode reward progression across curriculum phases. The distinct reward steps correspond to each phase unlock.</p>
</div>

---

## Hardware Debugging & Observation Logging

After initial deployment, observation logging was added to both the simulation and the physical hardware to diagnose discrepancies between the two environments. Side-by-side comparison of the logged state signals — joint angles, velocities, and motor commands — allowed pinpointing any remaining mismatches in sensor scaling, timing, or coordinate conventions before declaring a clean transfer.

<div class="project-compare-grid">
  <div class="project-compare-item">
    <span class="project-compare-label">Simulation</span>
    <img src="/images/projects/sim2real/simulated_observation_space.jpg" alt="Simulated observation space logging" />
  </div>
  <div class="project-compare-item">
    <span class="project-compare-label">Physical Hardware</span>
    <img src="/images/projects/sim2real/physical_observation_space.jpg" alt="Physical hardware observation space logging" />
  </div>
</div>

---

## Embedded Deployment


### Exporting the Policy

After training, the final PyTorch model weights were exported directly as a **C++ header file** — a flat array of floating-point values representing the neural network. No ONNX runtime, no interpreter, no Python: the entire forward pass was reimplemented in bare C++ on the microcontroller.

### Dual-Core Architecture on ESP32

The ESP32's dual-core architecture was leveraged to strictly isolate compute-intensive inference from real-time motor control:

<div class="project-arch-grid">
  <div class="project-arch-card">
    <div class="project-arch-core">Core 0</div>
    <div class="project-arch-title">RL Inference & State Estimation</div>
    <ul class="project-arch-list">
      <li>Reads encoder positions via SPI at 100 Hz</li>
      <li>Runs neural network forward pass</li>
      <li>Calculates target torque command</li>
    </ul>
  </div>
  <div class="project-arch-card">
    <div class="project-arch-core">Core 1</div>
    <div class="project-arch-title">Real-Time Motor Control</div>
    <ul class="project-arch-list">
      <li>Executes Field Oriented Control (FOC)</li>
      <li>Drives BLDC gimbal motor via SimpleFOC</li>
      <li>Handles current sensing & torque execution</li>
    </ul>
  </div>
</div>

Torque Field Oriented Control was used for actuation, providing precise, linear torque commands rather than velocity or position targets. This gave the RL policy direct, low-latency authority over the motor output, which was essential for the fast dynamics of the swing-up maneuver.

---

## Hardware Debugging & Observation Logging

The first deployment onto physical hardware did not work. The pendulum would immediately diverge rather than swing up and balance. Rather than retraining or adjusting reward functions, observation logging was added to both the simulation and the physical hardware to directly compare the two state streams side by side.

<div class="project-compare-grid">
  <div class="project-compare-item">
    <span class="project-compare-label">Simulation</span>
    <img src="/images/projects/sim2real/simulated_observation_space.jpg" alt="Simulated observation space logging" />
  </div>
  <div class="project-compare-item">
    <span class="project-compare-label">Physical Hardware</span>
    <img src="/images/projects/sim2real/physical_observation_space.jpg" alt="Physical hardware observation space logging" />
  </div>
</div>

The logs immediately revealed the issue: the SPI encoder readings from the physical AS5047P were **sign-inverted** relative to the simulation's coordinate convention. The motor was receiving the correct torque magnitude but driving the pendulum in the wrong direction on every step. A single sign flip in the firmware resolved the issue entirely — no retraining, no reward shaping, no parameter adjustment. The policy worked reliably from that point forward.

---

## Results

The trained policy was deployed onto the physical hardware with no additional tuning of any kind. The system immediately executed swing-up maneuvers and sustained continuous balance.
<div class="project-video">
  <iframe
    src="https://www.youtube.com/embed/OBtJI40cq7E"
    title="Sim2Real Furuta Pendulum — one hour balance timelapse"
    frameborder="0"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
    allowfullscreen>
  </iframe>
</div>

<div class="project-figure">
  <img src="/images/projects/sim2real/timelapse.png" alt="Timelapse of the Furuta pendulum balancing for over one hour" />
  <p class="project-caption">Timelapse of the physical system executing the learned policy — continuous balance maintained for over one hour.</p>
</div>
The trained policy achieved continuous balance for over one hour on the first clean run after the encoder sign fix — with zero modifications to the learned weights. The zero-shot transfer was validated not just by the duration of balance, but by the quality of the swing-up: the policy consistently executed the correct energy-pumping maneuver learned entirely in simulation.