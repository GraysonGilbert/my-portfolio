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

What began as an exploratory initiative to master CNC laser cutting operations at a local makerspace rapidly evolved into a rigorous lean manufacturing case study. By bridging GIS data extraction, vector design optimization, and batch production engineering, I developed a multi-layered wooden topographical map of my neighborhood that ultimately scaled into fulfilling 43 bespoke client orders.

<div class="project-figure">
  <img src="/images/projects/lasercut-maps/styled-first-map.jpg" alt="Custom framed wooden topographical map displayed with address pin" />
  <p class="project-caption">Initial production unit: multi-layered birch wood topographical map with custom coordinate pin and natural edge frame.</p>
</div>

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
  <div class="project-stat">
    <span class="project-stat-value">3 Layers</span>
    <span class="project-stat-label">Physical GIS Topology</span>
  </div>
</div>

---

## Lean DFM and Process Optimization

Scaling from a one-off prototype to a 43-unit production run required a systematic Design for Manufacturing (DFM) overhaul. The initial prototype suffered from excessive machine cycle times, inefficient material utilization, and labor-intensive assembly bottlenecks.

<div class="project-arch-grid">
  <div class="project-arch-card">
    <div class="project-arch-core">Initial State: Prototype Inefficiencies</div>
    <div class="project-arch-title">Initial Prototype Bottlenecks</div>
    <ul class="project-arch-list">
      <li>Individual G-code programs for each separate physical layer</li>
      <li>Manual recalibration and focal zeroing stops between cuts</li>
      <li>Loose part layout with excessive scrap stock and long transit moves</li>
      <li>12 hours total end-to-end build time per unit</li>
    </ul>
  </div>
  <div class="project-arch-card">
    <div class="project-arch-core">Optimized State: Scaled Production</div>
    <div class="project-arch-title">Lean Workflow Streamlining</div>
    <ul class="project-arch-list">
      <li>Consolidated all cutting into two master nested programs (layers + frame)</li>
      <li>Tight nesting maximizing raw sheet utilization and eliminating travel waste</li>
      <li>Batch finishing, staining, clamping, and polishing procedures</li>
      <li>Over 50% cut time reduction and drop to ~3 hours total build time</li>
    </ul>
  </div>
</div>

---

## GIS Vector Extraction and Multi-Layer Topology

Translating raw geographic data into physical artifacts required a multi-stage software and vector processing pipeline:

* **Data Extraction & Vectorization:** Extracted raw road networks and land elevation topology using the Google Maps API and SnazzyMaps styling layers to isolate clean vector boundaries.
* **Design for Manufacturing (DFM):** Processed raster and vector data in Inkscape, segmenting the map into three distinct physical elevation tiers: water basin, base land terrain, and the raised road grid. Rigorous node-level vector editing was performed to eliminate stray paths, ensure clean laser kerf compensation, and maintain structural bridges across delicate road networks.
* **Personalized Coordinate Pin:** Engineered a custom architectural feature integrating a distinct coordinate pin calibrated to each client's exact latitude and longitude, anchoring the abstract topographical relief to a personal geographic location.

---

## Production Variant Gallery

To cater to diverse interior aesthetics while maintaining standardized batch manufacturing protocols, three distinct wood stain and finish variants were engineered and cataloged.

<div class="project-gallery-3col">
  <div class="project-compare-item">
    <span class="project-compare-label">Ebony Stain Finish</span>
    <img src="/images/projects/lasercut-maps/black-map.jpg" alt="Topographical map in ebony black finish" />
  </div>
  <div class="project-compare-item">
    <span class="project-compare-label">Maritime Blue Stain</span>
    <img src="/images/projects/lasercut-maps/blue-map.jpg" alt="Topographical map in deep blue finish" />
  </div>
  <div class="project-compare-item">
    <span class="project-compare-label">Coastal Cyan Stain</span>
    <img src="/images/projects/lasercut-maps/light-blue-map.jpg" alt="Topographical map in light blue finish" />
  </div>
</div>

---

## Manufacturing Results and Quality Control

Over a one-year production lifecycle, I managed the complete end-to-end operation from raw material procurement and laser CNC processing to hand-finishing, quality inspection, and direct fulfillment. 

Key engineering and operational outcomes include:
* **Mastery of CNC Laser Operations:** Achieved complete proficiency in laser power, speed, frequency, and focal distance optimization across varied wood composite thicknesses.
* **Streamlined Batch Production:** Standardized clamping, adhesive curing, and finishing jigs that reduced per-unit labor overhead by 75%.
* **Customer Satisfaction:** Successfully delivered 43 custom maps with zero structural defects, proving that rigorous DFM principles can successfully transition hobbyist fabrication into profitable small-scale production.
