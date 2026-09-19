---
title: "Custom Modular Storage System: Ikea Billy Bookcase"
date: 2023-01-01
tags: ["CAD", "3D Printing", "Fusion 360", "Fabrication"]
summary: "Designed and manufactured a custom 3D-printed modular sliding drawer system for the Ikea Billy Bookcase, featuring a low-friction rolling-ball slide mechanism, standardized unit scaling, and a magnetic flush-mount acrylic enclosure."
cover:
  image: "images/projects/bookcase/thumbnail.png"
  alt: "Custom 3D-printed modular storage drawers inside an Ikea Billy Bookcase"
  hiddenInSingle: false
weight: 10
---

I needed a better way to organize my shop hardware, so I designed a custom 3D-printed modular sliding drawer system specifically for an Ikea Billy Bookcase. Inspired by the Gridfinity standard, this project replaces expensive commercial drawer slides with a high-density, fully printable storage setup.

---

## Sliding Track Design

Commercial drawer slides are expensive and usually require custom brackets to fit cleanly into standard shelving. To get around this, I designed a fully 3D-printed track assembly. 

The rails use standard plastic BBs inside closed raceways to act as cheap, low-friction linear bearings. To keep the drawers from drifting open, I integrated pockets at the back of each track for neodymium magnets. These act as a solid detent, holding the drawers securely closed without needing any mechanical latches.

---

## Drawer Scaling & CAD Assembly

To make the most of the vertical space, I indexed the drawer sizes directly to the Billy Bookcase's existing shelf-pin holes. This established a standard height increment:

- **Small Drawer (1 Unit):** Sized for low-profile tools, calipers, and drill bits.
- **Medium Drawer (2 Units):** Sized for fasteners, hand tools, and other hardware.
- **Large Drawer (5 Units):** Designed for bulky items and my multimeter.

Before printing a single part, I modeled the full bookcase and drawer stack in Fusion 360. Verifying the mechanical clearances and track tolerances in CAD upfront saved a massive amount of filament and trial-and-error.

<div class="project-figure">
  <img src="/images/projects/bookcase/digital-twin.jpg" alt="Fusion 360 assembly model of the modular storage system inside the Ikea Billy Bookcase" />
  <p class="project-caption">Fusion 360 assembly showing the vertical stack, shelf indexing, and interlocking rail housings.</p>
</div>

---

## CAD to Print Reality

Getting the printed parts to actually match the CAD models required spending some time dialing in the printer's horizontal expansion and cooling settings. Once the tolerances were locked in, the final printed drawers came out matching the digital designs.

### Small Drawer (1-Unit)

<div class="project-compare-grid">
  <div class="project-compare-item">
    <span class="project-compare-label">CAD Model</span>
    <img src="/images/projects/bookcase/s-drawer-cad.jpg" alt="CAD model of the 1-unit small drawer" />
  </div>
  <div class="project-compare-item">
    <span class="project-compare-label">Printed Part</span>
    <img src="/images/projects/bookcase/s-drawer-real.jpg" alt="Manufactured 1-unit small drawer" />
  </div>
</div>

### Medium Drawer (2-Unit)

<div class="project-compare-grid">
  <div class="project-compare-item">
    <span class="project-compare-label">CAD Model</span>
    <img src="/images/projects/bookcase/m-drawer-cad.jpg" alt="CAD model of the 2-unit medium drawer" />
  </div>
  <div class="project-compare-item">
    <span class="project-compare-label">Printed Part</span>
    <img src="/images/projects/bookcase/m-drawer-real.jpg" alt="Manufactured 2-unit medium drawer" />
  </div>
</div>

### Large Drawer (5-Unit)

<div class="project-compare-grid">
  <div class="project-compare-item">
    <span class="project-compare-label">CAD Model</span>
    <img src="/images/projects/bookcase/l-drawer-cad.jpg" alt="CAD model of the 5-unit large drawer" />
  </div>
  <div class="project-compare-item">
    <span class="project-compare-label">Printed Part</span>
    <img src="/images/projects/bookcase/l-drawer-real.jpg" alt="Manufactured 5-unit large drawer" />
  </div>
</div>

---

## Hardware Organization

For the inside of the drawers, I used the open-source Gridfinity system so all my bins and tool holders snap directly into the baseplate. I also modeled custom inserts for things like my calipers, so they don't slide around every time I open the drawer.

<div class="project-compare-grid">
  <div class="project-compare-item">
    <span class="project-compare-label">Gridfinity Bins</span>
    <img src="/images/projects/bookcase/gridfinity-storage.jpg" alt="Gridfinity storage matrix inside drawer" />
  </div>
  <div class="project-compare-item">
    <span class="project-compare-label">Custom Caliper Insert</span>
    <img src="/images/projects/bookcase/caliper-insert.jpg" alt="Custom 3D-printed caliper insert" />
  </div>
</div>

---

## Magnetic Acrylic Enclosure

I wanted a clean door for the top section of the bookcase to hide some electronics and loose parts. I originally designed a set of complex mechanical hinges, but they were over-engineered, prone to sagging, and got in the way. 

I ended up scrapping the hinges entirely for a flush-mounted laser-engraved acrylic panel held on by countersunk neodymium magnets. It snaps cleanly into place, and when I need to get into the shelf, I just pull the whole door off and magnetically dock it to the top of the bookcase.

<div class="project-figure">
  <img src="/images/projects/bookcase/top-door-open.jpg" alt="Top acrylic enclosure door open with magnetic mounting interface" />
  <p class="project-caption">The flush-mount acrylic door removed, showing the simple magnetic mounting interface.</p>
</div>

---

## Project Wrap-Up

This setup successfully turned a standard Ikea bookcase into a solid, high-density hardware organizer. The BB-loaded tracks and magnetic detents ended up working great for daily use.

Building this was a great way to practice managing clearance tolerances and parametric modeling in Fusion 360. While I successfully scaled the models manually for the three drawer sizes, my next iteration will use master sketches and user parameters so the drawer dimensions can automatically scale to any shelf height on the fly.

<div class="project-figure">
  <img src="/images/projects/bookcase/finished-bookcase.jpg" alt="Fully assembled custom modular storage system installed in the Ikea Billy Bookcase" />
  <p class="project-caption">The final custom storage system installed in the Ikea Billy Bookcase.</p>
</div>