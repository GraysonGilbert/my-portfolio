---
title: "CNC-Routed LED Sign: Maui, Hawaii"
date: 2023-03-01
tags: ["CNC", "CAD", "CAM", "Fabrication"]
summary: "Designed and manufactured a custom illuminated sign of the island of Maui on a Shapeoko 3 XXL CNC router, featuring multi-tool CAM toolpaths, custom V-carve engraving, and a recessed LED lighting system."
cover:
  image: "images/projects/maui-cnc-sign/finished-maui.jpg"
  alt: "Finished illuminated CNC-routed sign of Maui, Hawaii"
  hiddenInSingle: false
weight: 11
---

Bridging digital vector graphics and physical subtractive manufacturing, this project explores the end-to-end design and manufacturing workflow for a custom illuminated topographic wall art piece. Centered on the island of Maui, Hawaii, the sign combines precision multi-tool CNC routing, custom V-carve engraving, and an integrated LED backlighting system into a clean, flush-mounting presentation.

<div class="project-stats">
  <div class="project-stat">
    <span class="project-stat-value">3 Tools</span>
    <span class="project-stat-label">Multi-Operation CAM</span>
  </div>
  <div class="project-stat">
    <span class="project-stat-value">90° V-Bit</span>
    <span class="project-stat-label">Detail Engraving</span>
  </div>
  <div class="project-stat">
    <span class="project-stat-value">Shapeoko 3</span>
    <span class="project-stat-label">CNC Router</span>
  </div>
  <div class="project-stat">
    <span class="project-stat-value">Recessed</span>
    <span class="project-stat-label">LED Lighting Channel</span>
  </div>
</div>

---

## CAD and Vector Design Workflow

The development pipeline began by processing a reference outline of the island using Inkscape. The raster map was manually traced to capture accurate perimeter geometry, and custom typography was integrated directly into the vector layout before exporting a clean SVG file.

The SVG vector graphic was then imported into Autodesk Fusion 360 as a baseline sketch. The geometry was cleaned up to remove extraneous spline nodes, and parametric offset and extrusion tools were utilized to establish the three-dimensional body, outer boundary contours, and a rear internal channel designed to house the LED lighting components and wiring harness.

---

## CAM and Multi-Tool CNC Machining

To transform the 3D model into physical hardware, three distinct manufacturing operations were programmed in Fusion 360 and executed via Carbide Motion on a Shapeoko 3 XXL CNC router using medium-density fiberboard (MDF) stock:

* **Engraving:** Intricate text detailing and geographic markers were machined using a 90-degree V-carve bit, producing crisp line-width variations based on depth.
* **Pocketing:** An internal trench was cleared using a 1/8-inch flat end mill to create the recess for the LED strip.
* **Profiling:** The final outer contour of the island was separated from the stock using a 1/4-inch flat end mill with tab supports to prevent part shift during final passes.

<div class="project-figure">
  <img src="/images/projects/maui-cnc-sign/cnc-maui.jpg" alt="Shapeoko 3 XXL executing multi-tool CAM toolpaths in MDF stock" />
  <p class="project-caption">Shapeoko 3 XXL executing precision multi-tool CNC routing operations on MDF stock.</p>
</div>

---

## Hardware Integration and Final Assembly

Achieving a professional presentation required careful attention to both mechanical mounting and electrical wiring:

* **Flush Wall Mounting:** A manual milling machine was used to cut precision recessed keyhole mounting points into the back face of the MDF body, allowing the completed sign to hang completely flat against a wall with zero visible hardware.
* **Electrical Integration:** An LED lighting strip was press-fit into the CNC-routed internal channel. A DC barrel jack connector was soldered to the leads and secured to the enclosure, providing a robust interface for an external power supply.

---

## Results and Reflections

<div class="project-figure">
  <img src="/images/projects/maui-cnc-sign/finished-maui.jpg" alt="Finished illuminated CNC-routed sign of Maui, Hawaii" />
  <p class="project-caption">Completed illuminated sign featuring edge-diffused LED backlighting and crisp V-carved typography.</p>
</div>

The finished sign successfully demonstrates the power of integrating vector graphics workflows with multi-operation CNC machining. Key technical takeaways from the fabrication process include:

* **Toolpath Efficiency:** Sequencing tool changes from fine V-carving to heavy profiling minimized tool deflection and ensured tight registration across operations.
* **Feed and Speed Optimization:** Tuning spindle RPM and feed rates specifically for MDF stock prevented edge burn while maximizing surface finish quality.
* **Workholding and Zeroing:** Implementing robust zeroing protocols for tool changes eliminated Z-axis offset errors during multi-tool swaps.
