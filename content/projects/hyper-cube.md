---
title: "Infinity Mirror LED Hyper Cube"
date: 2021-06-01
tags: ["Electronics", "LED", "Fabrication", "3D Printing", "Fusion 360"]
summary: "Engineered and fabricated a custom infinity mirror LED cube from scratch, integrating 300+ individually addressable LEDs, concealed wire routing through structural acrylic tubing, and a Bluetooth smartphone controller — including hardware troubleshooting to resolve matrix voltage drop."
cover:
  image: "images/projects/hyper-cube/thumbnail.png"
  alt: "Assembly of the custom Infinity Mirror LED Hyper Cube frame"
  hiddenInSingle: false
weight: 13
---

I designed and built this LED Infinity Cube using 3D printed frames, one-way mirror film, and over 300 individually addressable WS2812B LEDs to create a 3D infinity mirror effect. The lighting is fully managed via a custom Bluetooth smartphone controller. The core challenge of the build was managing the high-density electrical routing for the LED strips while keeping the mechanical footprint tight enough to hide the wiring.


<div class="project-video-short-wrapper">
  <div class="project-video-short">
    <iframe src="https://www.youtube.com/embed/XHSU8LGoymg" title="Infinity Mirror LED Hyper Cube — Short demo" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
  </div>
</div>

---

## Mechanical Design & 3D Printing

I designed the physical structure of the cube in Fusion 360. To get the six mirror faces to align cleanly, I modeled custom 3-axis corner joints, interior mounting brackets, and aluminum trim end-caps to lock the frame together and hide the raw metal edges.

To keep the exterior looking clean, the structural support members also act as wire conduits. I used narrow acrylic tubing to form a rigid backbone that could simultaneously hide the wire routing. Because the space inside the tubes and 3D printed corners was incredibly tight, every wire had to be cut to an exact length to avoid bunching up during final assembly.

<div class="project-figure">
  <img src="/images/projects/hyper-cube/progress-picture-1.jpg" alt="Assembly of the custom Infinity Mirror LED Hyper Cube frame and acrylic support members" />
  <p class="project-caption">Initial frame assembly showing the 3D printed corner brackets and the acrylic conduit tubes.</p>
</div>

---

## Electrical Routing

Wiring over 300 addressable LEDs across a 3D frame required some upfront planning. To keep the dynamic lighting animations smooth and synchronized, I routed the data line as a single continuous path, running in series across every face of the cube.

This meant all the soldered connections had to fit inside the tight 3D printed corner brackets. The wiring had to be low-profile and solid enough to handle the mechanical stress of snapping the final housing together without breaking a connection.

<div class="project-figure">
  <img src="/images/projects/hyper-cube/solder-joints.jpg" alt="High-density soldered connections in tight corner joints" />
  <p class="project-caption">High-density solder joints squeezed into the corner brackets to keep the data and power lines hidden.</p>
</div>

---

## Debugging Voltage Drop & Power Injection

During initial bench testing, I ran into a standard WS2812B issue: voltage drop. With a continuous run of 300+ LEDs at full white brightness, the resistance in the strips caused the voltage to sag toward the end of the line. This resulted in the furthest LEDs turning a dim, reddish-orange color because the blue and green diodes weren't getting enough voltage to turn on fully.

To fix this without capping the overall brightness in software, I added 5V power injection taps to the wiring harness. By running standard power lines directly to intermediate points across the LED matrix, the voltage stayed stable across the entire run, restoring the bright, uniform white color.

<div class="project-figure">
  <img src="/images/projects/hyper-cube/voltage-drop.jpg" alt="Visual comparison showing color shift and brightness drop before power injection" />
  <p class="project-caption">Demonstration of voltage drop: the un-injected LED runs dim and shift orange toward the end of the circuit.</p>
</div>

---

## Integrated Base Enclosure

All supporting electronics were housed in a custom-designed base unit situated beneath the cube. The enclosure neatly organizes a high-current 5V DC switching power supply, and a Bluetooth wireless receiver module, ensuring safe wire termination and easy maintenance access while keeping external clutter out of sight.

<div class="project-figure">
  <img src="/images/projects/hyper-cube/power-source.jpg" alt="Custom 3D printed base housing power supply and Bluetooth controller" />
  <p class="project-caption">The integrated base enclosure housing the 5V DC power supply and Bluetooth control module.</p>
</div>

---

## Results & Demonstration

<div class="project-video">
  <iframe width="560" height="315" src="https://www.youtube.com/embed/BINwRzOokhE?si=xJFP7afQUzj1052N" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
</div>
