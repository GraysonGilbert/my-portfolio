---
title: "Infinity Mirror LED Hyper Cube"
date: 2022-05-01
tags: ["Electronics", "LED", "Fabrication", "3D Printing", "Fusion 360"]
summary: "Engineered and fabricated a custom infinity mirror LED cube from scratch, integrating 300+ individually addressable LEDs, concealed wire routing through structural acrylic tubing, and a Bluetooth smartphone controller — including hardware troubleshooting to resolve matrix voltage drop."
cover:
  image: "images/projects/hyper-cube/progress-picture-1.jpg"
  alt: "Assembly of the custom Infinity Mirror LED Hyper Cube frame"
  hiddenInSingle: false
weight: 13
---

Inspired by commercial optical illusions and infinity displays, I engineered and fabricated this custom LED Hyper Cube entirely from scratch. By combining precision additive manufacturing, one-way mirror film, and over 300 individually addressable WS2812B LEDs, the cube creates a mesmerizing, seemingly infinite tunnel of light in three dimensions. Fully managed via a custom Bluetooth smartphone controller, the project bridges mechanical design constraints, high-density electrical routing, and iterative hardware debugging.

<div class="project-stats">
  <div class="project-stat">
    <span class="project-stat-value">300+</span>
    <span class="project-stat-label">Addressable LEDs</span>
  </div>
  <div class="project-stat">
    <span class="project-stat-value">Bluetooth</span>
    <span class="project-stat-label">App Controlled</span>
  </div>
  <div class="project-stat">
    <span class="project-stat-value">Acrylic Conduit</span>
    <span class="project-stat-label">Concealed Wiring</span>
  </div>
  <div class="project-stat">
    <span class="project-stat-value">Power Injection</span>
    <span class="project-stat-label">Voltage Drop Fixed</span>
  </div>
</div>

<div class="project-video-short-wrapper">
  <div class="project-video-short">
    <iframe src="https://www.youtube.com/embed/XHSU8LGoymg" title="Infinity Mirror LED Hyper Cube — Short demo" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
  </div>
</div>

---

## Mechanical Design & Additive Fabrication

The physical structure of the hyper cube was designed entirely from scratch in Autodesk Fusion 360. Achieving clean geometric alignment across six intersecting mirror faces required custom-engineered 3-axis corner joints, interior mounting brackets, and specialized aluminum trim end-caps designed to securely lock the frame and conceal raw metal edges.

To maintain a sleek exterior free of wire clutter, the structural support members double as functional conduits. Narrow acrylic tubing serves both as a rigid mechanical backbone and as a concealed wire management channel. Because space within the tubing and corner junctions was exceptionally restricted, every electrical lead was meticulously measured, mapped, and cut to precise lengths prior to assembly to prevent internal bunching and maintain structural integrity.

<div class="project-figure">
  <img src="/images/projects/hyper-cube/progress-picture-1.jpg" alt="Assembly of the custom Infinity Mirror LED Hyper Cube frame and acrylic support members" />
  <p class="project-caption">Initial frame assembly showing the integration of 3D printed corner brackets and structural acrylic conduit tubes.</p>
</div>

---

## Electrical Integration & Topology

Powering and controlling over 300 addressable LEDs across a complex three-dimensional lattice required careful circuit planning. Data signal integrity was maintained by routing all LED strips in a single continuous data path traversing every face of the cube in series, ensuring that dynamic lighting patterns and animations travel smoothly and synchronously across all axes.

Soldered connections inside the tight 3D printed corner brackets demanded high-density, low-profile wiring. Each joint was executed with precision to withstand thermal cycling and mechanical stress during final housing assembly.

<div class="project-figure">
  <img src="/images/projects/hyper-cube/solder-joints.jpg" alt="High-density soldered connections in tight corner joints" />
  <p class="project-caption">High-density solder joints within the restricted corner enclosures, establishing robust data and power continuity.</p>
</div>

---

## Hardware Debugging & Power Injection

During initial bench testing, a major electrical hurdle emerged: voltage drop. Across a continuous run of 300+ LEDs operating at full white brightness, internal trace and wire resistance caused significant voltage decay from the power entry point to the furthest diodes. This resulted in severe chromatic degradation, with distant LEDs shifting visibly toward a dim, reddish-orange hue due to insufficient forward voltage on the blue and green channels.

To eliminate this artifact without reducing overall brightness, I redesigned the power distribution bus to implement periodic 5V power injection taps. By feeding regulated power directly into intermediate nodes across the LED matrix, voltage levels remained stable across every diode, instantly restoring uniform brightness and color fidelity.

<div class="project-figure">
  <img src="/images/projects/hyper-cube/voltage-drop.jpg" alt="Visual comparison showing color shift and brightness drop before power injection" />
  <p class="project-caption">Demonstration of voltage drop effects: un-injected runs exhibited noticeable luminance decay and color distortion toward the end of the circuit.</p>
</div>

---

## Integrated Base Enclosure

All supporting electronics were housed in a custom-designed 3D printed base unit situated beneath the cube. The enclosure neatly organizes a high-current 5V DC switching power supply, a Bluetooth wireless receiver module, and heavy-duty terminal blocks, ensuring safe wire termination and easy maintenance access while keeping external clutter out of sight.

<div class="project-figure">
  <img src="/images/projects/hyper-cube/power-source.jpg" alt="Custom 3D printed base housing power supply and Bluetooth controller" />
  <p class="project-caption">The integrated base enclosure housing the 5V DC power supply, terminal blocks, and Bluetooth control module.</p>
</div>

---

## Results & Demonstration

<div class="project-video">
  <iframe
    src="https://www.youtube.com/embed/BINwRzOokhE"
    title="Infinity Mirror LED Hyper Cube — Full demonstration"
    frameborder="0"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
    allowfullscreen>
  </iframe>
</div>

The completed Infinity Mirror LED Hyper Cube successfully merges precision CAD modeling, additive manufacturing, and robust electrical engineering into a striking physical art piece. Designing around severe spatial constraints provided valuable lessons in Design for Manufacturing (DFM), three-dimensional wire harness planning, and power distribution modeling for high-current LED arrays. The Bluetooth smartphone control integration rounds out the build, making it fully interactive and responsive to custom lighting effects.
