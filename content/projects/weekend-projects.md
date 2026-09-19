---
title: "Weekend & Rapid Fabrication Projects"
date: 2024-01-01
tags: ["Fabrication", "CAD", "Laser Cutting", "3D Printing", "CNC", "CAM"]
summary: "A curated anthology of rapid prototypes, targeted repairs, and community initiatives — spanning CNC-machined marine wayfinding signage, reverse-engineered 3D printed adapters, upcycled laser goods, and fluid flow jug modifications."
cover:
  image: "images/projects/weekend-projects/thumbnail.png"
  alt: "CNC-machined marine wayfinding sign installed at neighborhood marina"
  hiddenInSingle: false
weight: 19
---

Outside of my main engineering work, I usually have a few quick hardware or fabrication projects running on the side. This section is a collection of one-off weekend builds, repairs, and small commissions designed to solve practical problems or just use up scrap material around the shop.

<div class="project-arch-grid">
  <div class="project-arch-card">
    <div class="project-arch-core">Community Projects</div>
    <div class="project-arch-title">Signs & Awards</div>
    <ul class="project-arch-list">
      <li>Marina Dock Slip Markers: Outdoor vinyl cutting</li>
      <li>CNC Marina Sign: Machined two-color marine HDPE</li>
      <li>Chesapeake Race Awards: Batch laser-cut wooden keychains</li>
    </ul>
  </div>
  <div class="project-arch-card">
    <div class="project-arch-core">Practical Fixes</div>
    <div class="project-arch-title">CAD & 3D Printing</div>
    <ul class="project-arch-list">
      <li>Wing Foil Pump Adapter: Reverse-engineered 3D-printed nozzle</li>
      <li>Hardwood Coasters: Laser-cut from shop scrap with cork backing</li>
      <li>RTIC Jug Vent Retrofit: 3D-printed vent plug to stop pouring air-lock</li>
    </ul>
  </div>
</div>

---

## Marina Dock Signs

I made a set of high-visibility signs for my local marina to make it easier for visiting boaters to navigate the slips. I laid out the vector graphics and typography in LightBurn, then cut the final decals from all-weather outdoor vinyl using a Cricut plotter before mounting them directly to the wooden dock posts.

<div class="project-compare-grid">
  <div class="project-compare-item">
    <span class="project-compare-label">Pier A Marker</span>
    <img src="/images/projects/weekend-projects/a-pier.jpg" alt="Pier A marine dock marker sign" />
  </div>
  <div class="project-compare-item">
    <span class="project-compare-label">Pier B Marker</span>
    <img src="/images/projects/weekend-projects/b-pier.jpg" alt="Pier B marine dock marker sign" />
  </div>
</div>

<div class="project-compare-grid">
  <div class="project-compare-item">
    <span class="project-compare-label">Pumpout Station</span>
    <img src="/images/projects/weekend-projects/pumpout.jpg" alt="Pumpout station marina sign" />
  </div>
  <div class="project-compare-item">
    <span class="project-compare-label">Transient Dock</span>
    <img src="/images/projects/weekend-projects/transient-dock.jpg" alt="Transient dock marine sign" />
  </div>
</div>

---

## CNC-Machined Marina Sign

Following the success of the slip markers, I was commissioned to design and fabricate a primary, weather-resistant entrance sign positioned to welcome boat traffic approaching the marina from the water. 

The 2D vector artwork was developed in LightBurn and transitioned into Autodesk Fusion 360 to generate a complete 3D CAD model incorporating mounting pocket reliefs and bevel profiles. The sign was machined from dual-color, UV-stabilized marine-grade sign plastic on an industrial CNC router; the subtractive routing operation cuts through the top UV layer to expose the contrasting core color, guaranteeing high legibility without paint.

<div class="project-compare-grid">
  <div class="project-compare-item">
    <span class="project-compare-label">Fusion 360 CAD Model</span>
    <img src="/images/projects/weekend-projects/br-sign-cad.png" alt="Fusion 360 CAD model of marina wayfinding sign" />
  </div>
  <div class="project-compare-item">
    <span class="project-compare-label">CNC Machined Sign</span>
    <img src="/images/projects/weekend-projects/br-sign-real.jpg" alt="CNC machined dual-color plastic sign" />
  </div>
</div>

<div class="project-figure">
  <img src="/images/projects/weekend-projects/br-sign-installed.jpg" alt="Installed CNC marina sign on pilings" />
  <p class="project-caption">Final weather-resistant entrance sign installed on marina pilings facing water traffic.</p>
</div>

---

## Wing Foil Pump Adapter

When I couldn't find an off-the-shelf adapter to inflate my wing for foil, I decided to just print one. 

I took some caliper measurements of the proprietary valve and the pump hose, then modeled a high-pressure nozzle in CAD with a tapered barb to ensure a tight seal. I printed it solid (100% infill) in PETG. It worked perfectly in the field and easily handles the sustained inflation pressures without leaking.

<div class="project-compare-grid">
  <div class="project-compare-item">
    <span class="project-compare-label">CAD Model & Seal Geometry</span>
    <img src="/images/projects/weekend-projects/wing-adapter-cad.png" alt="CAD model showing seal geometry of pump adapter" />
  </div>
  <div class="project-compare-item">
    <span class="project-compare-label">Functional 3D Print</span>
    <img src="/images/projects/weekend-projects/wing-adapter-printed.jpg" alt="Functional 3D printed wing foil pump adapter" />
  </div>
</div>

---

## Scrap Hardwood Coasters

To keep scrap waste down in the shop, I started repurposing the high-grade hardwood cutoffs left over from my laser-cut map runs. 

I laser-cut the scraps into blanks and engraved them with local maps and wildlife designs. To speed up the cork backing process, I 3D-printed a quick alignment jig that holds the wooden disks securely, making batch assembly fast and perfectly registered every time.

<div class="project-compare-grid">
  <div class="project-compare-item">
    <span class="project-compare-label">Blue Ridge Mountain</span>
    <img src="/images/projects/weekend-projects/br-coaster.jpg" alt="Laser engraved Blue Ridge Mountain wooden coaster" />
  </div>
  <div class="project-compare-item">
    <span class="project-compare-label">Great Blue Heron</span>
    <img src="/images/projects/weekend-projects/heron-coaster.jpg" alt="Laser engraved Great Blue Heron wooden coaster" />
  </div>
</div>

---

## Chesupeake Paddleboard Race Keychains

I race paddleboards weekly on the Chesapeake Bay during the summer, so I offered to make some custom awards for the top finishers in the local Chesupeake SUP community. 

I converted the organization's event branding into vector artwork, then set up a batch production run on the laser cutter. The logos were etched directly onto hardwood keychain blanks, resulting in solid, practical end-of-season awards for the podium.

<div class="project-compare-grid">
  <div class="project-compare-item">
    <span class="project-compare-label">Single Award Blank</span>
    <img src="/images/projects/weekend-projects/chesupeake-keychain.jpg" alt="Single custom wooden keychain award" />
  </div>
  <div class="project-compare-item">
    <span class="project-compare-label">Batch Production Run</span>
    <img src="/images/projects/weekend-projects/multiple-keychains.jpg" alt="Batch production array of wooden keychains" />
  </div>
</div>

<div class="project-figure">
  <img src="/images/projects/weekend-projects/podium.JPG" alt="Podium presentation with winners receiving wooden awards" />
  <p class="project-caption">End-of-season podium presentation for the top-finishing racers.</p>
</div>

---

## RTIC 1-Gallon Jug Vent Mod

Standard RTIC one-gallon water jugs glug terribly because the lid doesn't vent air properly, creating a vacuum that restricts the pouring flow rate. 

Since I couldn't find an aftermarket fix, I modeled a vent plug in CAD and printed it in food-safe PETG. The plug adds an internal air channel that completely breaks the vacuum, cutting the pouring time down to seconds.

<div class="project-compare-grid">
  <div class="project-compare-item">
    <span class="project-compare-label">Vent Plug CAD</span>
    <img src="/images/projects/weekend-projects/rtic-mod-cad-1.png" alt="CAD top view of RTIC vent plug" />
  </div>
  <div class="project-compare-item">
    <span class="project-compare-label">Airflow Channel</span>
    <img src="/images/projects/weekend-projects/rtic-mod-cad-2.png" alt="CAD section view showing internal airflow channel" />
  </div>
</div>

<div class="project-figure">
  <iframe style="margin: 0 auto; display: block;" width="315" height="560" src="https://www.youtube.com/embed/ayCk9LnYBnc" title="RTIC Vent Mod Demonstration" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
  <p class="project-caption" style="text-align: center;">Demonstration of the functional vent plug installed on the RTIC lid, completely eliminating the pouring vacuum.</p>
</div>