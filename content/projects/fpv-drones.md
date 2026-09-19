---
title: "Custom FPV Racing Drones"
date: 2020-06-01
tags: ["Robotics", "Electronics", "Fabrication", "Embedded", "CAD"]
summary: "Designed, built, and tuned custom high-performance FPV quadcopters since 2016, developing hands-on mastery in precision micro-soldering, flight controller configuration, PID loop tuning, and custom CAD airframe components."
cover:
  image: "images/projects/fpv-drones/thumbnail.jpg"
  alt: "Custom built high-performance FPV freestyle quadcopter"
  hiddenInSingle: false
weight: 12
---

Building and flying FPV drones in 2016 was my entry point into hardware engineering. What started as a fun way to get dynamic camera angles of my friends biking quickly turned into a crash course in circuit integration, soldering, and troubleshooting. It’s what ultimately pushed me toward robotics.

---

## Electronics & Integration

Packing a flight controller, a 4-in-1 ESC, a 5.8 GHz VTX, and a receiver into a tight carbon fiber frame leaves almost zero room for error in wire management. Because the power system has to handle transient spikes well over 120A during aggressive punch-outs, clean soldering and proper capacitance are mandatory. I rely on low-ESR electrolytic capacitors at the battery leads to clamp inductive voltage spikes from the motors and keep the electrical noise from saturating the IMU gyros. I also spend a lot of time evaluating and tuning different ESC firmwares, like BLHeli_32, to optimize the motor startup signals and overall flight performance.

<div class="project-compare-grid">
  <div class="project-compare-item">
    <span class="project-compare-label">Electronics Stack</span>
    <img src="/images/projects/fpv-drones/drone-internals.jpg" alt="Internal electronics stack showing flight controller, ESC, and VTX wiring" />
  </div>
  <div class="project-compare-item">
    <span class="project-compare-label">Front Airframe Profile</span>
    <img src="/images/projects/fpv-drones/drone-front.jpg" alt="Front airframe profile showing camera mount and TPU bumper" />
  </div>
</div>

---

## Mechanical Design & Custom 3D Printing

A freestyle drone spends a lot of time crashing into concrete and trees. To protect the carbon fiber frame and electronics, I CAD and 3D print custom TPU bumpers, camera mounts, and antenna guides to absorb the shock of high-velocity impacts. Beyond impact protection, mechanical isolation is critical for flight performance. Soft-mounting the flight controller with silicone grommets filters out the high-frequency motor harmonics before they reach the IMU, feeding much cleaner gyro data into the stabilization loop.

---

## Diagnostics & Flight Tuning

Crashing means fixing things in the field...tracking down electrical shorts, ground loops, or RF interference on the bench or in the dirt. But getting the hardware running is only half the battle. Getting a drone to fly well requires iterative PID tuning, feedforward adjustments, and dynamic notch filtering in Betaflight. Dialing in these parameters eliminates prop-wash oscillation and cuts down the latency, making the drone respond instantly to stick inputs.

---

## The 2020 Build

The photos here highlight a custom freestyle quad I built during the 2020 lockdown, designed specifically to take a beating. The hands-on experience of sourcing parts, soldering under a microscope, managing ESC hardware, and debugging firmware laid the practical foundation for the embedded systems and hardware projects I build today.