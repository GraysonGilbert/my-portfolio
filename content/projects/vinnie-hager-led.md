---
title: "Custom LED Art Installation"
date: 2022-08-01
tags: ["Fabrication", "Electronics", "LED"]
summary: "Hand-crafted an illuminated wall installation translating the intricate linework of artist Vinnie Hager into physical form. Features dozens of hand-formed flexible LED segments, discrete micro-soldering, high-density concealed wire management, and DFM reflections."
cover:
  image: "images/projects/vinnie-hager-led/thumbnail.jpg"
  alt: "Illuminated custom LED art installation"
  hiddenInSingle: false
weight: 17
---

I created this illuminated art installation as a birthday gift for my mom following my college graduation. The project involved translating the intricate, signature linework of local artist Vinnie Hager into a physical LED display. After reaching out to Vinnie and receiving the green light to use his design, I spent countless hours over the summer meticulously hand-crafting the piece, finishing it just in time for her birthday.

<div class="project-compare-grid">
  <div class="project-compare-item">
    <span class="project-compare-label">Daylight / Unlit Form</span>
    <img src="/images/projects/vinnie-hager-led/completed.jpg" alt="Daylight unlit form showcasing painted canvas and clean surface aesthetics" />
  </div>
  <div class="project-compare-item">
    <span class="project-compare-label">Night / Fully Illuminated</span>
    <img src="/images/projects/vinnie-hager-led/finished-design.jpg" alt="Night fully illuminated state displaying vibrant LED segments" />
  </div>
</div>

---

## Phase 1: Scale Template and Frame Construction

Figuring out how to turn Vinnie’s complex, organic 2D linework into a physical structure took a lot of careful planning. I kicked off the build by making a massive, full-scale print template to act as my roadmap. This allowed me to map out the exact curves, measure all the LED segment lengths, and plan exactly where the wires would need to punch through the acrylic backing.

<div class="project-figure">
  <img src="/images/projects/vinnie-hager-led/full-scale-print-template.jpg" alt="Full scale print template for LED layout" />
  <p class="project-caption">Full-scale print template utilized to map exact curvature, segment lengths, and wiring feedthrough points across the canvas.</p>
</div>

---

## Phase 2: Manual Forming and High-Density Micro-Soldering

Next, I dove into the most labor-intensive phase: the LEDs. I spent hours measuring, cutting, and shaping each flexible LED segment before carefully bonding them in the correct shape to an acrylic backing. The real challenge, though, was the soldering. I hand-soldered tiny power and ground connections for every individual segment. To prevent voltage drop and keep the illumination perfectly even across the whole piece, I also had to route power rails throughout the project to periodically inject power back into the circuit.

<div class="project-compare-grid">
  <div class="project-compare-item">
    <span class="project-compare-label">Early Segment Layout & Wiring</span>
    <img src="/images/projects/vinnie-hager-led/progress-picture-1.jpg" alt="Early segment layout and initial wiring phase" />
  </div>
  <div class="project-compare-item">
    <span class="project-compare-label">High-Density Segment Integration</span>
    <img src="/images/projects/vinnie-hager-led/progress-picture-2.jpg" alt="High-density segment integration across the canvas" />
  </div>
</div>

---

## Lessons Learned: Design For Manufacturing (DFM)

While I managed to build the whole thing by hand, this project made me realize how much easier it would be with a CNC. If I were to do it again, I would definitely ditch the manual fabrication, model the design in CAD, and cut everything on a CNC router to streamline the whole process.

