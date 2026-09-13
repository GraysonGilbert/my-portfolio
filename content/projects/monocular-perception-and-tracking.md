---
title: "ACME Monocular Perception & 3D Spatial Tracking Module"
date: 2024-10-15
tags: ["Computer Vision", "C++", "YOLOv5", "Depth Anything", "OpenCV", "Machine Learning"]
summary: "Engineered a production-grade C++ monocular perception library for ACME Robotics fusing YOLOv5 2D object detection with the 'Depth Anything' foundation model. Delivers real-time 3D human obstacle detection and metric spatial localization from a single RGB camera stream, supported by a comprehensive CMake, CTest, Cppcheck, and Doxygen CI/CD suite."
cover:
  image: "images/projects/monocular-perception-and-tracking/demo_screenshot.png"
  alt: "Monocular 3D perception pipeline detecting humans and estimating metric (X, Y, Z) coordinates"
  hiddenInSingle: true
weight: 4
---

The ACME Monocular Perception & 3D Spatial Tracking Module was engineered as the comprehensive midterm project for the ENPM700 Software Development course at the University of Maryland. Designed specifically for ACME Robotics, this software library solves the complex multi-person (\(N \ge 1\)) obstacle detection and spatial tracking problem using exclusively a single monocular video stream. By eliminating the cost, weight, and computational complexity of active LiDAR sensors or stereo camera rigs, this solution provides autonomous mobile robots with robust 3D situational awareness from standard RGB cameras.

<div class="project-stats">
  <div class="project-stat"><span class="project-stat-value">YOLOv5 + Depth Anything</span><span class="project-stat-label">Dual ONNX Neural Engines</span></div>
  <div class="project-stat"><span class="project-stat-value">Monocular 3D</span><span class="project-stat-label">Metric Spatial Localization</span></div>
  <div class="project-stat"><span class="project-stat-value">C++ / OpenCV 4.10</span><span class="project-stat-label">Modular Shared Library</span></div>
  <div class="project-stat"><span class="project-stat-value">CTest & Cppcheck</span><span class="project-stat-label">Automated QA & CI/CD</span></div>
</div>

<div class="project-figure">
  <img src="/images/projects/monocular-perception-and-tracking/demo_screenshot.png" alt="Monocular 3D perception pipeline detecting humans and estimating metric (X, Y, Z) coordinates" />
  <p class="project-caption">Real-time perception GUI visualization depicting YOLOv5 bounding box detections with person classification confidence scores, overlaid with estimated metric 3D spatial coordinates (X, Y, Z) in meters computed via sensor fusion and pinhole camera back-projection.</p>
</div>

---

## Sensor Fusion & Metric Depth Calibration

To bridge the gap between 2D image-plane detections and 3D robot navigation frames, the perception pipeline combines object detection with monocular depth estimation and rigorous geometric calibration.

### 2D Detection & Bounding Box Generation
Incoming video frames are processed by an optimized YOLOv5 ONNX model running through the OpenCV DNN module. The network performs high-speed inference to detect human subjects, outputting bounding box coordinates \([u_{min}, v_{min}, u_{max}, v_{max}]\) corresponding to pixel boundaries on the image plane.

### Monocular Depth Estimation
Simultaneously, the frame is evaluated by the Depth Anything foundation model. This neural network generates dense relative depth maps across the entire scene, capturing fine-grained spatial gradients and surface structures without requiring infrared projectors or stereo baselines.

### Metric Scale & Offset Calibration
Because foundation depth models output arbitrary relative depth values (\(d_{rel}\)), the system applies a linear calibration mapping to convert relative values into metric depth (\(Z\) in meters). Calibration parameters (\(\alpha, \beta\)) are empirically derived using reference objects of known dimensions and configured directly in `DepthProcessor.hpp`:
<div class="project-math">
$$Z = \alpha \cdot d_{rel} + \beta$$
</div>

### 3D Spatial Back-Projection
Using pinhole camera projection geometry, the 2D bounding box centroid \((u, v)\) and metric depth \(Z\) are transformed into the robot camera coordinate frame \((X, Y, Z)\):
<div class="project-math">
$$X = \frac{(u - c_x) \cdot Z}{f_x}, \quad Y = \frac{(v - c_y) \cdot Z}{f_y}$$
</div>
where \((f_x, f_y)\) and \((c_x, c_y)\) represent the calibrated camera focal lengths and principal point offsets, yielding metric obstacle positions suitable for collision avoidance and path planning.

---

## Object-Oriented Architecture & UML Class Design

The software architecture follows strict object-oriented design principles, utilizing abstract base classes, polymorphic inheritance, and high-level architectural facades to encapsulate neural network inference engines.

<div class="project-figure">
  <img src="/images/projects/monocular-perception-and-tracking/uml.png" alt="UML Class Diagram for ACME Monocular Perception Module" />
  <p class="project-caption">UML Class Diagram illustrating the polymorphic inheritance hierarchy between the ImageProcessor base class, specialized YoloProcessor and DepthProcessor derived classes, and the top-level Analyzer facade orchestrating the perception pipeline.</p>
</div>

### Detailed Class Breakdown

- `ImageProcessor` Base Class: Serves as the polymorphic interface defining core virtual methods including `load_model()`, `process()`, `render()`, and `set_backend()`, supporting seamless switching between CPU and CUDA hardware acceleration.
- `YoloProcessor`: Inherits from `ImageProcessor` to specialize in YOLOv5 tensor parsing, anchor decoding, non-maximum suppression (NMS), and bounding box rendering.
- `DepthProcessor`: Inherits from `ImageProcessor` to execute Depth Anything inference, image normalization, colormap generation, and metric depth scaling.
- `Analyzer` Facade: Acts as the top-level orchestrator that aggregates instances of `YoloProcessor` and `DepthProcessor`, manages video streams (`load_video`, `load_camera`), and outputs structured 3D spatial coordinate vectors (`std::vector<std::pair<float, float>>` / obstacle structures) for downstream navigation nodes.

---

## Software Engineering, Testing & CI/CD Pipeline

To ensure production-grade reliability, memory safety, and maintainability, the library is backed by a rigorous development and quality assurance toolchain.

<div class="project-arch-grid">
  <div class="project-arch-card">
    <div class="project-arch-core">Modular Library & Assets</div>
    <div class="project-arch-title">C++ Architecture & Dependencies</div>
    <ul class="project-arch-list">
      <li>Compiled as a reusable C++ shared/static library for direct integration into robotics stacks</li>
      <li>Custom OpenCV 4.10 build incorporating DNN ONNX runtime and CUDA acceleration support</li>
      <li>Git Large File Storage (Git LFS) managing binary neural network weights (.onnx)</li>
    </ul>
  </div>
  <div class="project-arch-card">
    <div class="project-arch-core">DevOps & QA Suite</div>
    <div class="project-arch-title">Testing, Static Analysis & CI/CD</div>
    <ul class="project-arch-list">
      <li>Modern CMake multi-target build configuration ensuring rapid compilation</li>
      <li>CTest automated unit test framework verifying inference and math calculations</li>
      <li>Cppcheck static analysis enforcing Google C++ guidelines and preventing memory leaks</li>
      <li>Doxygen documentation generation coupled with gcovr/lcov code coverage metrics</li>
    </ul>
  </div>
</div>
