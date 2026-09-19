---
title: "IoT Black & White Image Scanner"
date: 2022-12-01
tags: ["Robotics", "IoT", "Embedded", "Python", "CAD", "SolidWorks"]
summary: "Electromechanical scanner that digitizes physical images using an LED and photoresistor, powered by a Raspberry Pi and controlled wirelessly via a custom web interface. Designed the complete 2-axis X-Y gantry, dual-actuator motion system, and hardware integration as the mechanical lead."
cover:
  image: "images/projects/iot-bw-image-scanner/thumbnail.jpg"
  alt: "Full assembly of the IoT electromechanical image scanner"
  hiddenInSingle: false
draft: true
build:
  render: never
  list: never
weight: 14
---

Developed as a capstone project for an Internet of Things (IoT) course, this electromechanical system digitizes physical images using only an LED and a photoresistor. Powered by a Raspberry Pi and controlled entirely wirelessly via a custom web interface, the project required the seamless integration of custom kinematics, sensor data acquisition, and full-stack web communication. Working in a three-person team, I served as the mechanical and hardware lead.

<div class="project-arch-grid">
  <div class="project-arch-card">
    <div class="project-arch-core">Role: Mechanical & Hardware Lead</div>
    <div class="project-arch-title">Electromechanical Subsystems (My Contribution)</div>
    <ul class="project-arch-list">
      <li>Designed complete 2-axis X-Y gantry from scratch in SolidWorks</li>
      <li>Rapid-prototyped modular PLA components with standard M3 fasteners</li>
      <li>Dual-actuator motion: Stepper motor (Y-indexing) + Servo sweep (X-axis)</li>
      <li>Wired Raspberry Pi GPIO and wrote OOP Python hardware validation scripts</li>
    </ul>
  </div>
  <div class="project-arch-card">
    <div class="project-arch-core">System Architecture</div>
    <div class="project-arch-title">Software & Full-Stack Pipeline</div>
    <ul class="project-arch-list">
      <li>Custom browser-based GUI for scan triggering and dimension input</li>
      <li>CGI and Python background scripts for asynchronous execution</li>
      <li>Analog reflectance normalization mapping voltages to greyscale pixels</li>
      <li>Autonomous 2D matrix reconstruction into downloadable JPEG images</li>
    </ul>
  </div>
</div>

---

## Mechanical Engineering: From Digital CAD to Physical Gantry

The physical system was engineered from the ground up to achieve reliable, repeatable 2-axis motion within strict project constraints. As mechanical and hardware lead, I focused on designing modular structural subassemblies and integrating the dual-actuator motion drive.

Using SolidWorks, I modeled the complete X-Y gantry, performing spatial and interference checks across all moving joints before committing to fabrication. The structural components and drive tracks were additively manufactured using FDM 3D printing in PLA, balancing low mass with structural rigidity. The entire assembly was designed for modularity, utilizing standard M3 threaded inserts and fasteners for rapid assembly, maintenance, and teardown.

The motion architecture couples a stepper motor and precision gear assembly for vertical (Y-axis) line indexing with a smooth servo motor sweep for horizontal (X-axis) scanning. Electronic hardware was systematically integrated with the Raspberry Pi GPIO header, backed by custom object-oriented Python test scripts to validate sensor telemetry, motor stepping resolution, and circuit continuity during prototyping.

<div class="project-compare-grid">
  <div class="project-compare-item">
    <span class="project-compare-label">SolidWorks Digital Assembly</span>
    <img src="/images/projects/iot-bw-image-scanner/cad-assembly.jpg" alt="SolidWorks digital CAD assembly of the 2-axis X-Y gantry" />
  </div>
  <div class="project-compare-item">
    <span class="project-compare-label">FDM Additive Fabrication</span>
    <img src="/images/projects/iot-bw-image-scanner/printing-parts.jpg" alt="3D printed modular PLA structural components" />
  </div>
</div>

<div class="project-figure">
  <img src="/images/projects/iot-bw-image-scanner/full-assembly.jpg" alt="Complete 2-axis electromechanical scanner assembly" />
  <p class="project-caption">Fully integrated electromechanical scanner, showcasing the modular PLA gantry, dual-actuator drive, and Raspberry Pi controller.</p>
</div>

---

## Optical Sensing & Discrete Reflectance Physics

At the heart of the digitizer is a minimalist yet highly effective optical sensing payload positioned millimeters above the scan bed. A high-brightness LED continuously illuminates a localized spot on the paper surface, while an adjacent photoresistor (light-dependent resistor, LDR) measures the reflected light intensity.

<div class="project-figure">
  <img src="/images/projects/iot-bw-image-scanner/scanning-tool-closeup.jpg" alt="Close-up of the optical sensor head with LED and photoresistor" />
  <p class="project-caption">Close-up of the sensor payload: high-brightness LED emitter paired with a light-dependent resistor (LDR) positioned millimeters above the scan bed.</p>
</div>

This analog reflectance mechanism exploits the optical contrast between white paper and dark toner or ink. Unprinted paper reflects high levels of light, lowering the photoresistor resistance, whereas inked areas absorb light, increasing resistance. The continuous analog voltage variations across the LDR are sampled via an analog-to-digital converter interface, establishing spatial coordinate indexing that translates physical reflectance gradients into a normalized greyscale pixel matrix.

---

## Web Interface & Image Synthesis

Remote operation and post-processing are handled through an intuitive web-based architecture.

<div class="project-figure">
  <img src="/images/projects/iot-bw-image-scanner/webpage.jpg" alt="Custom web GUI for remote scan configuration and download" />
  <p class="project-caption">Browser interface allowing remote configuration of scan bounds, real-time progress monitoring, and JPEG export.</p>
</div>

Teammates developed a responsive HTML browser interface paired with background Python and CGI scripts. Operators can configure custom scan boundaries, initiate remote scanning sequences, and monitor progress in real-time. Once a scan concludes, the backend software parses the raw voltage data, applies normalization algorithms, and reconstructs the 2D matrix into a downloadable JPEG image.

<div class="project-compare-grid">
  <div class="project-compare-item">
    <span class="project-compare-label">Text Scan: HI WRLD</span>
    <img src="/images/projects/iot-bw-image-scanner/hi-wrld.jpg" alt="Synthesized text scan output displaying HI WRLD" />
  </div>
  <div class="project-compare-item">
    <span class="project-compare-label">Image Scan: Smiley Face</span>
    <img src="/images/projects/iot-bw-image-scanner/smiley.jpg" alt="Synthesized image scan output displaying a smiley face" />
  </div>
</div>

---

## Hardware Demonstration

To evaluate mechanical tracking accuracy and end-to-end system reliability, rigorous test sequences were recorded. The following demonstrations illustrate individual gantry motion testing as well as the full automated scanning cycle from web trigger to image export.

<div class="project-video-grid">
  <div class="project-video">
    <iframe src="https://www.youtube.com/embed/NDIJYcaRBZA" title="IoT Scanner — Gantry motion test" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
  </div>
  <div class="project-video">
    <iframe src="https://www.youtube.com/embed/9Je25YHhv7c" title="IoT Scanner — Full automated scan sequence" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
  </div>
</div>
