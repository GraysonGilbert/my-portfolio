---
title: "ACME Monocular Perception & 3D Spatial Tracking Module"
date: 2025-10-18
tags: ["Computer Vision", "C++", "YOLOv5", "Depth Anything", "OpenCV", "Machine Learning"]
summary: "Engineered a production-grade C++ monocular perception library for ACME Robotics fusing YOLOv5 2D object detection with the 'Depth Anything' foundation model. Delivers real-time 3D human obstacle detection and metric spatial localization from a single RGB camera stream, supported by a comprehensive CMake, CTest, Cppcheck, and Doxygen CI/CD suite."
cover:
  image: "images/projects/monocular-perception-and-tracking/thumbnail.png"
  alt: "Monocular 3D perception pipeline detecting humans and estimating metric (X, Y, Z) coordinates"
  hiddenInSingle: true
weight: 4
---

The ACME Monocular Perception and Tracking Module is a C++ software library designed to give autonomous mobile robots 3D situational awareness using only a standard RGB camera. Originally developed for the ENPM700 Software Development course at the University of Maryland, the pipeline fuses YOLOv5 object detection with the Depth Anything monocular depth estimation model. By relying entirely on a monocular video stream, the system tracks multiple people and calculates their relative depth without the cost, weight, or computational overhead of active LiDAR or stereo camera rigs.

[GitHub Repository](https://github.com/GraysonGilbert/ACME_perception_module.git)

<div class="project-figure">
  <img src="/images/projects/monocular-perception-and-tracking/demo_screenshot.png" alt="Monocular 3D perception pipeline detecting humans and estimating metric (X, Y, Z) coordinates" />
  <p class="project-caption">Real-time perception GUI visualization depicting YOLOv5 bounding box detections with person classification confidence scores, overlaid with estimated metric 3D spatial coordinates (X, Y, Z) in meters computed via sensor fusion and pinhole camera back-projection.</p>
</div>

---

## Sensor Fusion & Metric Depth Calibration

The core engineering challenge of this module is translating 2D image-plane data into actual 3D metric coordinates. Since the raw inference is handled by pre-trained models, the C++ pipeline focuses entirely on efficient sensor fusion and spatial back-projection.

### 2D Detection & Depth Estimation:
2D Detection & Depth Estimation: Incoming video frames are processed through the OpenCV DNN module. A YOLOv5 model handles the high-speed human detection and bounding box generation, while the Depth Anything model simultaneously outputs a dense relative depth map of the scene. The two model outputs are then combined to determine the absolute depth of detected objects in a given frame.

### Metric Calibration: 
Because foundation models output arbitrary relative depth values, the system applies a linear calibration mapping. Using reference objects of known dimensions, the module calculates a scale factor and offset, configured directly in `DepthProcessor.hpp`, to convert these relative values into true metric depth in meters.

---

## Software Architecture & Class Design

The library is structured around standard object-oriented principles, using a base class interface to cleanly encapsulate the different neural network inference engines.

<div class="project-figure">
  <img src="/images/projects/monocular-perception-and-tracking/uml.png" alt="UML Class Diagram for ACME Monocular Perception Module" />
  <p class="project-caption">UML Class Diagram showing the hierarchy between the ImageProcessor base class, the specialized model classes, and the top-level Analyzer module.</p>
</div>

### Core Class Structure

- `ImageProcessor`: The abstract base class that defines the core interface for model handling (`load_model()`, `process()`, `render()`). It supports dynamic switching between CPU and CUDA hardware backends.
- `YoloProcessor`: Inherits from `ImageProcessor` to handle YOLOv5-specific operations, including tensor parsing, non-maximum suppression (NMS), and bounding box generation.
- `DepthProcessor`: Inherits from `ImageProcessor` to manage the Depth Anything inference, handling image normalization, metric depth scaling, and colormap generation.
- `Analyzer`: The top-level manager that aggregates the processors. It handles the incoming video streams and outputs the structured 3D spatial coordinate vectors required by downstream navigation nodes.

---

## Development & Testing Pipeline

The library is supported by a standard C++ testing and continuous integration setup to keep the codebase maintainable and catch errors early.

<div class="project-arch-grid">
  <div class="project-arch-card">
    <div class="project-arch-core">Architecture</div>
    <div class="project-arch-title">C++ & Dependencies</div>
    <ul class="project-arch-list">
      <li>Compiled as a reusable C++ library for easy integration into existing robotics stacks</li>
      <li>Built against OpenCV 4.10 to leverage the DNN ONNX runtime and CUDA acceleration</li>
      <li>Uses Git LFS to manage the binary neural network weights (.onnx)</li>
    </ul>
  </div>
  <div class="project-arch-card">
    <div class="project-arch-core">Tooling</div>
    <div class="project-arch-title">Testing & CI/CD</div>
    <ul class="project-arch-list">
      <li>Configured via modern CMake for multi-target building</li>
      <li>Unit-tested using GTest to verify math transformations and model inference</li>
      <li>Statically analyzed with Cppcheck to catch memory issues and enforce Google C++ style</li>
      <li>Automated Doxygen API documentation and gcovr/lcov code coverage reporting</li>
    </ul>
  </div>
</div>