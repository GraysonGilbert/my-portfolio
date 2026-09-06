---
title: "Sim2Real Rotary Inverted Pendulum"
date: 2026-05-01
tags: ["Robotics", "Machine Learning", "Embedded", "C++", "Python"]
summary: "Trained a PPO control policy in a custom MuJoCo Gymnasium environment and achieved zero-shot transfer onto an ESP32 microcontroller."
cover:
  image: "images/projects/sim2real.jpg"
  alt: "Sim2Real Rotary Inverted Pendulum hardware and simulation"
  hiddenInSingle: false
weight: 1
---

### Overview
This project focused on bridging reinforcement learning with physical embedded hardware. I designed the CAD models, 3D printed the physical components, and built a custom Gymnasium training environment in MuJoCo.

After training the Proximal Policy Optimization (PPO) policy, I exported the neural network weights to a C++ header file and deployed the inference engine onto a custom ESP32 hardware setup for real-time balancing.