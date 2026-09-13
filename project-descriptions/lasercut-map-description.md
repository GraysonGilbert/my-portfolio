## **Topographical Laser Cut Maps: Prototype to Production**

**Overview** What began as an initiative to master laser cutting operations at a local makerspace evolved into a small-scale production run of custom topographical maps. By leveraging map data and vector graphics, I engineered a multi-layered, multi-operation wooden map of my neighborhood, which ultimately generated enough local demand to manufacture and fulfill 43 individual orders.

**Design & Prototyping**

* **Data Extraction & Vectorization:** Utilized the Google Maps API (via SnazzyMaps) to extract raw road and land topology data.  
* **Design for Manufacturing (DFM):** Processed the raw image data in Inkscape, isolating the map into three distinct physical layers: water, land, and roads. This required extensive node-level vector editing to ensure clean toolpaths, structural integrity, and proper cut tolerances.  
* **Customization:** Integrated a customizable pin feature calibrated to specific latitude and longitude coordinates, allowing each piece to be personalized to the client's exact address.

**Process Optimization & Manufacturing**

* **Toolpath & Nesting Efficiency:** The initial prototype exposed inefficiencies in material usage and machine time. For the production models, I consolidated individual layer files into two highly optimized programs (one for map layers, one for the frame).  
* **Cycle Time Reduction:** By tightly nesting parts to maximize stock surface area, eliminating manual machine calibration stops between layers, and optimizing laser travel paths, I reduced the CNC laser cutting time by over 50%.  
* **Streamlined Assembly:** Iterated on the painting, gluing, and polishing techniques throughout the production run. This continuous workflow improvement reduced the total end-to-end build time per map from 12 hours down to just over 3 hours.

**Results & Takeaways** After successfully building the optimized production model, I marketed the maps to my local community. Over the course of a year, I managed the end-to-end sales, manufacturing, and delivery of 43 custom maps, ensuring strict quality control and high customer satisfaction on every unit while fully mastering the makerspace's laser cutter workflow.