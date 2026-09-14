---
title: "Custom Arduino-Controlled NanoLeaf LED Installation"
date: 2021-01-01
tags: ["Electronics", "Arduino", "LED", "Embedded", "Fabrication"]
summary: "Built an interactive wall installation featuring 280 individually addressable NeoPixels in handcrafted wooden hexagonal frames. Programmed custom Arduino C++ firmware with hardware interrupt mode-switching, analog potentiometer brightness mapping, and multi-point 5V power injection."
cover:
  image: "images/projects/nanoleaf/thumbnail.png"
  alt: "Full view of illuminated custom hexagonal NanoLeaf LED array"
  hiddenInSingle: false
weight: 16
---

Over the winter break of 2020-2021, this interactive lighting installation was engineered as a self-directed bridge from pure theoretical software (C++, Python, and MATLAB coursework) into physical embedded hardware and mechatronic systems. The installation features 280 individually addressable WS2812B NeoPixels housed within precision-cut wooden hexagonal frames, allowing operators to dynamically control illumination profiles and lighting patterns via integrated analog and digital inputs.

<div class="project-figure">
  <img src="/images/projects/nanoleaf/nanoleaf-array.jpg" alt="Custom hexagonal NanoLeaf LED array mounted and fully illuminated" />
  <p class="project-caption">Fully illuminated 280-NeoPixel modular hexagonal array mounted with custom matte-black stained wood frames and acrylic diffusion panels.</p>
</div>

## Technical Specifications & Hardware Telemetry

| Subsystem | Specification | Technical Description |
| :--- | :--- | :--- |
| **Optical Array** | 280 WS2812B NeoPixels | Individually addressable RGB diodes mapped across interlocking hexagons |
| **Controller** | Arduino (ATmega328P) | 16 MHz 8-bit MCU running bare-metal C++ control routines |
| **Input Controls** | Hardware Interrupt + ADC | Instantaneous push-button pattern cycle (`attachInterrupt`) & potentiometer brightness |
| **Power Bus** | Regulated 5V DC Rail | Multi-point power injection to prevent diode starvation and voltage decay |

---

## Enclosure Fabrication & Modular Prototyping

Before committing to the full multi-hexagonal wall installation, a rigorous two-hexagon proof-of-concept was constructed to validate circuit topology, signal transmission, and mechanical assembly tolerances. 

The physical enclosure fabrication required meticulous woodworking. Each hexagonal frame was hand-cut with precise 60-degree miters to ensure tight, seamless geometric interlocking. After assembly, the wooden frames were finished with a deep matte-black wood stain, creating high visual contrast against the illuminated acrylic panels when powered on while maintaining a sleek, professional appearance when powered off. Laser-cut and shop-adapted acrylic diffusion panels were fitted directly over the LED chambers to homogenize point-source diode emission into a uniform, ambient glow.

---

## Embedded Firmware & Real-Time Control

The firmware was developed from scratch in C++ for the ATmega328P microcontroller, prioritizing execution efficiency and zero-latency user interaction:

* **Hardware Interrupt Mode Switching:** Attached an external push-button interrupt to digital pin 2 (`attachInterrupt`). This enables instantaneous pattern switching upon user actuation without polling overhead, ensuring the microcontroller never misses a trigger during intensive animation loops.
* **Non-Blocking Timing Architecture:** Replaced blocking `delay()` functions with elapsed-time tracking via `millis()` counters. This allows background pattern rendering and sensor polling to execute concurrently and deterministically.
* **Non-Linear Brightness Mapping:** Integrated an analog potentiometer connected to an ADC channel, translating raw voltage readings through a non-linear exponential curve mapping to match the human eye's logarithmic luminous sensitivity.

---

## Chromatic Lighting Profiles & Power Distribution

Managing 280 WS2812B NeoPixels operating at maximum white brightness demanded careful power budget analysis. At peak load, each RGB diode draws approximately 60mA, resulting in a theoretical maximum current draw of roughly 16.8A ($280 \times 0.06\text{A}$). Feeding this current from a single point would introduce severe trace resistance, causing voltage drop and a noticeable reddish-orange color shift at the tail end of the strip due to insufficient forward voltage on the blue and green semiconductor junctions.

To guarantee uniform color balance and prevent voltage sag, parallel 5V power injection taps were distributed across intermediate nodes in the array.

<div class="project-compare-grid">
  <div class="project-compare-item">
    <span class="project-compare-label">Static Ambient Mode</span>
    <img src="/images/projects/nanoleaf/pink.jpg" alt="Static ambient lighting profile displaying uniform pink hues" />
  </div>
  <div class="project-compare-item">
    <span class="project-compare-label">Dynamic Rainbow Gradient</span>
    <img src="/images/projects/nanoleaf/rainbow.jpg" alt="Dynamic rainbow gradient running across the hexagonal array" />
  </div>
</div>

---

## Interactive Video Demonstration

The responsiveness of the hardware interrupts and the smoothness of the color transitions are demonstrated in the operational video feeds below:

<div class="project-video-grid">
  <div class="project-video">
    <iframe src="https://www.youtube.com/embed/PmaV1rqOmeM" title="Pattern cycling and push-button interrupt response" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
  </div>
  <div class="project-video">
    <iframe src="https://www.youtube.com/embed/yTLSK1VgtIE" title="Smooth brightness adjustment and color transitions" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
  </div>
</div>

---

## Retrospective & Engineering Impact

Building the NanoLeaf installation marked a decisive turning point in my engineering trajectory. Successfully merging low-level C++ firmware with physical woodworking, circuit design, and electrical power budgeting bridged the gap between academic programming and hardware realization. This hands-on confidence directly catalyzed my pursuit of advanced coursework in Mechatronics, IoT systems, and autonomous robotics.
