---
title: "Project MARS: Multi-Agent Robotic SLAM"
date: 2025-08-01
tags: ["Robotics", "ROS 2", "SLAM", "C++", "Python", "Autonomous Systems"]
summary: "Scalable multi-robot mapping and autonomous exploration system in ROS 2 Humble and Webots. Orchestrates a TurtleBot3 fleet using slam_toolbox, REP-105 compliant TF trees, deterministic occupancy-grid fusion in mars_overseer, and an automated TDD CI/CD pipeline."
cover:
  image: "images/projects/mars/2-robot-slam.png"
  alt: "Project MARS Multi-Agent Robotic SLAM in Webots and RViz2"
  hiddenInSingle: true
weight: 3
---

Project MARS (Multi-Agent Robotic SLAM) is a facility-scale digital twin generation and autonomous mapping system engineered in ROS 2 Humble and Webots. By orchestrating a distributed fleet of TurtleBot3 Burger mobile robots, Project MARS achieves decentralized per-robot local mapping coupled with centralized probabilistic map fusion—reconstructing complex indoor architectural environments with high fidelity and zero inter-robot interference.

<div class="project-stats">
  <div class="project-stat">
    <span class="project-stat-value">ROS 2</span>
    <span class="project-stat-label">Humble Middleware</span>
  </div>
  <div class="project-stat">
    <span class="project-stat-value">Webots</span>
    <span class="project-stat-label">Multi-Robot Physics</span>
  </div>
  <div class="project-stat">
    <span class="project-stat-value">SE(2)</span>
    <span class="project-stat-label">Deterministic Fusion</span>
  </div>
  <div class="project-stat">
    <span class="project-stat-value">100%</span>
    <span class="project-stat-label">TDD & CI/CD Coverage</span>
  </div>
</div>

<div class="project-video-grid">
  <div class="project-video">
    <iframe
      src="https://www.youtube.com/embed/BxGX-qR2iZE"
      title="Project MARS - 1-Robot SLAM Execution"
      frameborder="0"
      allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
      allowfullscreen>
    </iframe>
  </div>
  <div class="project-video">
    <iframe
      src="https://www.youtube.com/embed/aSOs7f2JbLM"
      title="Project MARS - Multi-Robot Fleet SLAM Execution"
      frameborder="0"
      allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
      allowfullscreen>
    </iframe>
  </div>
</div>

---

## Core Architecture & Middleware Specification Matrix

The system architecture relies on a decoupled, modular ROS 2 package hierarchy communicating over FastDDS middleware. Each layer is engineered for strict separation of concerns, ensuring deterministic execution and real-time performance across simulation nodes.

| Subsystem / Layer | Technology Stack | Architectural Responsibility |
| :--- | :--- | :--- |
| **Robotics Middleware** | ROS 2 Humble (C++ / Python) | Distributed DDS communication, namespaced topics & TF trees |
| **Simulation Environment** | Webots Multi-Robot Platform | Physics-accurate indoor facility modeling & sensor simulation |
| **Local Mapping Engine** | slam_toolbox (Synchronous) | Per-robot 2D graph-based SLAM & occupancy grid estimation |
| **Robot Hardware Model** | TurtleBot3 Burger Fleet | Differential-drive kinematic platform with 360° planar LiDAR |
| **Verification & Testing** | GoogleTest + Catch2 | Algorithmic unit testing and multi-node integration test harnesses |
| **CI/CD Pipeline** | GitHub Actions + CodeCov | Automated builds, regression testing, and code coverage metrics |

---

## ROS 2 Fleet Package Architecture Grid

Project MARS is structured into four core ROS 2 packages, establishing modular boundaries between simulation bringup, per-robot SLAM execution, global map fusion, and autonomous navigation planning.

<div class="project-arch-grid">
  <div class="project-arch-card">
    <div class="project-arch-core">Package: mars_fleet_bringup</div>
    <div class="project-arch-title">Simulation & Fleet Orchestration</div>
    <ul class="project-arch-list">
      <li>Launches Webots multi-robot simulation environments</li>
      <li>Spawns namespaced robot instances with independent configurations</li>
      <li>Initializes synchronized RViz2 visualization telemetry</li>
    </ul>
  </div>
  <div class="project-arch-card">
    <div class="project-arch-core">Package: mars_robot_slam</div>
    <div class="project-arch-title">Per-Robot Local SLAM Stack</div>
    <ul class="project-arch-list">
      <li>REP-105 compliant TF tree (map → odom → base_footprint → base_scan)</li>
      <li>Custom LiDAR frame header correction resolving Webots timestamp bugs</li>
      <li>Local synchronous 2D mapping via slam_toolbox</li>
    </ul>
  </div>
</div>
<div class="project-arch-grid">
  <div class="project-arch-card">
    <div class="project-arch-core">Package: mars_overseer</div>
    <div class="project-arch-title">Global Map Fusion Engine</div>
    <ul class="project-arch-list">
      <li>Centralized subscription to per-robot occupancy grid map topics</li>
      <li>Rigid-body spatial transformation aligning known initial poses</li>
      <li>Deterministic multi-grid cell probability fusion into unified /global_map</li>
    </ul>
  </div>
  <div class="project-arch-card">
    <div class="project-arch-core">Package: mars_exploration</div>
    <div class="project-arch-title">Autonomous Fleet Navigation</div>
    <ul class="project-arch-list">
      <li>Sector-based spatial partitioning to eliminate inter-robot collision risk</li>
      <li>Automated waypoint path sequencing for rapid environment coverage</li>
      <li>Modular architecture supporting frontier-based exploration fallbacks</li>
    </ul>
  </div>
</div>

---

## Distributed Coordinate Frames & Frame Header Bug Resolution

To operate multiple robots within a shared workspace without catastrophic parameter or topic collisions, Project MARS enforces rigorous ROS 2 namespacing (`/robot1`, `/robot2`). Each robot namespace encapsulates its own sensor drivers, odometry publishers, and local costmaps.

### REP-105 Compliance & TF Tree Architecture

All transform trees adhere strictly to ROS REP-105 standards, maintaining the kinematic chain:
`map` → `odom` → `base_footprint` → `base_scan`

Maintaining this strict hierarchical separation ensures that odometric drift estimated by `slam_toolbox` is cleanly isolated to the global-to-odometry map frame correction, while wheel odometry handles high-frequency base stabilization.

### Debugging the Webots LiDAR Frame Header Bug

During initial multi-node bringup, `slam_toolbox` frequently suffered catastrophic lookup failures and dropped laser scan messages, outputting TF extrapolation warnings (`Lookup would require extrapolation into the future`). 

**Root Cause Analysis:** Webots sensor message publishers generated LiDAR scans with uninitialized or non-monotonic system timestamps and inconsistent `frame_id` bindings. When `slam_toolbox` attempted to query the TF buffer at the exact scan acquisition timestamp, the time delta between the Webots message header stamp and the ROS system clock caused transform lookups to fail.

**Engineering Solution:** We authored a custom message filter and timestamp re-stamping proxy node inserted between the Webots bridge and `slam_toolbox`. This node intercepts raw `sensor_msgs/LaserScan` messages, overwrites the header timestamp with the current synchronized ROS system time (`this->now()`), ensures correct namespaced `frame_id` attachment, and republishes the stream. This eliminated TF extrapolation exceptions and restored robust real-time SLAM processing.

<div class="project-figure">
  <img src="/images/projects/mars/rosgraph.png" alt="ROS 2 Computation Graph for Project MARS multi-agent system" />
  <p class="project-caption">ROS 2 computation graph illustrating decoupled namespaced nodes communicating through FastDDS and converging into the centralized mars_overseer fusion engine.</p>
</div>

---

## Deterministic Occupancy-Grid Fusion (mars_overseer)

While decentralized per-robot SLAM (`slam_toolbox`) provides high-frequency local trajectory tracking, generating a unified facility map requires aggregating individual occupancy grids into a single global representation without introducing ghost artifacts or blurring walls.

### SE(2) Grid Alignment

<div class="project-math">
$$T_i = \begin{bmatrix} \cos\theta_i & -\sin\theta_i & x_i \\ \sin\theta_i & \cos\theta_i & y_i \\ 0 & 0 & 1 \end{bmatrix}$$
</div>

Using this spatial transformation, incoming costmaps from `/robot_n/map` are re-projected into the global reference coordinate frame before cell-by-cell evaluation.

### Bayesian Probability Updates

Overlapping observations from multiple robots are fused using log-odds occupancy grid updating. Rather than performing naive overwrites, individual cell log-odds values $L(m_x \mid z_{1:t})$ are accumulated across sensor streams:

<div class="project-math">
$$L(t) = L(t-1) + \operatorname{inv\_sensor\_model}(z_t, x_t) - L_0$$
</div>

This probabilistic formulation ensures that regions observed by multiple robots reinforce confidence in obstacle boundaries while rapidly clearing out dynamic or transient sensor noise.

---

## DevOps, Test-Driven Development & Continuous Integration

Project MARS was developed under a rigorous professional software engineering lifecycle, ensuring absolute code quality, test coverage, and maintainability.

- **Agile Iterative Process (AIP):** Structured around strict 1-week sprint cycles, detailed sprint backlogs, feature branch workflows, and collaborative pair programming sessions to maintain architectural alignment.
- **Test-Driven Development (TDD):** Adhered strictly to a red-green-refactor cycle. Algorithmic components and mathematical transformations were unit-tested using **GoogleTest**, while multi-node communication pipelines and lifecycle bringup sequences were validated via integration test harnesses built with **Catch2**.
- **Continuous Integration (CI/CD):** Configured automated **GitHub Actions** workflows executing `colcon build` and `colcon test` across every pull request. The CI pipeline enforces zero-regression policies and automatically uploads test coverage reports to **CodeCov**.
- **Documentation Target:** Authored a dedicated CMake/colcon build target for local Doxygen API documentation generation, enabling developers to inspect annotated class hierarchies, message interfaces, and subsystem APIs offline.
