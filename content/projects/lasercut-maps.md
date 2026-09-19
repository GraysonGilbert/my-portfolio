---
title: "Topographical Laser Cut Maps: Prototype to Production"
date: 2023-02-01
tags: ["Fabrication", "Laser Cutting", "DFM"]
summary: "Engineered a multi-layer topographical map from raw GIS data through to a streamlined production workflow, fulfilling 43 custom orders with a 75% reduction in total build time and a 50% cut in CNC laser cycle time."
cover:
  image: "images/projects/lasercut-maps/thumbnail.jpg"
  alt: "Custom wooden multi-layer topographical laser-cut map"
  hiddenInSingle: false
weight: 15
---

What started as a personal project to learn the laser cutter at my local makerspace quickly turned into a small-scale production run. By combining GIS data, vector design, and batch manufacturing techniques, I built a multi-layered wooden topographical map of my neighborhood that eventually scaled into fulfilling 43 custom orders.

<div class="project-stats">
  <div class="project-stat">
    <span class="project-stat-value">43 Units</span>
    <span class="project-stat-label">Orders Fulfilled</span>
  </div>
  <div class="project-stat">
    <span class="project-stat-value">12h → 3h</span>
    <span class="project-stat-label">Build Time per Unit</span>
  </div>
  <div class="project-stat">
    <span class="project-stat-value">>50%</span>
    <span class="project-stat-label">Laser Cycle Reduction</span>
  </div>
</div>

---

## Lean DFM and Process Optimization

Scaling from a single prototype to a 43-unit run meant I had to completely rethink how these maps were made. The first version simply took too long to cut, wasted a lot of wood, and was incredibly tedious to assemble.

<div class="project-arch-grid">
  <div class="project-arch-card">
    <div class="project-arch-core">The Prototype</div>
    <div class="project-arch-title">Initial Bottlenecks</div>
    <ul class="project-arch-list">
      <li>Running separate cut files for every single physical layer</li>
      <li>Stopping to manually recalibrate the laser focus between cuts</li>
      <li>Poor part layout leading to wasted material and long laser travel times</li>
      <li>~12 hours of total end-to-end build time per map</li>
    </ul>
  </div>
  <div class="project-arch-card">
    <div class="project-arch-core">The Production Run</div>
    <div class="project-arch-title">Batch Manufacturing</div>
    <ul class="project-arch-list">
      <li>Combined all layers and frame components into just two nested cut files</li>
      <li>Tightly nested parts to maximize the wood sheet and reduce laser travel</li>
      <li>Switched to batch processes for staining, clamping, and gluing</li>
      <li>Cut laser time by over 50% and dropped total build time to ~3 hours</li>
    </ul>
  </div>
</div>

---

## GIS Data & Vector Design

Turning raw geographic data into a clean, cuttable design required a structured vector workflow:

* **Data Extraction:** I pulled the raw road networks and land boundaries using the Google Maps API and SnazzyMaps to isolate high-contrast vector lines.
* **Vector Optimization:** Using Inkscape, I separated the map into three physical layers: the water basin, the base land terrain, and the top road grid. I cleaned up the vector nodes to remove stray paths, account for the laser kerf, and ensure the delicate road networks remained structurally intact.
* **Custom Coordinate Pin:** To personalize each map, I added a custom marker pin placed at the client's exact latitude and longitude.

---

## Color Variants

To offer a few different styles without slowing down batch production, I offered three standard stain colors for the map's background water layer.

<div class="project-gallery-3col">
  <div class="project-compare-item">
    <span class="project-compare-label">Black</span>
    <img src="/images/projects/lasercut-maps/black-map.jpg" alt="Topographical map with black water background" />
  </div>
  <div class="project-compare-item">
    <span class="project-compare-label">Deep Blue</span>
    <img src="/images/projects/lasercut-maps/blue-map.jpg" alt="Topographical map with deep blue water background" />
  </div>
  <div class="project-compare-item">
    <span class="project-compare-label">Light Blue</span>
    <img src="/images/projects/lasercut-maps/light-blue-map.jpg" alt="Topographical map with light blue water background" />
  </div>
</div>

---

## Production Results

Over the course of a 8 months, I managed the entire process from buying the raw wood and running the laser cutter to final assembly and shipping. 

Key takeaways from the production run include:
* **CNC Laser Operations:** Dialed in the laser power, speed, frequency, and focal distance settings across various wood thicknesses to get perfectly clean, repeatable cuts.
* **Assembly Jigs:** Built standard jigs for clamping, gluing, and finishing that cut the manual assembly time per unit by 75%.
* **Final Delivery:** Manufactured and delivered 43 custom maps, successfully turning a hobbyist makerspace project into a reliable small-scale production run.