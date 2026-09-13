---
title: "Custom LED Art Installation"
date: 2022-08-01
tags: ["Fabrication", "Electronics", "LED", "Soldering"]
summary: "Hand-crafted an illuminated wall installation translating the intricate linework of artist Vinnie Hager into physical form. Features dozens of hand-formed flexible LED segments, discrete micro-soldering, high-density concealed wire management, and DFM reflections."
cover:
  image: "images/projects/vinnie-hager-led/completed.jpg"
  alt: "Illuminated custom LED art installation"
  hiddenInSingle: false
weight: 17
---

Created as a bespoke gift, this project involved translating the intricate signature linework of local artist Vinnie Hager into a physical illuminated art installation, completed with the artist's permission. Over the course of a summer, the piece was meticulously hand-crafted, focusing on precise geometric fidelity, clean aesthetic contrast, and robust electrical integration.

<div class="project-compare-grid">
  <div class="project-compare-item">
    <span class="project-compare-label">Daylight / Unlit Form</span>
    <img src="/images/projects/vinnie-hager-led/finished-design.jpg" alt="Daylight unlit form showcasing painted canvas and clean surface aesthetics" />
  </div>
  <div class="project-compare-item">
    <span class="project-compare-label">Night / Fully Illuminated</span>
    <img src="/images/projects/vinnie-hager-led/completed.jpg" alt="Night fully illuminated state displaying vibrant LED segments" />
  </div>
</div>

---

## Phase 1: Scale Template and Frame Construction

Translating complex organic linework from two-dimensional concept art into a rigid physical chassis required precise dimensional planning. The fabrication workflow began by generating a full-scale print template to map exact curves, segment lengths, and feedthrough points across the canvas.

<div class="project-figure">
  <img src="/images/projects/vinnie-hager-led/full-scale-print-template.jpg" alt="Full scale print template for LED layout" />
  <p class="project-caption">Full-scale print template utilized to map exact curvature, segment lengths, and wiring feedthrough points across the canvas.</p>
</div>

With the blueprint validated, physical construction moved to building the structural wooden frame and preparing the hand-painted canvas backdrop.

<div class="project-compare-grid">
  <div class="project-compare-item">
    <span class="project-compare-label">Structural Wooden Frame</span>
    <img src="/images/projects/vinnie-hager-led/building-frame.jpg" alt="Structural wooden frame assembly" />
  </div>
  <div class="project-compare-item">
    <span class="project-compare-label">Hand-Painted Canvas Backdrop</span>
    <img src="/images/projects/vinnie-hager-led/painting.jpg" alt="Hand-painted canvas backdrop" />
  </div>
</div>

---

## Phase 2: Manual Forming and High-Density Micro-Soldering

Once the substrate and enclosure were established, the most labor-intensive phase commenced: fabricating and integrating the lighting elements. This required individually measuring, cutting, shaping, and chemically bonding dozens of flexible LED segments to mirror the original artwork's intricate glyphs.

Power and ground distribution demanded meticulous hand-soldering of discrete lead pairs for every individual segment across the canvas, establishing reliable electrical continuity without compromising structural profiles.

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

## Phase 3: Concealed Wire Management

A critical design mandate for the installation was completely hiding all electrical bus wires and interconnections from the front-facing view. Maintaining an immaculate visual surface when unlit or illuminated required engineering tight-clearance sub-surface pass-throughs. 

Where continuous graphical symbols spanned across isolated regions, concealed sub-surface interconnects had to be routed for over 12 distinct LED segments, requiring precise spatial planning to prevent wire pinching, short circuits, or pressure bulges on the painted canvas.

---

## Design for Manufacturing (DFM) Evolution

While successfully executed entirely by hand, this project highlighted the critical value of automated manufacturing processes for complex geometries. In future iterations, transitioning from manual fabrication to a computer-numerical-control (CNC) workflow would dramatically optimize scalability.

<div class="project-arch-grid">
  <div class="project-arch-card">
    <div class="project-arch-core">Executed Prototype: Manual Craftsmanship</div>
    <div class="project-arch-title">Hand-Formed Fabrication Process</div>
    <ul class="project-arch-list">
      <li>Manual forming, trimming, and bonding of dozens of curved LED strips</li>
      <li>Hand-soldered point-to-point discrete lead pairs across every glyph</li>
      <li>Custom hand-tensioned wire routing behind painted canvas</li>
      <li>High labor duration requiring an entire summer of focused build time</li>
    </ul>
  </div>
  <div class="project-arch-card">
    <div class="project-arch-core">Production Evolution: Automated DFM</div>
    <div class="project-arch-title">CNC-Routed Subtractive Workflow</div>
    <ul class="project-arch-list">
      <li>Vectorized CAD contours with parametric pocket offsets in Fusion 360</li>
      <li>CNC-machined acrylic substrate with precision recessed friction-fit trenches</li>
      <li>Integrated underside wire raceways eliminating manual feedthroughs</li>
      <li>Estimated 85% cycle time reduction with micron-level geometric repeatability</li>
    </ul>
  </div>
</div>
